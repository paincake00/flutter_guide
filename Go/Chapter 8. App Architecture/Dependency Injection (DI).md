
Source: https://golangforall.com/ru/post/dependency-injection.html

**Есть несколько вариантов реализации DI в Go:**

### Singleton

Получение зависимости синглтона подключения к БД, которая может использовать другие зависимости (например, Config):
```go
package db

var db *sql.DB

func GetDB(cfg *config.Config) (*sql.DB, error) {
	if db != nil {
		return db, nil
	}
	
	var err error
	db, err = sql.Open("postgres", fmt.Sprintf(
		"host=%s port=%s user=%s dbname=%s sslmode=disable password=%s",
		configStruct.DbHost, 
		configStruct.DbPort, 
		configStruct.DbUser, 
		configStruct.DbName, 
		configStruct.DbPass,
	)
	if err != nil {
		return nil, err
	}
	return db, nil
}
```

### Multiton

Multiton, или пул синглотов, может помочь для выбора конкретного из пула подключений к БД (например, несколько БД):

```go
package db

var pool *DBpool

type DBpool struct {
	lock sync.Mutex
	pool map[string]*sql.DB
}

func init() {
	if pool == nil {
		pool := &DBpool{
			pool: make(map[string]*sql.DB)
		}
	}
}

func GetDB(dbAlias string, cfg *config.Config) (*sql.DB, error) {
	pool.lock.Lock()
	defer pool.lock.Unlock()
	
	db, ok := pool[dbAlias]
	if ok {
		return db, nil
	}
	
	var err error
	db, err = sql.Open("postgres", fmt.Sprintf(
		"host=%s port=%s user=%s dbname=%s sslmode=disable password=%s",
		configStruct[dbAlias].DbHost, 
		configStruct[dbAlias].DbPort, 
		configStruct[dbAlias].DbUser, 
		configStruct[dbAlias].DbName, 
		configStruct[dbAlias].DbPass)
	if err != nil {
		return nil, err
	}
	
	pool[dbAlias] = db
	
	return db, nil
}
```


### Ручной DI (hand-wired)

#### Контейнер (injector)

Контейнер берет на себя функции **создания, хранения и получения** сущностей, это помогает описывать код создания каждой сущности единожды.

При использовании контейнера, зачастую программирование перетекает в _конфигурирование_ (например, XML или YAML документ со структурой зависимостей).

Вызвать экспортируемый App можно в любом месте через создание нового метода для него через `(a *application.App)`.

```go
package main

import (
    "fmt"
    "log"
    "net/http"
)

type App struct {
    Logger *log.Logger
    Mailer Mailer
    Store  PostStore
}

func (a *App) HandleStorePost(w http.ResponseWriter, r *http.Request) {
    a.Logger.Println("Handling store post...")
    post := Post{Title: "Go DI"}
    if err := a.Store.Save(post); err != nil {
        http.Error(w, "failed to save post", http.StatusInternalServerError)
        return
    }
    a.Mailer.Send("admin@example.com", "New post stored!")
    fmt.Fprintln(w, "Post stored successfully")
}

func main() {
    app := &App{
        Logger: log.Default(),
        Mailer: Mailer{},
        Store:  PostStore{},
    }
    http.HandleFunc("/posts", app.HandleStorePost)
    http.ListenAndServe(":8080", nil)
}

// Dummy implementations
type Post struct{ Title string }
type Mailer struct{}
func (m Mailer) Send(to, body string) { fmt.Println("Mail sent to", to) }
type PostStore struct{}
func (s PostStore) Save(p Post) error { fmt.Println("Post saved:", p.Title); return nil }
```

#### Explicit dependency passing

Функции явно принимают только нужные зависимости (это проще для использования в маленьких проектах).

Но в отличие от контейнера зависимости придется явно "тянуть" через поля структур и аргументы функций из места, где проинициализированы зависимости (обычно в `main`).

```go
package main

import (
    "fmt"
    "log"
    "net/http"
)

func HandleStorePost(
    logger *log.Logger,
    mailer Mailer,
    store PostStore,
) http.HandlerFunc {
    return func(w http.ResponseWriter, r *http.Request) {
        logger.Println("Handling store post...")
        post := Post{Title: "Go DI"}
        if err := store.Save(post); err != nil {
            http.Error(w, "failed to save post", http.StatusInternalServerError)
            return
        }
        mailer.Send("admin@example.com", "New post stored!")
        fmt.Fprintln(w, "Post stored successfully")
    }
}

func main() {
    logger := log.Default()
    mailer := Mailer{}
    store := PostStore{}

    http.HandleFunc("/posts", HandleStorePost(logger, mailer, store))
    http.ListenAndServe(":8080", nil)
}

// Dummy implementations
type Post struct{ Title string }
type Mailer struct{}
func (m Mailer) Send(to, body string) { fmt.Println("Mail sent to", to) }
type PostStore struct{}
func (s PostStore) Save(p Post) error { fmt.Println("Post saved:", p.Title); return nil }

```

### DI with libs & frameworks

**В GO существует несколько библиотек, реализующих по-разному функционал контейнера зависимостей:**

#### uber-go/dig

[dig](https://github.com/uber-go/dig) позволяет сконфигурировать контейнер с помощью анонимных функций на GO, которые могут возвращать саму зависимость или ошибку, и использует `reflect`:

```go
package main

import (
	"encoding/json"
	"go.uber.org/dig"
	"log"
	"os"
)

type Config struct {
	Prefix string
}

func main() {
	// создаем контейнер
	c := dig.New()
	
	// зависимость Config
	err := c.Provide(func() (*Config, error) {
		// в реальном проекте Config может читаться из файла
		var cfg Config
		// такие кавычки нужны, чтобы символы в строке не экранировались
		err := json.Unmarshal([]byte(`{"prefix": "[foo] "}`), &cfg)
		return &cfg, err
	})
	if err != nil {
		panic(err)
	}
	
	// Строим зависимость Logger с использованием Config
	err = c.Provide(func(cfg *Config) *log.Logger {
		return log.New(os.Stdout, cfg.Prefix, 0)
	})
	if err != nil {
		panic(err)
	}
	
	// Получаем Logger, но сначала создаем зависимость Config
	err = c.Invoke(func(l *log.Logger) {
		l.Print("Logger вызван!")
	})
	if err != nil {
		panic(err)
	}
}
```

Зависимости определяются по возвращаемому значению анонимных функций, которые указывались в `c.Provide`, поэтому есть методы как давать имена для зависимостей с одним и тем же типом и как их получать:
```go
package main

import (
	"fmt"
	"go.uber.org/dig"
)
  
func main() {
	c := dig.New()
	
	c.Provide(func() string {
		// какоя-то зависимость 1
		return "username"
	}, dig.Name("username"))
	c.Provide(func() string {
		// какоя-то зависимость 2
		return "password"
	}, dig.Name("password"))
	  
	// p - это структура с аннотациями
	err := c.Invoke(func(p struct {
		dig.In
		
		// имя одной зависимости, которая возвращет string
		U string `name:"username"`
		// имя другой зависимости, которая возвращает string
		P string `name:"password"` 
	}) {
		fmt.Println("user >>>", p.U)
		fmt.Println("pwd >>>", p.P)
	})
	if err != nil {
		panic(err)
	}
}
```

#### elliotchance/dingo

Необходимо в YAML описать все зависимости, после чего сгенерировать файл контейнера.

```yaml
services:
  Config:
    type: '*Config'
    error: return nil
    returns: NewConfig()
  Logger:
    type: '*log.Logger'
    import:
      — 'os'
    returns: log.New(os.Stdout, @{Config}.Prefix, 0)
```

Для каждой сущности придется указать следующие параметры:

- `imports` — импорты, которые надо добавить в сгенерированный GO файл;
- `error` — фрагмент GO-кода, который надо выполнить при проверке ошибки;
- `returns` — фрагмент GO-кода, который инициализирует и вернет нашу сущность;

Вынесена логика для создания Config в функцию `NewConfig`:
```go
package main

import "encoding/json"

type Config struct {
	Prefix string
}

func NewConfig() (*Config, error) {
	var cfg Config
	err := json.Unmarshal([]byte(`{"prefix": "[foo] "}`), &cfg)
	return &cfg, err
}

func main() {
	DefaultContainer.GetLogger().Println("ok")
}
```

Для генерации необходимо запустить `go get -u github.com/elliotchance/dingo; dingo` в папке проекта. Создается сгенерированный файл `dingo.go`:

```go
// Code generated by dingo; DO NOT EDIT
package main

import (
	log "log"
	"os"
)

type Container struct {
	Config *Config
	Logger *log.Logger
}

var DefaultContainer = NewContainer()

func NewContainer() *Container {
	return &Container{}
}

func (container *Container) GetConfig() *Config {
	if container.Config == nil {
		service, err := NewConfig()
		if err != nil {
			return nil
		}
		container.Config = service
	}
	return container.Config
}

func (container *Container) GetLogger() *log.Logger {
	if container.Logger == nil {
		service := log.New(os.Stdout, container.GetConfig().Prefix, 0)
		container.Logger = service
	}
	return container.Logger
}
```

#### sarulabs/di

[sarulabs/di](https://github.com/sarulabs/di) имеет сходства с `dig`, однако не использует `reflect`. Все зависимости в `di` должны иметь уникальные имена:
```go
package main

import (
	"encoding/json"
	"fmt"
	"log"
	"os"

	"github.com/sarulabs/di"
)

type Config struct {
	Prefix string
}

func main() {
	builder, err := di.NewBuilder()
	if err != nil {
		panic(err)
	}

	err = builder.Add(di.Def{
		Name: "config",
		Build: func(ctn di.Container) (interface{}, error) {
			var cfg Config
			err := json.Unmarshal([]byte(`{"prefix": "[foo] "}`), &cfg)
			return &cfg, err
		},
	})
	if err != nil {
		panic(err)
	}

	err = builder.Add(di.Def{
		Name: "logger",
		Build: func(ctn di.Container) (interface{}, error) {
			var cfg *Config
			err = ctn.Fill("config", &cfg)
			return log.New(os.Stdout, cfg.Prefix, 0), nil
		},
		// DeleteWithSubContainers - вызывает код из Close
		// при уничтожении контейнера 
		Close: func(obj interface{}) error {
			if _, ok := obj.(*log.Logger); ok {
				fmt.Println("logger close")
			}
			return nil
		},
	})
	if err != nil {
		panic(err)
	}

	container := builder.Build()

	var l *log.Logger
	err = container.Fill("logger", &l)
	if err != nil {
		panic(err)
	}

	l.Println("ok")

	err = container.DeleteWithSubContainers()
	if err != nil {
		panic(err)
	}

}
```

#### google/wire

C [wire](https://github.com/google/wire) мы должны написать шаблон функции-конструктора каждой сущности с использованием вызова `wire.Build`. 

В начало файлов с такими шаблонами необходимо добавить комментарий:
`//+build wireinject`.

Запуск `go get github.com/google/wire/cmd/wire; wire` сгенерирует файл `*_gen.go`. 
Шаблон конструктора логгера с конфигом добавляем:
```go
//+build wireinject

package main

import (
	"log"

	"github.com/google/wire"
)

// Шаблон для генерации 
func GetLogger() (*log.Logger, error) {
	panic(wire.Build(NewLogger, NewConfig))
}
```

Сгенерированный код добавляется из файла `*_gen.go` выглядит так:
```go
import (
	"log"
)

// Injectors from wire.go:

func GetLogger() (*log.Logger, error) {
	config, err := NewConfig()
	if err != nil {
		return nil, err
	}
	logger := NewLogger(config)
	return logger, nil
}
```