
Uber Fx - фреймворк для построения приложений на Go, который:
- управляет зависимостями через DI-контейнер
- контролирует жизненный цикл компонентов приложения (запуск, остановка, обработка ошибок)
- избавляет от запутанности в больших сервисах

Позволяет удобно связывать компоненты приложения (сервиса):
- конфиги
- логгеры
- базы данных
- серверы (HTTP/gRPC)
- брокеры сообщений (RabbitMQ) и т.д.

#### Workflow

```go
                   ┌────────────────────────────┐
                   │        fx.New()            │
                   └────────────┬───────────────┘
                                │
                    ┌───────────▼────────────┐
                    │  DI-контейнер (Provide)│
                    └───────────┬────────────┘
                                │
     ┌──────────────────────────▼────────────────────────┐
     │   Создаются зависимости по fx.Provide(...)        │
     │  (инициализация в порядке графа зависимостей)     │
     └──────────────────────────┬────────────────────────┘
                                │
                     ┌──────────▼─────────┐
                     │     fx.Invoke      │
                     └──────────┬─────────┘
                                │
         ┌──────────────────────▼───────────────────────┐
         │   Fx вызывает функции, у которых есть        │
         │     нужные зависимости (handler, init, ...)  │
         └──────────────────────┬───────────────────────┘
                                │
              ┌────────────────▼────────────────┐
              │       fx.Lifecycle (lc)         │
              └────────────────┬────────────────┘
                               │
         ┌─────────────────────▼──────────────────────┐
         │ lc.Append(fx.Hook{                         │
         │   OnStart: запуск сервера/клиента и т.д.   │
         │   OnStop: остановка, закрытие соединений   │
         └─────────────────────┬──────────────────────┘
                               │
                         ┌─────▼─────┐
                         │ fx.Run()  │
                         └─────┬─────┘
                               │
            ┌──────────────────▼───────────────────┐
            │ Все компоненты запускаются по OnStart │
            │ Приложение работает до SIGINT/SIGTERM │
            └──────────────────┬───────────────────┘
                               │
           ┌───────────────────▼───────────────────┐
           │ OnStop вызывается — graceful shutdown │
           └───────────────────────────────────────┘

```

#### Запуск Echo сервера

Происходит:
- Загружает конфиг
- Инициализирует логгер
- Запускает HTTP-сервер (Echo)
- Graceful shutdown при SIGINT/SIGTERM

```go
package main

import (
	"context"
	"fmt"
	"net/http"
	"os"
	"time"

	"go.uber.org/fx"
	"go.uber.org/zap"
	"github.com/labstack/echo/v4"
)

// --- Config ---
type Config struct {
	Port string
}

func NewConfig() *Config {
	return &Config{Port: "8080"}
}

// --- Logger ---
func NewLogger() (*zap.Logger, error) {
	return zap.NewProduction()
}

// --- HTTP Server ---
func NewEchoServer() *echo.Echo {
	e := echo.New()
	e.GET("/", func(c echo.Context) error {
		return c.String(http.StatusOK, "Hello from Uber Fx!")
	})
	return e
}

// --- Run Servers ---
func RunServer(lc fx.Lifecycle, log *zap.Logger, e *echo.Echo, cfg *Config) {
	server := &http.Server{
		Addr:    ":" + cfg.Port,
		Handler: e,
	}

	lc.Append(fx.Hook{
		OnStart: func(ctx context.Context) error {
			log.Info("Starting HTTP server...", zap.String("port", cfg.Port))
			go func() {
				if err := server.ListenAndServe(); err != nil && err != http.ErrServerClosed {
					log.Fatal("Server error", zap.Error(err))
				}
			}()
			return nil
		},
		OnStop: func(ctx context.Context) error {
			log.Info("Shutting down HTTP server...")
			return server.Shutdown(ctx)
		},
	})
}

func main() {
	app := fx.New(
		fx.Options(
			// --- Register dependencies ---
			fx.Provide(
				NewConfig,
				NewLogger,
				NewEchoServer,
			),
	
			// --- Invoke startup ---
			fx.Invoke(RunServer),
		),
	)

	app.Run() // блокирует и ждет сигналов SIGINT/SIGTERM
}
```

Основные методы:

| `fx.Provide`   | Регистрирует конструкторы зависимостей            |
| -------------- | ------------------------------------------------- |
| `fx.Invoke`    | Вызывает функции с уже построенными зависимостями |
| `fx.Lifecycle` | Даёт доступ к управлению жизненным циклом         |
| `fx.Options`   | Группирует модули и зависимости                   |
| `.Run()`       | Запускает приложение и ждёт сигналов              |


