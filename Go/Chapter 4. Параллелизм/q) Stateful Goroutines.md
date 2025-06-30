
Вместо Mutex можно использовать встроенный подход для синхронизации горутин.

**Stateful goroutine** - это горутина, которая хранит и управляет состояние, которое можно безопасно изменять другим горутинам через каналы.

```run-go
package main
import (
	"fmt"
	"math/rand"
	"sync/atomic"
	"time"
)

type readOp struct {
	key int
	resp chan int
}

type writeOp struct {
	key int
	val int
	resp chan bool
}

func main() {
	var readOps uint64
	var writeOps uint64
	
	reads := make(chan readOp)
	writes := make(chan writeOp)
	
	go func() {
		var state = make(map[int]int)
		for {
			select {
				case read := <-reads:
					read.resp <- state[read.key] // отправляем response
				case write := <-writes:
					state[write.key] = write.val
					write.resp <- true // отправляем response
			}
		}
	}()
	
	for range 100 {
		go func() {
			for {
				read := readOp{
					key: rand.Intn(5),
					resp: make(chan int),
				}
				reads <- read // отправляем запрос на чтение state
				<-read.resp // ждем response
				atomic.AddUint64(&readOps, 1)
				time.Sleep(time.Millisecond)
			}
		}()
	}
	
	for range 10 {
		go func() {
			for {
				write := writeOp{
					key: rand.Intn(5),
					val: rand.Intn(100),
					resp: make(chan bool),
				}
				writes <- write
				<-write.resp
				atomic.AddUint64(&writeOps, 1)
				time.Sleep(time.Millisecond)
			}
		}()
	}
	
	time.Sleep(time.Second)
	
	readOpsFinal := atomic.LoadUint64(&readOps)
    fmt.Println("readOps:", readOpsFinal)
    writeOpsFinal := atomic.LoadUint64(&writeOps)
    fmt.Println("writeOps:", writeOpsFinal)
}
```

`readOps` и `writeOps` показывают сколько запросов на чтение/запись было сделано за секунду (time.Second).

