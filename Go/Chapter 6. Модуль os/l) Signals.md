
## Что такое сигналы

Сигналы (signals) - это программные прерывания, отправляемые процессами операционной системы или другими процессами. Они представляют собой асинхронный механизм уведомления процессов о различных событиях.

## Как работают сигналы в Go

Go предоставляет пакет `os/signal` для работы с сигналами. Этот пакет позволяет:

1. Подписываться на сигналы
2. Обрабатывать полученные сигналы
3. Игнорировать определенные сигналы
4. Отправлять сигналы другим процессам

## Распространенные сигналы

```
SIGINT  (2)  - прерывание с терминала (Ctrl+C)
SIGTERM (15) - сигнал завершения (используется при graceful shutdown)
SIGKILL (9)  - немедленное завершение (не может быть перехвачен)
SIGHUP  (1)  - отключение терминала (часто используется для перезагрузки конфигурации)
SIGUSR1/2    - пользовательские сигналы (для определенных приложением действий)
```

## Простой пример
```go
package main

import (
    "fmt"
    "os"
    "os/signal"
    "syscall"
)

func main() {

    sigs := make(chan os.Signal, 1)

    signal.Notify(sigs, syscall.SIGINT, syscall.SIGTERM)

    done := make(chan bool, 1)

    go func() {

        sig := <-sigs
        fmt.Println()
        fmt.Println(sig)
        done <- true
    }()

    fmt.Println("awaiting signal")
    <-done
    fmt.Println("exiting")
}
```

Запуск и завершение через `ctr+c`:
```
& go run main.go

awaiting signal
^C
interrupt
exiting
```

## Пример с graceful shutdown сервера

```go
package main

import (
    "context"
    "fmt"
    "log"
    "net/http"
    "os"
    "os/signal"
    "syscall"
    "time"
)

func main() {
    // Создаем HTTP-сервер
    server := &http.Server{
        Addr: ":8080",
        Handler: http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
            fmt.Fprintln(w, "Hello, World!")
        }),
    }

    // Запускаем сервер в отдельной горутине
    go func() {
        log.Println("Сервер запущен на :8080")
        if err := server.ListenAndServe(); err != http.ErrServerClosed {
            log.Fatalf("Ошибка HTTP-сервера: %v", err)
        }
    }()

    // Канал для получения сигналов
    stop := make(chan os.Signal, 1)
    
    // Уведомлять о SIGINT и SIGTERM
    signal.Notify(stop, syscall.SIGINT, syscall.SIGTERM)
    
    // Блокируем до получения сигнала
    <-stop
    log.Println("Получен сигнал остановки, начинаем graceful shutdown...")

    // Создаем контекст с тайм-аутом для завершения
    ctx, cancel := context.WithTimeout(context.Background(), 10*time.Second)
    defer cancel()

    // Пытаемся корректно завершить HTTP-сервер
    if err := server.Shutdown(ctx); err != nil {
        log.Fatalf("Ошибка при graceful shutdown: %v", err)
    }

    log.Println("Сервер успешно остановлен")
}
```
