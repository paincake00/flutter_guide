
Tickers используются если необходимо делать неоднократно (через интервалы) в будущем.

Создаем тикер, который возвращать значения из канала `С` пока не остановим его:
```run-go
package main

import (
	"fmt"
	"time"
)

func main() {
	ticker := time.NewTicker(500 * time.Millisecond)
	done := make(chan bool)
	
	go func() {
		for {
			select {
				case <-done:
					return
				case t := <-ticker.C:
					fmt.Println("Tick at", t) 
			}
		}
	}()
	
	time.Sleep(1600 * time.Millisecond)
	ticker.Stop()
	done <- true
	fmt.Println("ticker stopped")
}
```
