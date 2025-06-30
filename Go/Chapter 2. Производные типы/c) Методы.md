
Метод - это функция, привязанная к определенному типу. 

При определении метода необходимо указать получателя (receiver). Получатель - это параметр того типа, к которому прикрепляется метод.

```go
func (имя_параметра тип_получателя) имя_метода(параметры) (типы_возвращаемых_результатов) {
	. . .
}
```

Пример:
```run-go
package main
import "fmt"

type library []string

func (l library) print() {
	for _, val := range l {
		fmt.Println(val)
	}
}

func main() {
	var lib library = library{"ok", "ko", "kk"}
	lib.print()
}
```

### Методы структур

Есть несколько типов получателя: по указателю и по значению. По значению просто передает копию структуры в метод и исходная структура не изменяется (не мутируется). А при передаче получателя с типом по указателю появляется возможность влиять на исходную структуру (изменять данные для полей), и это можно делать просто обращаясь через dot-оператор, так как Go автоматически достает значение из указателя.

```run-go
package main
import "fmt"

type rect struct {
	width, height int
}

// обрабатывает получателя как указатель (изменение исходной стуктуры)
func (r *rect) area() int {
	if r.width <= 0 { r.width = 1 }
	if r.height <= 0 { r.height = 1 }
	return r.width * r.height
}

func (r rect) perimeter() int {
	return 2*r.width + 2*r.height
}

func main() {
	r := rect{width: -1, height: 5}
	
	fmt.Println("area: ", r.area())
	fmt.Println("perimeter: ", r.perimeter())
	
	rp := &r // можно вызывать методы и у указателя
	fmt.Println("area: ", rp.area())
	fmt.Println("perimeter: ", rp.perimeter())
}
```
