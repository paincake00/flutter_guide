
Mutex - это примитив синхронизации в Go, который гарантирует, что только одна горутина может выполнять защищённый участок кода в определенный момент времени, а другие пока не могут.

```run-go
package main
import (
	"fmt"
	"sync"
)

type Container struct {
	mu sync.Mutex
	counters map[string]int
}

func (c *Container) inc(name string) {
	c.mu.Lock() // блокируем mutex
	defer c.mu.Unlock() // т.к. defer, то разблокировка будет выполлнена в конце функции (после изменения счетчика)
	c.counters[name]++
}

func main() {
	c := Container{
		counters: map[string]int{"a": 0, "b": 0},
	}
	
	// не WaitGroup, не Mutex не надо инициализировать
	// Они уже работают в своем "нулевом" состоянии
	var wg sync.WaitGroup
	
	doInc := func(name string, n int) {
		for range n {
			c.inc(name)
		}
		wg.Done() // а можно было указать в начале с defer
	}
	
	wg.Add(3)
	go doInc("a", 10000)
	go doInc("a", 10000)
	go doInc("b", 10000)
	
	wg.Wait()
	
	fmt.Println(c.counters)
}
```
