
Для работы с директориями применяются пакеты `os` и `io/fs`

```go
package main

import (
    "fmt"
    "io/fs"
    "os"
    "path/filepath"
)

func check(e error) {
    if e != nil {
        panic(e)
    }
}

func main() {
    // создать директорию внутри текущей
    err := os.Mkdir("subdir", 0755)
    check(err)

    // удаление поддерева каталога в конце
    defer os.RemoveAll("subdir")

    createEmptyFile := func(name string) {
        d := []byte("")
        check(os.WriteFile(name, d, 0644))
    }

    createEmptyFile("subdir/file1") // или "./subdir/file1"

    // иерархии папок с помощью MkdirAll
    err = os.MkdirAll("subdir/parent/child", 0755)
    check(err)

    createEmptyFile("subdir/parent/file2")
    createEmptyFile("subdir/parent/file3")
    createEmptyFile("subdir/parent/child/file4")

    // ReadDir возвращает срез с сущностями каталога (файлы, папки)
    // в указанном пути
    c, err := os.ReadDir("subdir/parent")
    check(err)

    fmt.Println("Listing subdir/parent")
    for _, entry := range c {
        fmt.Println(" ", entry.Name(), entry.IsDir())
    }

    // Chdir - меняет текущую директорию (аналог cd)
    err = os.Chdir("subdir/parent/child")
    check(err)

    c, err = os.ReadDir(".")
    check(err)

    fmt.Println("Listing subdir/parent/child")
    for _, entry := range c {
        fmt.Println(" ", entry.Name(), entry.IsDir())
    }

    err = os.Chdir("../../..")
    check(err)

    // Можно посетить директорию рекурсивно,
    // перебирая все пути и сущности поддерева
    fmt.Println("Visiting subdir")
    filepath.WalkDir("subdir", visit)
}

func visit(path string, d fs.DirEntry, err error) error {
    if err != nil {
        return err
    }
    fmt.Println(" ", path, d.IsDir())
    return nil
}
```

Вывод:
```
Listing subdir/parent
  child true
  file2 false
  file3 false
Listing subdir/parent/child
  file4 false
Visiting subdir
  subdir true
  subdir/file1 false
  subdir/parent true
  subdir/parent/child true
  subdir/parent/child/file4 false
  subdir/parent/file2 false
  subdir/parent/file3 false
```