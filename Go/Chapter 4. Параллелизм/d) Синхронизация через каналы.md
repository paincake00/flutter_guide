
Каналы можно использовать, чтобы синхронизировать работу горутин (сообщить остальным о завершении выполнения текущей, к примеру).

В коде ниже в канал `done` добавляется значение, что говорит о завершении горутины, чтобы могли принять это другие горутины:
```run-go
package main
import (
	"time"
	"fmt"
)

func worker(done chan bool) {
	fmt.Print("working...")
	time.Sleep(time.Second)
	fmt.Println("done")
	
	done <- true
}

func main() {
	done := make(chan bool, 1)
	go worker(done)

	// если убрать, то main выполнится и не будет ждать worker
	<-done
}
```
