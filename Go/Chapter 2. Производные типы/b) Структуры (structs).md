
Структуры Go - это типизированные коллекции полей. Они полезны для группировки данных вместе для формирования записей.

Структуры мутабельны (изменяемы) и поля без значений будут принимать по умолчанию нулевое значение (zero-valued), например, для int это 0, для bool это false и т.д.

Если структура начинается с большой буквы (`type Person struct`), то она видна вне пакета, а если с маленькой (`type person struct`) - только внутри этого пакета.

```run-go
package main
import "fmt"

type person struct {
    name string
    age  int
    surname string
    isAdult bool
}

func newPerson(name string) *person {
    p := person{name: name}
    p.age = 42
    return &p
}

func main() {
    fmt.Println(person{"Bob", 20, "Marly", true})
    // именованные поля выборочно
    fmt.Println(person{name: "Alice", age: 30})
    fmt.Println(person{name: "Fred"}) // age == 0 по умолчанию

	// получение структурного указателя
    fmt.Println(&person{name: "Ann", age: 40})
	fmt.Println(newPerson("Jon"))

	// извлекаем значение
    fmt.Println(*newPerson("Jon"))

    s := person{name: "Sean", age: 50}
    fmt.Println(s.name)

    sp := &s
    // dot-оператор может сразу получать значение из указателя без *
    fmt.Println(sp.age)
    // можно и с явным указанием
    fmt.Println((*sp).age)

    sp.age = 51
    fmt.Println(sp.age)

	// объявление анонимной структуры (полезно для тестов)
    dog := struct {
        name   string
        isGood bool
    }{
        "Rex",
        true,
    }
    fmt.Println(dog)
}
```

### Вложенные структуры (struct embedding)

```run-go
package main
import "fmt"

type contact struct {
	email string
	phone string
}

type person struct {
	name string
	age int
	contactInfo contact
}

func main() {
	var tom = person{
		name: "Bob",
		age: 21,
		contactInfo: contact{
			email: "tom@gmail.com",
			phone: "+1234567899",
		},
	}
	tom.contactInfo.email = "supertom@gmail.com"
	
	fmt.Println(tom.contactInfo.email)
	fmt.Println(tom.contactInfo.phone)
}
```

Можно сократить запись:
```run-go
package main
import "fmt"

type contact struct {
	email string
	phone string
}

type person struct {
	name string
	age int
	contact
}

func main() {
	var tom = person{
		name: "Bob",
		age: 21,
		contact: contact{
			email: "tom@gmail.com",
			phone: "+1234567899",
		},
	}
	tom.email = "supertom@gmail.com"
	
	fmt.Println(tom.email)
	fmt.Println(tom.phone)
}

```

==Это аналог классического наследования через встраивание (embedding). Все поля и методы contact доступны в "дочерней" структуре.==

Если в структуре есть поле типа такого же, как у этой же структуры, то необходимо обозначить это поле как указатель на структуру этого типа:
```run-go
package main
import "fmt"

type node struct {
	value int
	next *node
}

func printNodeValue(n *node) {
	fmt.Println(n.value)
	if n.next != nil {
		printNodeValue(n.next)
	}
}

func main() {
	first := node{value: 4}
	second := node{value: 6}
	third := node{value: 8}
	
	first.next = &second
	second.next = &third
	
	var current *node = &first
	for current != nil {
		fmt.Println(current.value)
		current = current.next
	}
}
```
