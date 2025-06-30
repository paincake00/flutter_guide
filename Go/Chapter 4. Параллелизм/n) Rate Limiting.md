
**Rate Limiting (ограничение скорости)** - важный механизм для контроля использования ресурсов и поддержания качества обслуживания от сервиса.

Грубо говоря, это реализация троттлинга (добавление задержки) запросов к сервису.

Это необходимо для защиты сервера от перегрузки и для предотвращения злоупотребления API.

Rate Limiting в Go реализуется с помощью tickers:
```run-go
package main
import (
	"fmt"
	"time"
)

func main() {
	requests := make(chan int, 5)
	for i := 1; i <= 5; i++ {
		requests <- i
	}
	close(requests)
	
	limiter := time.Tick(200 * time.Millisecond)
	
	for req := range requests {
		// задержка в 200мс
		<-limiter
		fmt.Println("request", req, time.Now())
	}
	
	// ограничитель всплеска запросов
	burstyLimiter := make(chan time.Time, 3)
	
	// первые 3 запроса - без ограничителя
	for range 3 {
		burstyLimiter <- time.Now()
	}
	
	go func() {
		// на последующие запросы - задрежка в 200мс
		for t := range time.Tick(200 * time.Millisecond) {
			burstyLimiter <- t
		}
	} ()
	
	burstyReqs := make(chan int, 5)
	for i := 1; i <= 5; i++ {
		burstyReqs <- i
	}
	close(burstyReqs)
	for req := range burstyReqs {
		<-burstyLimiter
		fmt.Println("request", req, time.Now())
	}
}
```
