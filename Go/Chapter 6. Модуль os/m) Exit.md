
Используйте os.Exit для немедленного выхода с заданным статусом.

```run-go
package main

import (
    "fmt"
    "os"
)

func main() {

    defer fmt.Println("!")

    os.Exit(3)
}
```

Если запустить билд, то можно получить статус:
```
$ go build main.go
$ ./main
$ echo $?
3
```

Команда `echo $?` позволяет увидеть, какой код возврата вернула программа после её выполнения.