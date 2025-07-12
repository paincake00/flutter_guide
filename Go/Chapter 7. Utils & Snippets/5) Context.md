
Контекст (`context.Context`) позволяет:
- Отменять операции, если клиент закрыл соединение
- Устанавливать таймауты и дедлайны
- Передавать данные, связанные с текущим запросом
Это очень важно в HTTP-серверах, микросервисах и асинхронных задачах.

Простой пример с `context.WithCancel`:
```run-go
package main

import (
	"context"
	"fmt"
	"time"
)

func operation(ctx context.Context, duration time.Duration) {
	select {
		case <-time.After(duration):
			fmt.Println("Operation completed")
		case <-ctx.Done(): // контекст отменился извне
			fmt.Println("Operation cancelled: ", ctx.Err())
	}
}

func main() {
	ctx, cancel := context.WithCancel(context.Background())
	
	go operation(ctx, 5*time.Second)
	
	time.Sleep(2*time.Second)
	cancel() // отмена (пользователь закрыл подключение)
	
	time.Sleep(time.Second)
}
```

Контекст можно получать из request и работать с ним.
##### Зачем нужен контекст в `http.Request`?

1. **Отмена запроса:** Контекст позволяет отменить выполнение запроса, если клиент разрывает соединение или если запрос обрабатывается слишком долго. Это помогает освободить ресурсы и остановить ненужные операции.

2. **Тайм-ауты:** Контекст может содержать информацию о тайм-аутах, что позволяет автоматически отменять запрос, если он не завершается в установленное время.

3. **Передача данных:** Контекст позволяет передавать данные, специфичные для запроса, через различные части приложения, такие как middleware и обработчики.

```go
package main

import (
	"fmt"
	"net/http"
	"time"
)

func hello(w http.ResponseWriter, req *http.Request) {
	ctx := req.Context()
	fmt.Println("server: hello handler started")
	defer fmt.Println("server: hello handler ended")

	select {
	case <-time.After(10 * time.Second):
		fmt.Fprintf(w, "hello\n")
	case <-ctx.Done():
		err := ctx.Err()
		fmt.Println("server: ", err)
		internalError := http.StatusInternalServerError
		http.Error(w, err.Error(), internalError)
	}
}

func main() {
	http.HandleFunc("/hello", hello)
	http.ListenAndServe(":8090", nil)
}
```

Запуск сервера на фоне и досрочная отмена:
```sh
$ go run context.go &

$ curl localhost:8090/hello
	server: hello handler started
	^C
	server: context canceled
	server: hello handler ended
```