
Стандартная библиотека Go предоставляет простые инструменты для вывода журналов из программ Go, с пакетом журналов для свободного вывода и пакетом `log/slog` для структурированного вывода.

### Пакет log

```run-go
package main

import (
	"log"
	"bytes"
	"fmt"
	"os"
)

func main() {
	// уже по-умолчанию выводит в os.Stderr
	log.Println("std logger")
	
	// с помощью флагов можно изменять вывод
	log.SetFlags(log.LstdFlags | log.Lmicroseconds)
	log.Println("with micro")

	log.SetFlags(log.LstdFlags | log.Lshortfile)
    log.Println("with file/line")
    
    // создание кастомного логгера
    myLogger := log.New(os.Stdout, "my: ", log.LstdFlags)
    myLogger.Println("from myLogger")
    
	// сменить префикс у логгера
	myLogger.SetPrefix("1-my: ")
	log.Println("from myLogger")
	
	// логгер может принимать разные источники для вывода
	var buf bytes.Buffer
	
	bufLogger := log.New(&buf, "buf: ", log.LstdFlags)
	bufLogger.Println("hello") // логгер запишет в буфер
	
	fmt.Print("from bufLogger: ", buf.String())
}
```

### Пакет log/slog

Пакет log/slog предоставляет **структурированный** вывод логов.
 
 Вывод логов в JSON формате напрямую:
```run-go
package main

import (
	"log/slog"
	"os"
)
  
func main() {
	jsonHandler := slog.NewJSONHandler(os.Stderr, nil)
	myslog := slog.New(jsonHandler)
	myslog.Info("hi there")
	
	// можно перечислить доп. ключи и значения  
	myslog.Info("hello again", "key", "val", "age", 25)
}
```
