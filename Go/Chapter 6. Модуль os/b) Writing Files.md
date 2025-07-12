
При записи в выгруженный в память файл через `os.WriteFile` 3-им аргументов пишется file mode, который определяет уровень доступа к файлу.
К примеру, в 8-очной СС число `0644` эквивалентно `rw-r--r--`, где:

| 6   | Владелец: чтение + запись (`rw-`)    |
| --- | ------------------------------------ |
| 4   | Группа: только чтение (`r--`)        |
| 4   | Все остальные: только чтение (`r--`) |
```go
package main

import (
    "bufio"
    "fmt"
    "os"
)

func check(e error) {
    if e != nil {
        panic(e)
    }
}

func main() {
    d1 := []byte("hello world")
    // файл будет создан, если его нет
    err := os.WriteFile("./tmp/dat1", d1, 0644)
    check(err)

    f, err := os.Create("./tmp/dat2")
    check(err)

    defer f.Close()

    d2 := []byte{115, 111, 109, 101, 10}
    n2, err := f.Write(d2) // возвращает кол-во записанных байт
    check(err)
    fmt.Printf("wrote %d bytes\n", n2)

    n3, err := f.WriteString("writes\n")
    check(err)
    fmt.Printf("wrote %d bytes\n", n3)

    f.Sync() // сохранение файла (из буфера на диск)

    w := bufio.NewWriter(f)
    n4, err := w.WriteString("buffered\n")
    check(err)
    fmt.Printf("wrote %d bytes\n", n4)

    w.Flush() // буферные записи сохраняются в файл
}
```

Файл `dat2`:
```
some
writes
buffered

```

