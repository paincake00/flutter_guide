
Timeouts позволяют ограничивать время выполнения асинхронной операции (например, слишком долгий ответ от API).

```run-go
package main

import (
	"fmt"
	"time"
)

func main() {
	c1 := make(chan string, 1)
	go func() {
		time.Sleep(2 * time.Second)
		c1 <- "result 1"
	} ()
	
	select {
		case res := <-c1:
			fmt.Println(res)
		case <-time.After(time.Second):
			fmt.Println("timeout 1")
	}
	
	c2 := make(chan string, 1)
	go func() {
		time.Sleep(2 * time.Second)
		c2 <- "result 2"
	} ()
	
	select {
		case res := <-c2:
			fmt.Println(res)
		case <-time.After(3 * time.Second):
			fmt.Println("timeout 2")
	}
}
```
