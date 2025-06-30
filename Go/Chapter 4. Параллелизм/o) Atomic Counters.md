
**Атомарный (atomic)** — это операция или тип данных, который **гарантирует безопасное выполнение при работе с разделяемой памятью из нескольких горутин** , без необходимости явной блокировки (например, через мьютекс).

Atomic counter - это переменная-счетчик, к которой может обращаться несколько горутин и безопасно (атомарно) изменять ее.

```run-go
package main

import (
	"fmt"
	"sync"
	"sync/atomic"
)

func main() {
	var ops atomic.Uint64 // один из атомарных типов
	
	var wg sync.WaitGroup
	
	for range 50 {
		wg.Add(1)
		
		go func() {
			for range 1000 {
				ops.Add(1)
			}
			wg.Done()
		} ()
	}
	
	wg.Wait()
	
	fmt.Println("counter value: ", ops.Load())
}
```

### Практика: атомарное изменение структуры

```run-go
package main
import (
	"fmt"
	"time"
	"sync"
	"sync/atomic"
)

type User struct {
	Name string
	Age int
}

func main() {
	var user atomic.Value
	
	user.Store(User{Name: "noname", Age: 18})
	
	var wg sync.WaitGroup
	
	for i := 1; i <= 5; i++ {
		wg.Add(1)
		go func(i int) {
			defer wg.Done()
			newUser := User{Name: fmt.Sprintf("User-%d", i), Age: 20+i}
			user.Store(newUser)
			time.Sleep(10 * time.Millisecond)
		} (i)
	}
	
	wg.Wait()
		
	fmt.Println("User Name: ", user.Load().(User).Name, " Age: ", user.Load().(User).Age)
	
	// fmt.Println("Итоговый пользователь:", user.Load().(User))
}
```

`user.Load().(User)` - это явное приведение к типу `User` значения из счетчика.

