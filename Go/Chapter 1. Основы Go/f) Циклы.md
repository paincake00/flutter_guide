
### for
```go
// full for со всеми параметрами
for i:=0; i<10: i++ {
	fmt.Println(i * i)
}

// for с условием и изменением счетчика
var i int = 0
for ; i<10; i++ {
	fmt.Println(i * i)
}

// for только с условием
var e int = 0
for e<10 { // можно указывать без ; 
	fmt.Println(e)
	e++
}
```

### Вложенный for
```run-go
package main
import "fmt"

func main() {
	for i:=1; i<6; i++{
		for j:=1; j<6; j++{
			fmt.Println(i*j, "\t")
		}
		fmt.Println()
	}
}
```

### for с range

```go
// По элементам массива/среза
fruits := []string{"apple", "banana", "cherry"}
for index, value := range fruits {
    fmt.Printf("Index: %d, Value: %s\n", index, value)
}

// По символам строки
for i, char := range "Привет" {
    fmt.Printf("Index: %d, Char: %c\n", i, char)
}

// По ключам/значениям карты
ratings := map[string]float64{"Go": 4.8, "Python": 4.7}
for lang, rating := range ratings {
    fmt.Printf("%s: %.1f\n", lang, rating)
}
```

```run-go
package main

import "fmt"

func main() {

    i := 1
    for i <= 3 {
        fmt.Println(i)
        i = i + 1
    }

    for j := 0; j < 3; j++ {
        fmt.Println(j)
    }

    for i := range 3 {
        fmt.Println("range", i)
    }

    for {
        fmt.Println("loop")
        break
    }

    for n := range 6 {
        if n%2 == 0 {
            continue
        }
        fmt.Println(n)
    }
}
```


