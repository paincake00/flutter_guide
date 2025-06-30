
Обобщенные типы (или Generics) в Go состоят из параметра типа (`T`) и ограничения (например, `any`):

```go
func Print[T any](s []T) {
	for _, v := range s {
		fmt.Println(v)
	}
}
```

```run-go
package main
import "fmt"

func Sum[T int | float64](a, b T) T {
	return a + b
}

func main() {
	fmt.Println(Sum(4, 5))
	fmt.Println(Sum(2.3, 9))
}
```

Таблица ограничений:

| any                 | Любой тип (interface{} под капотом)                                                                               |
| ------------------- | ----------------------------------------------------------------------------------------------------------------- |
| comparable          | Типы, которые можно сравнивать через `==` и `!=`                                                                  |
| constraints.Ordered | Можно сравнивать через `>`, `<`, `>=`, `<=` (внешняя библиотека)                                                  |
| int, float64 и т.д. | Конкретные типы, которые можно перечислить через "\|"                                                             |
| ~int, ~[]E          | Принимать типы, которые могут быть обозначены через псевдонимы (`type MyInt int`, `type MySlice []string` и т.д.) |
| MyInterface         | Принимать только типы, реализующие указанный интерфейс                                                            |
#### comparable:
```run-go
package main
import "fmt"

func Find[T comparable](slice []T, value T) int {
    for i, v := range slice {
        if v == value {
            return i
        }
    }
    return -1
}

func main() {
	fmt.Println(Find([]int{1, 2, 3}, 3))
	fmt.Println(Find([]string{"a", "b"}, "b"))
}
```

#### ~int:
```run-go
package main
import "fmt"

type MyInt int

func Double[T ~int](v T) T {
    return v * 2
}

func main() {
    var a MyInt = 10
    fmt.Println(Double(a)) // ✅ Работает, так как MyInt — это псевдоним int
}
```

#### MyInterface:
```go
type Number interface {
    int | int8 | int16 | int32 | int64 |
    uint | uint8 | uint16 | uint32 | uint64 | uintptr
}

func Sum[T Number](a, b T) T {
    return a + b
}
```

#### Official example:

```run-go
package main
import "fmt"

func SlicesIndex[S ~[]E, E comparable](s S, v E) int {
	for i, _ := range s {
		if v == s[i] {
			return i
		}
	}
	return -1
}

type List[T any] struct {
	head, tail *element[T]
}

type element[T any] struct {
	next *element[T]
	value T
}

func (lst *List[T]) Push(v T) {
	if lst.tail == nil {
		lst.head = &element[T]{value: v}
		lst.tail = lst.head
	} else {
		lst.tail.next = &element[T]{value: v}
		lst.tail = lst.tail.next
	}
}

func (lst *List[T]) AllElements() []T {
	var elems []T
	for e := lst.head; e != nil; e = e.next {
		elems = append(elems, e.value)
	}
	return elems
}

func main() {
	var s = []string{"foo", "bar", "zoo"}
	
	fmt.Println("index of zoo: ", SlicesIndex(s, "zoo"))
	
	_ = SlicesIndex[[]string, string](s, "zoo")

	lst := List[int]{}
	lst.Push(10)
	lst.Push(13)
	lst.Push(20)
	fmt.Println("list: ", lst.AllElements())
}
```
