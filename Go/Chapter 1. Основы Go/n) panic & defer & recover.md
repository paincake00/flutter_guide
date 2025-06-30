
### Panic

**panic** - это механизм, который используется для обработки **чрезвычайных ситуаций** , когда программа сталкивается с чем-то **непредвиденным и критическим**.

```run-go
package main

import (
	"os"
)

func main() {
	panic("a problem")
	
	_, err := os.Create("/tmp/file") // создание нового файла
	if err != nil {
		panic(err)
	}
}
```

### Defer

**defer** - функция, гарантирующая, что будет выполнен переданный ей код в конце, независимо от ошибок.

```run-go
package main

import (
	"fmt"
	"os"
)

func main() {
	f := createFile("/tmp/defer.txt")
	// выполнится все равно, даже если panic сработал
	defer closeFile(f)
	writeFile(f)
}

func createFile(p string) *os.File {
	fmt.Println("creating")
	f, err := os.Create(p)
	if err != nil {
		panic(err)
	}
	return f
}

func writeFile(f *os.File) {
	fmt.Println("writing")
	fmt.Fprintln(f, "data")
}

func closeFile(f *os.File) {
	fmt.Println("closing")
	err := f.Close()
	
	if err != nil {
		panic(err)
	}
}
```

### Recover

**recover** может остановить панику от прерывания программы и позволить ей продолжить выполнение вместо этого.

Пример применения: 
Сервер не захочет сбоить, если на одном из клиентских подключений будет обнаружена критическая ошибка. Вместо этого сервер захочет закрыть это соединение и продолжить обслуживание других клиентов. На самом деле, это то, что Go net/http делает по умолчанию для HTTP-серверов.

`recover` вызывается вместе с `defer`, чтобы в любом случае перехватить панику:
```run-go
package main

import "fmt"

func mayPanic() {
	panic("a problem")
}

func main() {
	defer func() {
		if r := recover(); r != nil {
			fmt.Println("Recovered. Err: ", r)
		}
	}()
	
	mayPanic()
	
	fmt.Println("Some code after panic")
}
```
