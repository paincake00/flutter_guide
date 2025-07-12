
*В операционной системе:*
Процесс — это программа в состоянии выполнения. Он включает в себя не только код программы, но и её текущее состояние: значения регистров процессора, переменные, файлы, которые она открыла, и так далее.
Каждый процесс имеет своё собственное виртуальное адресное пространство, что обеспечивает изоляцию от других процессов.

Пакет `os/exec` в Go позволяет запускать внешние команды.

##### Пример запуска команды `ls -la`:
```run-go
package main

import (
    "fmt"
    "os/exec"
)

func main() {
    // Создаем команду "ls -la" (показать содержимое директории)
    cmd := exec.Command("ls", "-la")
    
    // Запускаем команду и получаем вывод
    output, err := cmd.Output()
    if err != nil {
        fmt.Printf("Ошибка выполнения команды: %s\n", err)
        return
    }
    
    // Выводим результат
    fmt.Printf("Вывод команды:\n%s\n", output)
}
```

##### Получение вывода с разделением stdout и stderr:
```run-go
package main
  
import (
	"bytes"
	"fmt"
	"os/exec"
)
  
func main() {

	cmd := exec.Command("ping", "-c", "3", "google.com")
	  
	var stdout, stderr bytes.Buffer
	cmd.Stdout = &stdout
	cmd.Stderr = &stderr
	  
	err := cmd.Run() // запускает и ждет завершения команды
	if err != nil {
		fmt.Printf("Ошибка: %s\n", err)
		fmt.Printf("Stderr: %s\n", stderr.String())
		return
	}
	  
	fmt.Printf("Вывод команды:\n%s\n", stdout.String())
}
```

##### Передача входных данных процессу:
```run-go
package main

import (
    "bytes"
    "fmt"
    "os/exec"
)

func main() {
    // Создаем команду "grep hello"
    cmd := exec.Command("grep", "hello")
    
    // Входные данные для команды
    input := bytes.NewBufferString("hello world\ngoodbye world")
    cmd.Stdin = input
    
    // Получаем вывод
    output, err := cmd.Output()
    if err != nil {
        fmt.Printf("Ошибка: %s\n", err)
        return
    }
    
    fmt.Printf("Grep нашел: %s\n", output)
}
```

##### Асинхронный запуск процесса:
```run-go
package main

import (
    "fmt"
    "os/exec"
    "time"
)

func main() {
    // Создаем долго работающую команду
    cmd := exec.Command("sleep", "5")
    
    // Запускаем асинхронно
    if err := cmd.Start(); err != nil {
        fmt.Printf("Ошибка запуска: %s\n", err)
        return
    }
    
    fmt.Println("Процесс запущен и работает в фоне...")
    
    // Делаем что-то другое пока команда выполняется
    for i := 0; i < 3; i++ {
        fmt.Printf("Программа продолжает работу... %d\n", i)
        time.Sleep(time.Second)
    }
    
    // Дожидаемся завершения процесса
    err := cmd.Wait()
    if err != nil {
        fmt.Printf("Ошибка при ожидании: %s\n", err)
        return
    }
    
    fmt.Println("Процесс успешно завершился!")
}
```

# Замена процесса

Иногда необходимо не создать (spawn) процесс из текущего Go программы, а заменить Go процесс на другой (даже не Go):
```run-go
package main
  
import (
	"os"
	"os/exec"
	"syscall"
)
  
func main() {
	// находит абсолютный путь к исполняемому двоичному файлу 
	// (условно "bin/ls")
	bin, err := exec.LookPath("ls")
	if err != nil {
		panic(err)
	}
	  
	args := []string{"ls", "-l"}
	
	env := os.Environ()
	
	err = syscall.Exec(bin, args, env)
	if err != nil {
		panic(err)
	}
}
```
