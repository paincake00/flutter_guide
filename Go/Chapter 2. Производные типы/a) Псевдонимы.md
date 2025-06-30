
Оператор type позволяет определять для некоторого типа псевдоним. Псевдоним не представляет новый тип, а лишь определяет новое название для уже существующего типа:
`type mile uint`

Можно объявлять переменные этого типа и работать с ними как с объектами базового типа uint:
```run-go
package main
import "fmt"

type mile uint

func main() {
	var distance mile = 5
	fmt.Println(distance)
	distance += 5
	fmt.Println(distance) 
}
```

Псевдонимы могут быть полезны для указания конкретного типа, что может дать дополнительным смысл коду:
```run-go
package main
import "fmt"

type mile uint
type kilometer uint

func distanceToEnemy(distance mile) {
	fmt.Println(distance, "миль")
}

func main() {
	var distance mile = 5
	distanceToEnemy(distance)
}
```

Кроме того, псевдонимы могут сокращать код, например, дать название типу функции:
```run-go
package main
import "fmt"

type BinaryOp func(int, int) int

func action(a, b int, op BinaryOp) {
	res := op(a, b)
	fmt.Println(res)
}

func add(a, b int) int {
	return a + b
}

func main() {
	var op BinaryOp = add
	action(4, 5, op)	
}
```
