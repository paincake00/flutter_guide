
Указатель - это переменная, которая хранит адрес другой переменной в памяти.

```run-go
package main

import "fmt"

func main() {
	var x int = 4 // определяем переменную
	var p *int // определяем указатель
	p = &x // указатель получает адрес переменной
	fmt.Println(p) // значение самого указателя - адрес переменной x
	fmt.Println(*p) // получение значения переменной по адресу
}
```

#### Применение

- По умолчанию в Go аргументы передаются по значению (копируются). Указатели позволяют **изменять** исходные переменные, ссылаясь на них:

```run-go
package main

import "fmt"

func increment(x *int) {
    *x++ // меняем значение переменной, полученной по адресу
}

func main() {
    a := 5
    increment(&a) // передаем адрес
    fmt.Println(a) // вывод: 6
}
```

- Указатели экономят место при передаче в функцию большой структуры (избавляет от создания копии) для ее последующего **изменения**:

```run-go
package main

import "fmt"

type User struct {
	Name string
	Email string
	Avatar []byte // большое изображение
}

func updateUser(u *User) {
	u.Name = "NewName"
}
```

То есть, наиболее частым применением указателей является получение значения переменной (полученной по адресу) и его изменение (без копирования):

```run-go
package main

import "fmt"

func zeroVal(val int) {
	val = 0
}

func zeroPointer(ptr *int) {
	*ptr = 0
}

func main() {
	i := 1
    fmt.Println("initial:", i)
    
    zeroVal(i)
    fmt.Println("zeroVal:", i)
    
    zeroPointer(&i)
    fmt.Println("zeroPointer:", i)
    
    fmt.Println("address:", &i)
}
```
