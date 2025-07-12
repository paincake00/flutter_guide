
### Client

```go
package main

import (
    "bufio"
    "fmt"
    "net/http"
)

func main() {
    url := "https://gobyexample.com"
    resp, err := http.Get(url)
    if err != nil {
        panic(err)
    }

    fmt.Println("Response status:", resp.Status)

    scanner := bufio.NewScanner(resp.Body)
    for i := 0; scanner.Scan() && i < 5; i++ {
        fmt.Println(i, " | ", scanner.Text())
    }

    if err := scanner.Err(); err != nil {
        panic(err)
    }
}
```

OUTPUT:
```sh
Response status:  200 OK
0  |  <!DOCTYPE html>
1  |  <html>
2  |    <head>
3  |      <meta charset="utf-8">
4  |      <title>Go by Example</title>
```

Запрос к JSONPlaceholder API с парсингом json:
```run-go
package main

import (
    "encoding/json"
    "fmt"
    "net/http"
)

type PostResp struct {
    UserId int    `json:"userId"`
    Id     int    `json:"id"`
    Title  string `json:"title"`
    Body   string `json:"body"`
}

// Переопределяем метод String() для кастомного вывода через fmt.Println и т.д.
func (pr *PostResp) String() string {
    return fmt.Sprintf("{\n userId: %d\n id: %d\n title: %s\n body: %s\n}", pr.UserId, pr.Id, pr.Title, pr.Body)
}

func main() {
    url := "https://jsonplaceholder.typicode.com/posts/1"

    resp, err := http.Get(url)
    if err != nil {
        panic(err)
    }

    fmt.Println("Response status:", resp.Status)

    dec := json.NewDecoder(resp.Body)
    var buf *PostResp = &PostResp{}
    err = dec.Decode(buf)
    if err != nil {
        panic(err)
    }

    fmt.Println(buf)
}
```

### Server

Фундаментальная концепция net/http серверов это хендлеры (handlers).
Handler это объект, реализующий интерфейс `http.Handler`. 
Самый простой способ написать хендлер это использовать адаптер `http.HandlerFunc`.

```go
package main

import (
	"fmt"
	"net/http"
)

// Это функция реализует адаптер http.HandlerFunc
// http.ResponseWriter используется для возврата HTTP ответа от сервера
func hello(w http.ResponseWriter, _ *http.Request) {
	fmt.Fprintf(w, "hello\n")
}

func headers(w http.ResponseWriter, r *http.Request) {
	for name, header := range r.Header {
		for _, h := range header {
			fmt.Fprintf(w, "%v: %v\n", name, h)
		}
	}
}

func main() {
	http.HandleFunc("/hello", hello)
	http.HandleFunc("/headers", headers)
	
	// прослушать порт 8090 и вызвать хендлер
	// nil - обработчик по-умолчанию
	http.ListenAndServe(":8090", nil)
}
```

Запуск и обращение:
```sh
$ go run http-server.go $
. . .
Запуск сервера в фоне
. . .

$ curl localhost:8090/hello
hello

$ curl localhost:8090/headers
Accept: */*
User-Agent: curl/8.7.1
```