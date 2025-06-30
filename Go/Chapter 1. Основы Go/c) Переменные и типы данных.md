
### Объявление переменных:
```go
// Явное объявление с указанием типа
var name string = "Имя"

// Неявное определение типа
var age = 30

// Краткое объявление (работает только внутри функций)
salary := 50000.50

// Объявление нескольких переменных
var (
    isActive bool = true
    count    int  = 10
)
```

### константы:
```run-go
package main

import (
    "fmt"
    "math"
)

const s string = "constant"

func main() {
    fmt.Println(s)

    const n = 500000000

    const d = 3e20 / n
    fmt.Println(d)

    fmt.Println(int64(d))

    fmt.Println(math.Sin(n))
}
```

### Типы данных:
```go
// Целые числа
var a int = 10       // int зависит от архитектуры (32 или 64 бита)
var b int8 = 127     // -128 до 127
var c int16 = 32767  // -32768 до 32767
var d int32 = 2147483647
var e int64 = 9223372036854775807
var f uint = 10      // только положительные
var ff byte = 255 // или же unint8

// Числа с плавающей точкой
var g float32 = 3.14 // от 1.4*10^-45 до 3.4*10^38 (4 байта)
var h float64 = 3.141592653589793 // от 4.9*10^-324 до 1.8*10^308 (8 байт)

// Логический тип
var i bool = true

// Строки
var j string = "Привет," + "мир!"

// Руны (символы Unicode)
var k rune = -2147483646  // это int32
```

