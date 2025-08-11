
SQLc - позволяет генерировать на основе готовых SQL-запросов методы для repository-классов на Go (и на других языках тоже) для работы с БД.


Определяем файл `sqlc.yaml`:
```yaml
version: "2"
sql:
	- engine: "postgresql" # или mysql, sqlite и т.д.
	  schema: "./migrations" # путь до схемы БД
	  queries: "query.sql" # файл (или файлы) с нашими SQL запросами
	  gen:
	    go:
	      package: "repository" # имя пакета сгенерированного кода
	      out: "internal/repository" # относительный путь, где будет Go файл 
```

### Query Annotations

 Для каждого отдельного запроса над ним описывается аннотация, исходя из которой sqlc определяет имя метода и его поведение:
 
```
-- name: <name> <command>
```

Можно выделить несколько команд:

##### `:exec`

Генерирует метод, который по сути ничего не возвращает (кроме ошибки):
```sql
-- name: DeleteAuthor :exec
DELETE FROM authors
WHERE id = $1; 
```

```go
func (q *Queries) DeleteAuthor(ctx context.Context, id int64) error {
	// ExecContext - выполняет SQL запрос
	// deleteAuthor - содержит строку с SQL запросом
	_, err := q.db.ExecContext(ctx, deleteAuthor, id)
	return err
}
```

##### `:one`

Генерирует метод, который возвращает лишь одну запись (record) или же одну строку:
```sql
-- name: GetAuthor :one
SELECT * FROM authors
WHERE id = $1 LIMIT 1;
```

```go
func (q *Queries) GetAuthor(ctx context.Context, id int64) (Author, error) {
	row, err := q.db.ExecContext(ctx, getAuthor, id)
	// ...
}
```

##### `:many`

Генерирует метод, который возвращает срез записей (или строк):
```sql
-- name: ListAuthors :many
SELECT * FROM authors
ORDER BY name; 
```

```go
func (q *Queries) ListAuthors(ctx context.Context) ([]Author, error) {
	rows, err := q.db.ExecContext(ctx, listAuthors)
	// ...
}
```

### GEN

Установка утилиты `sqlc` через `brew`:
```
brew install sqlc
```

Запуск генерации на основе описанного `sqlc.yaml`:
```
sqlc generate
```

Будет создано 3 файла:
- `db.go` (реализует доступ к запросам)
- `models.go` (содержит все модели)
- `query.sql.go` (файл с запросами, а именно методами)