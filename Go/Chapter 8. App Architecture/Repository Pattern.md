
Way to organize and abstract data access logic (business logic) from the application logic.

![[Pasted image 20250718152256.png|500]]

#### Transport Layer

Это слой, отвечающий за внешнее взаимодействие (например, с пользователем или другим сервисом). Он принимает запросы, валидирует их и передает дальше в Service Layer.

Содержит:
- HTTP/gRPC хендлеры
- Middleware (логирование, авторизация, CORS и т.п.)
- Роутеры
- DTO (Data Transfer Objects) для входящих и исходящих данных

```go
// transport/http/handler.go
type UserHandler struct {
	userService service.UserService
}

func (h *UserHandler) GetUser(w http.ResponseWriter, r *http.Request) {
	ctx := r.Context()
	id := chi.URLParam(r, "id")
	user, err := h.userService.GetUser(ctx, id)
	if err != nil {
		http.Error(w, "User not found", http.StatusNotFound)
		return
	}
	json.NewEncoder(w).Encode(user)
} 
```

```go
// transport/http/router.go
func NewRouter(userHandler *UserHandler) http.Handler {
	r := chi.NewRouter()
	r.Get("/user/{id}", userHandler.GetUser)
	return r
}
```

#### Service Layer

Этот слой содержит бизнес-логику приложения. Он инкапсулирует доступ для транспортного слоя к слою данных.

Содержит:
- Бизнес-правила (use cases)
- Сервисы, которые работают с несколькими репозиториями
- Трансформации данных (mappers) между доменными моделями и DTO

```go
// service/user_service.go
type UserService interface {
	GetUser(ctx context.Context, id string) (*User, error)
}

// видна только внутри пакета, т.к. со строчной буквы
type userService struct {
	userRepo repository.UserRepository
}

func (s *userService) GetUser(ctx context.Context, id string) (*User, error) {
	user, err := s.userRepo.FindByID(ctx, id)
	if err != nil {
		return nil, err
	}
	if user.IsBanned {
		return nil, errors.New("user is banned")
	}
	return user, nil
}

func NewUserService(repo repository.UserRepository) UserRepository {
	return &userService{userRepo: repo}
}
```

#### Storage Layer

Это слой, который работает с хранилищами данных (БД, файлы, кэш, сторонние API и др.). Здесь реализуется доступ к данным и они инкапсулируются за интерфейсами (Repository).

Содержит:
- Репозитории (CRUD для доменных сущностей)
- DAO (Data Access Object)
- Мапперы между доменными моделями и схемами БД

```go
// repository/user_repository.go
type UserRepository interface {
	FindByID(ctx context.Context, id string) (*User, error)
}

type userRepository struct {
	db *sql.DB
}

func (r *userRepository) FindByID(ctx context.Context, id string) (*User, error) {
	row := r.db.QueryRowContext(ctx, "SELECT id, name, is_banned FROM users WHERE id = ?", id)
	var user User
	err := row.Scan(&user.ID, &user.Name, &user.IsBanned)
	if err != nil {
		return nil, err
	}
	return &user, nil
}

func NewUserRepository(db *sql.DB) UserRepository {
	return &userRepository{db: db}
}
```

