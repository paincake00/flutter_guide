
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

## Виды контекста

#### Базовый пустой (context.Background)

```go
ctx := context.Background()
```

Обычно служит "корнем" для дерева контекста.
Он пустой: не имеет дедлайнов, не содержит значений и не может быть отменен.
Обычно используется в точке входа в приложение (`main`) или в тестах.

#### Тестовый (context.TODO)

```go
ctx := context.TODO()
```

Такой же пустой контекст, но с пометкой, что место требует контекста, но вы ещё не решили, какой использовать.
Используется в местах, где интерфейс требует `Context`, но правильный вариант ещё не определён.

#### С отменой вручную (context.WithCancel)

```go
ctx, cancel := context.WithCancel(parentCtx)
defer cancel() // функция вызывается при отмене (здесь для очистки ресурсов)
```

Контекст, который может быть отменен явно через вызов `cancel()`.
Используется при завершении операции по внешнему событию (например, закрытие сервера) или для остановки фоновых горутин.

#### С тайм-аутом (context.WithTimeout)

```go
ctx, cancel := context.WithTimeout(parentCtx, 2*time.Second)
defer cancel()
```

Контекст, который автоматически отменится через заданное время.
Можно использовать для ограничения длительности запросов к внешним сервисам или БД.

#### С дедлайном (context.WithDeadline)

```go
ctx, cancel := context.WithDeadline(parentCtx, time.Now().Add(5*time.Second))
defer cancel()
```

Контекст, который отменяется в конкретное заданное время.
Используется, когда есть глобальный момент завершения задачи (например, окончание работы сервера в полночь).

#### С хранением значений (context.WithValue)

```go
ctx := context.WithValue(parentCtx, userKey, userValue)
```

Контекст с сохраненным значением по ключу.
Передача метаданных запроса (например, `requestID`, `userID`).
Логирование, трассировка, аутентификация.

#### Из готовых API

- **HTTP-запросы:**

```go
ctx := req.Context()
```
Получает контекст, который отменится при завершении запроса или закрытии соединения.

- **gRPC:**

```go
ctx := stream.Context()
```
Автоматически управляет временем жизни RPC-запроса.

- **`os/signal`**:

```go
ctx, stop := signal.NotifyContext(context.Background(), os.Interrupt)
defer stop()
```
Отменяется при определенном сигнале от ОС.

|Ситуация|Контекст|
|---|---|
|Корень цепочки|`Background()`|
|Код ещё в разработке|`TODO()`|
|Отмена по событию|`WithCancel()`|
|Ограничение по времени|`WithTimeout()`|
|Конкретная дата/время конца|`WithDeadline()`|
|Передача метаданных|`WithValue()`|
|HTTP/gRPC/Signal обработка|Из API (`req.Context()`, `signal.NotifyContext` и т.п.)|

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