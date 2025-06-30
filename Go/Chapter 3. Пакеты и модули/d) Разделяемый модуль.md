
В одном каталоге `projects` есть два проекта:
- **messagelib** - модуль, который будет библиотекой
- **helloapp** - модуль с основной программой

В модуле-библиотеке определяем файл `message.go`:
```go
package message
import "fmt"

func Hello() {
	fmt.Println("Hello METANIT.COM")
}
```
И инициализируем модуль:
```sh
go mod init metanit.com/messages
```

В основной программе файл `main.go`:
```go
package main
import "metanit.com/messages"

func main() {
	messages.Hello()
}
```
И инициализируем модуль:
```sh
go mod init helloapp
```

Теперь связать модуль, находящийся локально можно через:
```sh
go mod edit -replace="metanit.com/messages=../messagelib"
```

После этого `go.mod` в helloapp изменится:
```
module helloapp

go 1.24.4

replace metanit.com/messages => ../messageslib
```