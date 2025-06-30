
select позволяет ожидать выполнения какой-нибудь операции среди множества каналов.

В этом коде select операцию добавления в один из перечисленных каналов:
```run-go
package main

import (
    "fmt"
    "time"
)

func main() {

    c1 := make(chan string)
    c2 := make(chan string)

    go func() {
        time.Sleep(1 * time.Second)
        c1 <- "one"
    }()
    go func() {
        time.Sleep(2 * time.Second)
        c2 <- "two"
    }()

    for range 2 {
        select {
	        case msg1 := <-c1:
	            fmt.Println("received", msg1)
	        case msg2 := <-c2:
	            fmt.Println("received", msg2)
		}
    }
}
```

### Non-Blocking Channel Operations

Чтобы отправка и получение данных через каналы не блокировалась, можно использовать `default`, который вернет результат по умолчанию:

```run-go
package main

import (
    "fmt"
    "time"
)

func receiver(ch chan string) {
    for {
        msg, ok := <-ch
        if !ok {
            fmt.Println("Канал закрыт, завершаем приёмник")
            return
        }
        fmt.Println("Получено сообщение:", msg)
        time.Sleep(2 * time.Second) // имитация долгой обработки
    }
}

func nonBlockingSend(ch chan string, data string) {
    select {
    case ch <- data:
        fmt.Printf("✅ Успешно отправлено: %s\n", data)
    default:
        fmt.Printf("❌ Не удалось отправить: канал занят или заполнен\n")
    }
}

func main() {
    // Создаём буферизованный канал на 2 элемента
    ch := make(chan string, 2)

    // Запускаем горутину-приёмник
    go receiver(ch)

    // Отправляем сообщения с задержкой
    for i := 1; i <= 5; i++ {
        msg := fmt.Sprintf("сообщение #%d", i)
        nonBlockingSend(ch, msg)
        time.Sleep(500 * time.Millisecond) // делаем паузу между попытками
    }

    // Ждём немного, чтобы последнее сообщение успело обработаться
    time.Sleep(3 * time.Second)
    close(ch)

    fmt.Println("Программа завершена.")
}
```
