
Горутина - это легкий поток (lightweight thread), управляемый средой выполнения Go (Go runtime).

Функция `main` сама по себе уже является горутиной.

Горутина:
- Обозначается с помощью с ключевого слова `go`
- Выполняется параллельно с другими горутинами
- Может запускаться на разных системных потоках под управлением планировщика (scheduler), встроенного в Go

```run-go
package main
import (
	"time"
	"fmt"
)

func f(from string) {
	for i := range 3 {
		fmt.Println(from, ":", i)
	}
}

func main() {
	// sync func
	f("direct")
	
	// async
	go f("goroutine")
	
	// another async
	go func(msg string) {
		fmt.Println(msg)
	}("going")
	
	// wait when end all goroutines
	time.Sleep(time.Second)
	fmt.Println("finish") 
}
```

