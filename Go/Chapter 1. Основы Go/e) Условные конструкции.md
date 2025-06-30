
### if-else:
```go
age := 20

if age >= 18 {
    fmt.Println("Совершеннолетний")
} else {
    fmt.Println("Несовершеннолетний")
}

// if с инициализацией
if score := getScore(); score > 90 {
    fmt.Println("Отлично!")
} else if score > 70 {
    fmt.Println("Хорошо")
} else {
    fmt.Println("Старайтесь лучше")
}
```

### switch:
```go
day := "Monday"

switch day {
	case "Monday":
	    fmt.Println("Начало недели")
	case "Friday":
	    fmt.Println("Конец рабочей недели")
	case "Saturday", "Sunday":
	    fmt.Println("Выходной")
	default:
	    fmt.Println("Обычный день")
}

// switch без выражения
switch {
	case age < 18:
	    fmt.Println("Несовершеннолетний")
	case age >= 18 && age < 65:
	    fmt.Println("Взрослый")
	default:
	    fmt.Println("Пожилой")
}
```

```run-go
package main

import (
    "fmt"
    "time"
)

func main() {

    i := 2
    fmt.Print("Write ", i, " as ")
    switch i {
    case 1:
        fmt.Println("one")
    case 2:
        fmt.Println("two")
    case 3:
        fmt.Println("three")
    }

    switch time.Now().Weekday() {
    case time.Saturday, time.Sunday:
        fmt.Println("It's the weekend")
    default:
        fmt.Println("It's a weekday")
    }

    t := time.Now()
    switch {
    case t.Hour() < 12:
        fmt.Println("It's before noon")
    default:
        fmt.Println("It's after noon")
    }

    whatAmI := func(i interface{}) {
        switch t := i.(type) {
        case bool:
            fmt.Println("I'm a bool")
        case int:
            fmt.Println("I'm an int")
        default:
            fmt.Printf("Don't know type %T\n", t)
        }
    }
    whatAmI(true)
    whatAmI(1)
    whatAmI("hey")
}
```
