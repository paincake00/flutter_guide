
Перечисление - это тип, имеющий фиксированное число возможных значений, каждое из которых имеет свое имя.
В Go нет встроенной реализации enums, но можно их реализовать с помощью языковых конструкций.

##### Реализация:

- Создадим перечисление ServerState, в основе которого будет лежать тип int. 
- Возможные значения перечисления будут определены как константы.
- С помощью ключевого слова `iota` можно автоматически задать значения для констант (0, 1, 2 и т.д.).
- Определяем отображение (map) и даем каждому значению строку (будет содержать имя).
- Реализуем интерфейс `fmt.Stringer` через определение метода String(), чтобы автоматически выводить строковые значения.

```run-go
package main
import "fmt"

type ServerState int

const (
	StateIdle ServerState = iota
	StateConnected
	StateError
	StateRetrying
)

var stateNames = map[ServerState]string{
	StateIdle: "idle",
	StateConnected: "connected",
	StateError: "error",
	StateRetrying: "retrying",
}

func (state ServerState) String() string {
	return stateNames[state]
}

func main() {
	state := transition(StateIdle)
	fmt.Println(state)
	
	state2 := transition(state)
	fmt.Println(state2)
}

func transition(s ServerState) ServerState {
	switch s {
		case StateIdle:
			return StateConnected
		case StateConnected, StateRetrying:
			return StateIdle
		case StateError:
			return StateError
		default:
			panic(fmt.Errorf("unknown state: %s", s))
	}
}
```

По итогу: enum в Go это просто набор констант.