
```run-go
package main

import (
	"fmt"
	"os"
)

type point struct {
	x, y int
}

func main() {
	p := point{1, 2}
	
	// %v - печатает структуры
	fmt.Printf("struct1: %v\n", p)
	
	// отображение названий полей
	fmt.Printf("struct2: %+v\n", p)
	
	// синтаксическое представление
	fmt.Printf("struct3: %#v\n", p)
	
	// %T - печать типа
	fmt.Printf("type: %T\n", p)
	
	fmt.Printf("bool: %t\n", true)
	
	fmt.Printf("int: %d\n", 123) // integer
	
	fmt.Printf("binary: %b\n", 14) // binary
	
	fmt.Printf("char: %c\n", 33) // char
	
	fmt.Printf("hex: %x\n", 456) // hex
	
	fmt.Printf("float1: %f\n", 78.4) // float
	
	// %e и %E - float in scientific notation
	fmt.Printf("float2: %e\n", 123400000.0)
	fmt.Printf("float3: %E\n", 123400000.0)
	
	// %s - вывод строки
	fmt.Printf("str1: %s\n", "\"string\"")
	
	// %q - вывод строки включая кавычки
	fmt.Printf("str2: %q\n", "\"string\"")
	
	fmt.Printf("str3: %x\n", "hex this")
	
	fmt.Printf("pointer: %p\n", &p) // pointer
	
	// можно управлять шириной и точностью при выводе
	fmt.Printf("width1: |%6d|%6d|\n", 12, 345)
	
	// с точностью до 2-ух знаков после запятой
	fmt.Printf("width2: |%6.2f|%6.2f|\n", 1.2, 3.45)
	
	// по левой стороне через флаг "-"
	fmt.Printf("width3: |%-6.2f|%-6.2f|\n", 1.2, 3.45)
	
	// также для строк
	fmt.Printf("width4: |%6s|%6s|\n", "foo", "b")
	fmt.Printf("width5: |%-6s|%-6s|\n", "foo", "b")
	
	// Форматирование строки без печати
	s := fmt.Sprintf("sprintf: a %s", "string")
	fmt.Println(s)
	
	// явное указание потока для вывода
	fmt.Fprintf(os.Stderr, "io: an %s\n", "error")
}
```
