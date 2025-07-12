
Аргументы командной строки являются распространенным способом параметризации выполнения программ. Например, go run hello.go использует аргументы run и hello.go для программы go.

```go
package main

import (
	"fmt"
	"os"
)

func main() {
	argsWithProg := os.Args
	argsWithoutProg := os.Args[1:]
	
	arg := os.Args[3]
	
	fmt.Println(argsWithProg)
	fmt.Println(argsWithoutProg)
	fmt.Println(arg)
}
```

Результат:
```
$ go build command-line-arguments.go

$ ./command-line-arguments a b c d

[./command-line-arguments a b c d]       
[a b c d]
c
```