
Иногда нужно выполнить какое-то событие отложено в будущем.

Таймеры представляют собой одно событие в будущем, через заданное время. 
У таймера есть канал, который блокируется пока из этого канала не поступит значение, что таймер сработал.

Почему таймер может быть полезнее, чем простое использование `time.Sleep`, это возможность его досрочно остановить:
```run-go
package main

import (
    "fmt"
    "time"
)

func main() {

    timer1 := time.NewTimer(2 * time.Second)

    <-timer1.C
    fmt.Println("Timer 1 fired")

    timer2 := time.NewTimer(time.Second)
    go func() {
        <-timer2.C
        fmt.Println("Timer 2 fired")
    }()
	// останавливаем timer2
    stop2 := timer2.Stop()
    if stop2 {
        fmt.Println("Timer 2 stopped")
    }

	// ожидаем завершения горутины
    time.Sleep(2 * time.Second)
}
```
