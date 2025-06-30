
В Go это идиоматическое сообщение об ошибках с помощью явного, отдельного возвращаемого значения:
```run-go
package main
import (
	"fmt"
	"errors"
)

func f(arg int) (int, error) {
	if arg == 42 {
		return -1, errors.New("can't work with 42")
	}
	return arg + 3, nil
}

func main() {
	for _, i := range []int{7, 42, 8} {
		if r, e := f(i); e != nil {
			fmt.Println("failed:", e)
		} else {
			fmt.Println("success", r)
		}
	}
}
```

Можно создавать кастомные ошибки через имплементацию интерфейса:
```run-go
package main

import (
    "errors"
    "fmt"
)

type argError struct {
    arg     int
    message string
}

func (e *argError) Error() string {
    return fmt.Sprintf("%d - %s", e.arg, e.message)
}

func f(arg int) (int, error) {
    if arg == 42 {

        return -1, &argError{arg, "can't work with it"}
    }
    return arg + 3, nil
}

func main() {

    _, err := f(42)
    var ae *argError
    if errors.As(err, &ae) {
        fmt.Println(ae.arg)
        fmt.Println(ae.message)
    } else {
        fmt.Println("err doesn't match argError")
    }
}
```
