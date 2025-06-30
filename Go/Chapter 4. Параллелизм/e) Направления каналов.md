
При передаче каналов в функцию, можно указывать будет ли канал предназначен только для получения или отправки значений:
```run-go
package main
import "fmt"

func ping(pings chan<- string, msg string) {
	pings <- msg
}

func pong(pings <-chan string, pongs chan<- string) {
	msg := <-pings
	pongs <- msg
}

func main() {
	pings := make(chan string, 1)
	pongs := make(chan string, 1)
	ping(pings, "new hit!")
	pong(pings, pongs)
	fmt.Println(<-pongs)
}
```
