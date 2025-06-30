
```run-go
package main

import (
    "fmt"
    "maps"
)

func main() {

    m := map[string]int{}

    m["k1"] = 7
    m["k2"] = 13

    fmt.Println("map:", m)

    v1 := m["k1"]
    fmt.Println("v1:", v1)

    v3 := m["k3"]
    fmt.Println("v3:", v3)

    fmt.Println("len:", len(m))

    delete(m, "k2")
    fmt.Println("map:", m)

    clear(m)
    fmt.Println("map:", m)

    _, prs := m["k2"] // _ - это само значение, prs - есть ли значение
    fmt.Println("prs:", prs)

    n := map[string]int{"foo": 1, "bar": 2}
    fmt.Println("map:", n)

    n2 := map[string]int{"foo": 1, "bar": 2}
    if maps.Equal(n, n2) {
        fmt.Println("n == n2")
    }
}
```

Также есть функция для удаления по ключу: `delete(map, key)`
```go
var people = map[string]int{"Tom": 1, "Bob": 2, "Sam": 8}

delete(people, "Bob")

fmt.Println(people)     // map[Tom:1  Sam:8]
```