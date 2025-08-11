
tutorial postgres: https://github.com/golang-migrate/migrate/blob/master/database/postgres/TUTORIAL.md

##### Зачем нужен:

- **📁 Управление миграциями**  
	Автоматически применяет (`up`) и откатывает (`down`) SQL-файлы по версии и порядку.

- **🔢 Версионирование схемы БД**  
    Следит за текущей миграцией в специальной таблице в БД (`schema_migrations`), не даёт повторно запускать миграции.

- **🔄 Откат миграций**  
    Позволяет безопасно вернуться на любую предыдущую версию (команда `down`, `force`, `goto` и т.д.).

- **📦 Интеграция с CI/CD и Go-кодом**  
    Можно использовать CLI или Go API — удобно подключать в деплой/тесты.

- **🧪 Предсказуемость и стабильность**  
    Миграции применяются строго в указанном порядке, исключается ручной хаос.

- **⚙️ Поддержка разных источников**  
    Миграции можно запускать: локально, через HTTP, через файловые системы, с AWS S3 и т.д.

- **🧰 Поддержка многих СУБД**  
    Работает с PostgreSQL, MySQL, SQLite, MSSQL и др.

### Workshop

```
brew install golang-migrate
```

```sh
migrate create -ext sql -dir db/migrations -seq create_users_table
```

Новые файлы в `db/migrations`:
- `000001_create_users_table.down.sql`
- `000001_create_users_table.up.sql`

Напишем для `up.sql`:
```sql
CREATE TABLE IF NOT EXISTS users(
	user_id serial PRIMARY KEY,
	username VARCHAR (50) UNIQUE NOT NULL,
	password VARCHAR (50) NOT NULL,
	email VARCHAR (300) UNIQUE NOT NULL 
);
```

В `down.sql`:
```sql
DROP TABLE IF EXISTS users;
```

Запуск миграции командой:
```sh
migrate -path db/migrations \
  -database "postgres://myuser:mypass@localhost:5432/mydb?sslmode=disable" \
  up
```

