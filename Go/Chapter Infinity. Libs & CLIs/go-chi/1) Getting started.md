
Installation: `go get -u github.com/go-chi/chi/v5`

```go
package main

import (
    "net/http"

    "github.com/go-chi/chi/v5"
    "github.com/go-chi/chi/v5/middleware"
)

func main() {
    r := chi.NewRouter()
    
    // вывод логов в консоль
    r.Use(middleware.Logger)
    
    // добавления хендлера
    r.Get("/", func(w http.ResponseWriter, r *http.Request) {
        w.Write([]byte("Hello World!")) 
    })
    http.ListenAndServe(":3000", r)
}
```

По адресу `http://localhost:3000` будет сообщение `Hello World!`.