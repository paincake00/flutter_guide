
### Массивы:

```run-go
package main

import "fmt"

func main() {

    var a [5]int
    fmt.Println("emp:", a)

    a[4] = 100
    fmt.Println("set:", a)
    fmt.Println("get:", a[4])

    fmt.Println("len:", len(a))

    b := [5]int{1, 2, 3, 4, 5}
    fmt.Println("dcl:", b)

    b = [...]int{1, 2, 3, 4, 5} // компилятор определяет фикс. длину
    fmt.Println("dcl:", b)

    b = [...]int{100, 3: 400, 500}
    fmt.Println("idx:", b)

    var twoD [2][3]int
    for i := range 2 {
        for j := range 3 {
            twoD[i][j] = i + j
        }
    }
    fmt.Println("2d: ", twoD)

    twoD = [2][3]int{
        {1, 2, 3},
        {1, 2, 3},
    }
    fmt.Println("2d: ", twoD)
}
```

### Срезы:

Подробнее в доке: 
- https://go.dev/blog/slices-intro
- https://go.dev/blog/slices

В отличие от массивов, срезы печатаются только по элементам, которые они содержат (не количеству элементов). Неинициализированный срез равен нулю и имеет длину 0.

```run-go
package main

import (
    "fmt"
    "slices"
)

func main() {

    var s []string
    fmt.Println("uninit:", s, s == nil, len(s) == 0)

    s = make([]string, 3)
    fmt.Println("emp:", s, "len:", len(s), "cap:", cap(s))

    s[0] = "a"
    s[1] = "b"
    s[2] = "c"
    fmt.Println("set:", s)
    fmt.Println("get:", s[2])

    fmt.Println("len:", len(s))

    s = append(s, "d")
    s = append(s, "e", "f")
    fmt.Println("apd:", s)

    c := make([]string, len(s))
    copy(c, s)
    fmt.Println("cpy:", c)

    l := s[2:5]
    fmt.Println("sl1:", l)

    l = s[:5]
    fmt.Println("sl2:", l)

    l = s[2:]
    fmt.Println("sl3:", l)

    t := []string{"g", "h", "i"}
    fmt.Println("dcl:", t)

    t2 := []string{"g", "h", "i"}
    if slices.Equal(t, t2) {
        fmt.Println("t == t2")
    }

    twoD := make([][]int, 3)
    for i := range 3 {
        innerLen := i + 1
        twoD[i] = make([]int, innerLen)
        for j := range innerLen {
            twoD[i][j] = i + j
        }
    }
    fmt.Println("2d: ", twoD)
}
```

#### Структура среза:
![[Pasted image 20250613080148.png]]
- PTR - указатель на массив
- LEN - длина сегмента (сколько элементов в диапазоне)
- CAP - кол-во элементов в массиве, начиная с указателя среза
  
Создание среза через `make([]byte, 5)`:
![[Pasted image 20250613081141.png|500]]

`s = s[2:4]` - меняем срез (и его структуру):
![[Pasted image 20250613081455.png|500]]
И здесь длина будет 2 (так как два элемента в срезе), а емкость 3 (так как начиная с первого элемента в срезе и до конца базового массива).

Так как срез `s` был переписан, то если применим `s[:cap(s)]`, то будет в нем содержаться три элемента, потому что для capacity для этого среза уже равен трем:
![[Pasted image 20250613181808.png|500]]

#### Увеличение среза s через создание промежуточного t c копированием элементов:
```go
t := make([]byte, len(s), (cap(s)+1)*2)
copy(t, s) // вместо for i:=range s {t[i]=s[i]}
s = t
```
Копирование также можно использовать для экономии памяти, чтобы в новый срез добавить только необходимые данные.

#### Вставка в срез:
```run-go
package main

import (
	"fmt"
)

func Insert(slice []int, index, value int) []int {
	slice = slice[0 : len(slice)+1]
	copy(slice[index+1:], slice[index:])
	slice[index] = value
	return slice
}

func main() {
	slice := make([]int, 10, 20) // Note capacity > length: room to add element.
	
	fmt.Println(slice) // only zeros
	
	for i := range slice {
		slice[i] = i // now val := index
	}
	
	fmt.Println(slice)
	slice = Insert(slice, 5, 99)
	fmt.Println(slice)
}
```
Но все упирается в емкость среза, которая может быть превышена (cap(slice) == len(slice) -> full slice), что может вызвать панику.

#### Добавление элементов в срез:
```go
// Объявление среза
var slice []int                  // Пустой срез
slice2 := []int{1, 2, 3, 4, 5}   // С инициализацией

// Создание среза из массива
arr := [5]int{1, 2, 3, 4, 5}
slice3 := arr[1:4]  // [2, 3, 4]

// Функции для работы со срезами
slice = append(slice, 10)    // Добавление элемента
slice = append(slice, 20, 30)  // Добавление нескольких

// Make для создания среза с предварительным выделением памяти
slice4 := make([]int, 5)      // Длина 5, емкость 5 
slice5 := make([]int, 0, 10)  // Длина 0, емкость 10
```

Реализация собственного Append:
```run-go
package main

import "fmt"

func Append(slice []int, elements ...int) []int {
	n := len(slice)
	total := n + len(elements)
	if total > cap(slice) {
		newSize := total*3/2 + 1
		newSlice := make([]int, total, newSize)
		copy(newSlice, slice)
		slice = newSlice
	}
	copy(slice[n:], elements)
	return slice
}

func main() {
	slice1 := []int{0, 1, 2, 3, 4}
	slice2 := []int{55, 66, 77}
	fmt.Println(slice1)
	slice1 = Append(slice1, slice2...)
	fmt.Println(slice1)
}
```

#### Удаление элемента через срез:
```run-go
package main

import "fmt"

func main() {
	// var users []string = []string{"a", "b", "c", "d"}
	
	users := make([]int, 5)
	users[0] = 2
	users[3] = 4
	
	var index = 2
	
	users = append(users[:index], users[index+1:]...)
	
	fmt.Println(users)
}
```
