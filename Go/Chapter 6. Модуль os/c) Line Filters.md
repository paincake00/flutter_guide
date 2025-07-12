
Line Filter - это распространенный тип программы, которая считывает входные данные на stdin, обрабатывает их, а затем печатает некоторый полученный результат в stdout. 
grep и sed являются распространенными линейными фильтрами.

grep - это утилита командной строки для поиска в наборах данных открытого текста для строк, соответствующих регулярному выражению.
```sh
$ grep 'o' ./tmp/dat 
hello
go
forever
```

Воспользуемся scanner для реализации программы для записи заглавными буквами всего ввода из stdin:
```go
package main

import (
	"fmt"
	"bufio"
	"os"
	"strings"
)

func main() {
	scanner := bufio.NewScanner(os.Stdin)
	
	// считывание следующей строки, если есть
	for scanner.Scan() {
		ucl := strings.ToUpper(scanner.Text())
		fmt.Println(ucl)
	}
	
	if err := scanner.Err(); err != nil {
		fmt.Fprintln(os.Stderr, "error:", err)
        os.Exit(1)
	}
}
```

Запуск:
```sh
$ cat ./tmp/dat | go run main.go
HELLO
GO
FOREVER
```

 `|` — оператор pipe: перенаправляет вывод первой команды (`cat`) во **ввод второй команды** (`go run ...`)