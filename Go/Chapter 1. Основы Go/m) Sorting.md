
### Built-in sorting of slices

Можно сортировать упорядочиваемые типы: int (все виды), float32, float64, string
```run-go
package main

import (
	"fmt"
	"slices"
)

func main() {
	strs := []string{"n", "a", "r"}
	slices.Sort(strs)
	fmt.Println("Strings: ", strs)
	
	ints := []int{7, 4, 8}
	slices.Sort(ints)
	fmt.Println("Ints: ", ints)
	
	// проверить отсортирован ли уже
	s := slices.IsSorted(strs)
	fmt.Println("Sorted: ", s)
}
```

### Сортировка по компаратору

Можно отсортировать по определенному порядку через функцию (компаратор).

Например, отсортировать строки по длине:
```run-go
package main
import (
	"fmt"
	"slices"
	"cmp"
)

func main() {
	strs := []string{"peach", "banana", "kiwi"}
	
	lenCmp := func(a, b string) int {
		return cmp.Compare(len(a), len(b))
	}
	
	slices.SortFunc(strs, lenCmp)
	fmt.Println(strs)
	
    type Person struct {
        name string
        age  int
    }

    people := []Person{
        Person{name: "Jax", age: 37},
        Person{name: "TJ", age: 25},
        Person{name: "Alex", age: 72},
    }
    
    slices.SortFunc(people, func(a, b Person) int {
	    return cmp.Compare(a.age, b.age)
    })
    fmt.Println(people)
}
```

