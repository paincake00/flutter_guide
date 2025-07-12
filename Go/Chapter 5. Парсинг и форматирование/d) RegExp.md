
**RegExp (Regular Expressions)** — это **мощный инструмент для поиска, сопоставления и обработки текста** по определённому шаблону.

```run-go
package main

import (
	"fmt"
	"bytes"
	"regexp"
)

func main() {
	// прямое сопоставление регулярного выражения со строкой
	match, _ := regexp.MatchString("p([a-z]+)ch", "peach")
	fmt.Println(match) // возвращает типа bool
	
	// компиляция RegExp-структуру 
	// для неоднократного использования регулярного выражения
	r, _ := regexp.Compile("p([a-z]+)ch")
	
	fmt.Println(r.MatchString("peach"))
	
	// найти соответствие для regexp
	fmt.Println(r.FindString("peach punch"))
	
	// возвращает начальный и конечный индекс найденного совпадения
	// по регулярному выражению
	fmt.Println("idx:", r.FindStringIndex("peach punch"))
	
	// Возврат match и submatch (например, "([a-z]+)")
	fmt.Println(r.FindStringSubmatch("peach punch"))
	fmt.Println(r.FindStringSubmatchIndex("peach punch"))
	
	// найти все совпадения и вернуть срез
	fmt.Println(r.FindAllString("peach punch pinch", -1))
	
	// также индексы совпадений
	fmt.Println("all:", r.FindAllStringSubmatchIndex("peach punch pinch", -1))
	
	// создание без возврата ошибки (вместо нее - паника)
	r = regexp.MustCompile("p([a-z]+)ch")
	fmt.Println("regexp:", r)
	
	// замена найденной строки по regexp на другую
	fmt.Println(r.ReplaceAllString("a peach", "apple"))
	
	// применение функции (только с байтами)
	in := []byte("a peach")
	out := r.ReplaceAllFunc(in, bytes.ToUpper)
	fmt.Println(string(out))
}
```
