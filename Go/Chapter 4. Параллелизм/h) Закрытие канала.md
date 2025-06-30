
Закрытие канала означает, что на него больше не будет отправляться значения. Это может быть полезно для передачи завершения получателям канала.

```run-go
package main

import "fmt"
import "time"

func main() {
    jobs := make(chan int, 5)
    done := make(chan bool)

    go func() {
        for {
            j, more := <-jobs
            if more {
                fmt.Println("received job", j)
            } else {
                fmt.Println("received all jobs")
                done <- true
                return
            }
        }
    }()

    for j := 1; j <= 3; j++ {
	    time.Sleep(time.Second)
        jobs <- j
        fmt.Println("sent job", j)
    }
    close(jobs) // это делает more==false
    fmt.Println("sent all jobs")

    <-done

    _, ok := <-jobs
    fmt.Println("received more jobs:", ok)
}
```
