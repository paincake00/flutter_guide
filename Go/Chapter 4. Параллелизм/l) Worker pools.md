
**Worker Pool** — это паттерн, который используется для выполнения множества задач параллельно с ограниченным количеством горутин. 

Вместо запуска новой горутины для каждой задачи, мы используем фиксированное количество **рабочих**, которые берут задачи из канала и обрабатывают их.

Вот канал с 5 задачами и 3 воркерами (по сути, столько же и горутин будет создано - 3):
```run-go
package main

import (
	"fmt"
	"time"
)

func worker(id int, jobs <-chan int, results chan<- int) {
	// выполняется пока в канале есть "новая работа"
	for j := range jobs {
		fmt.Println("worker", id, "started  job", j)
        time.Sleep(time.Second)
        fmt.Println("worker", id, "finished job", j)
        results <- j * 2
	}
}

func main() {
	const numJobs = 5
	jobs := make(chan int, numJobs)
	results := make(chan int, numJobs)
	
	for w := 1; w <= 3; w++ {
		go worker(w, jobs, results)
	}
	for j := 1; j <= numJobs; j++ {
		jobs <- j
	}
	close(jobs)
	
	// ожидаем завершения всех горутин
	for a := 1; a <= numJobs; a++ {
		<-results
	}
}
```
