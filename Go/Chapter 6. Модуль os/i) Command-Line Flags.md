
Флаги командной строки являются распространенным способом указания параметров для программ командной строки. Например, в wc -l -l является флагом командной строки.

```go
package main

import (
	"flag"
	"fmt"
)
  
func main() {
	// три аргумента:
	// name - указывается имя флага (word)
	// value - значение по умолчанию (foo)
	// usage - описание, например для --help (a string)
	wordPtr := flag.String("word", "foo", "a string")
	  
	numbPtr := flag.Int("numb", 42, "an int")
	forkPtr := flag.Bool("fork", false, "a bool")
	  
	var svar string
	flag.StringVar(&svar, "svar", "bar", "a string var") // аналог для flag.String
	  
	// Эта функция разбирает аргументы командной строки
	// и заполняет переменные соответствующими значениями
	flag.Parse()
	  
	fmt.Println("word:", *wordPtr)
	fmt.Println("numb:", *numbPtr)
	fmt.Println("fork:", *forkPtr)
	fmt.Println("svar:", svar)
	fmt.Println("tail:", flag.Args())
}
```

Запускаем собранную программу с атрибутами:
```sh
./main -word=fff -fork=true -svar=brb ex1 ex2
```

Получаем:
```sh
word: fff
numb: 42
fork: true
svar: brb
tail: [ex1 ex2]
```

#### Атрибут --help

Вызов команды:
```sh
./main --help
```

Получаем результат `--help`:
```
Usage of ./main:
  -fork
        a bool
  -numb int
        an int (default 42)
  -svar string
        a string var (default "bar")
  -word string
        a string (default "foo")
```