
//go:embed - это директива компилятора, которая позволяет программам включать произвольные файлы и папки в двоичный файл Go во время сборки.

```go
package main

import (
    "embed"
)

//go:embed tmp/dat
var fileString string

//go:embed tmp/dat2
var fileByte []byte

// реализация простой виртуальной файловой системы:
//
//go:embed tmp/dat1
var fs embed.FS

func main() {
    println(fileString)
    println(string(fileByte))

    content, _ := fs.ReadFile("tmp/dat1")
    println(string(content))
}
```

Вывод:
```
hello
go
forever

some
writes
buffered

hello world
```