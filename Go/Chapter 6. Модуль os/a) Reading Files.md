
Есть несколько способов чтения файлов:
- Однократно (разовая загрузка в память)
- По строкам
- Порциями

```go
package main

import (
	"bufio"
	"fmt"
	"io"
	"os"
)

func check(e error) {
	if e != nil {
		panic(e)
	}
}

func main() {



	// ( 1 ) считывание файла целиком в память
	fmt.Println("----1----")
	
	dat, err := os.ReadFile("./tmp/dat")
	check(err)
	fmt.Println(string(dat))
	
	// открытие файла (os.File) для обработки
	f, err := os.Open("./tmp/dat")
	check(err)
	
	
	
	// ( 2 ) Считывания файла блоками (порциями) по 5 байт (\n - это 1 байт)
	fmt.Println("----2----")
	
	buffer := make([]byte, 5) 
	for {
		block, err := f.Read(buffer) // block - кол-во байтов
		if block == 0 && err != nil {
			break
		}
		fmt.Printf("%d bytes: %s\n", block, string(buffer[:block]))
	}
	
	// Найти место (сдвиг курсора по байтам) и начать чтение
	pstn, err := f.Seek(6, io.SeekStart)
	check(err)
	buffer1 := make([]byte, 2)
	block1, err := f.Read(buffer1)
	check(err)
	fmt.Printf("%d bytes | %d pstn: %s\n", block1, pstn, string(buffer1[:block1]))
	
	_, err = f.Seek(2, io.SeekCurrent)
	
	_, err = f.Seek(-4, io.SeekEnd)
	
	pstn1, err := f.Seek(6, io.SeekStart)
	check(err)
	buffer2 := make([]byte, 2)
	block2, err := io.ReadAtLeast(f, buffer2, 2) // 2 - min число байт для чтения
	// n, err := io.ReadAtLeast(reader, buffer, min)
	check(err)
	fmt.Printf("%d bytes | %d pstn: %s\n", block2, pstn1, string(buffer2))
	
	// возврщаем курсор в начало
	_, err = f.Seek(0, io.SeekStart)
	check(err)
	
	
	
	// ( 3 ) Чтение файла по строкам
	fmt.Println("----3----")
	
	reader := bufio.NewReader(f)
	for {
		line, err := reader.ReadString('\n') // читает до символа '\n'
		if err != nil {
			if err == io.EOF {
				break
			}
			check(err)
		}
		fmt.Println(string(line))
	}
	
	f.Close()
}
```

файл:
```
hello
go
forever
```

Вывод:
```
----1----
hello
go
forever

----2----
5 bytes: hello
5 bytes: 
go
f
5 bytes: oreve
2 bytes: r

2 bytes | 6 pstn: go
2 bytes | 6 pstn: go
----3----
hello

go

forever

```