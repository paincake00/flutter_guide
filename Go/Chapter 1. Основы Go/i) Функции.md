
### Обычные функции:

```run-go
package main

import "fmt"

func plus(a int, b int) int {

    return a + b
}

func plusPlus(a, b, c int) int {
    return a + b + c
}

func main() {

    res := plus(1, 2)
    fmt.Println("1+2 =", res)

    res = plusPlus(1, 2, 3)
    fmt.Println("1+2+3 =", res)
}
```

### Множественный возврат

```run-go
package main

import "fmt"

func val() (int, int) {
	return 3, 7
}

func main() {
	a, b := val()
	fmt.Println(a, b)
}
```

### Произвольное количество параметров

```run-go
package main

import "fmt"

func sum(nums ...int) {
	fmt.Print(nums, " -s-u-m-> ")
	
	total := 0
	for _, e := range nums {
		total += e
	}
	fmt.Println(total)
}

func main() {
	sum(5, 6)
	sum(1, 2, 3)
	
	nums := []int{1, 3, 5, 6}
	sum(nums...)
}
```

### Анонимные функции

```run-go
package main

import "fmt"

func main() {
	f := func(x, y int) int {return x+y}
	
	fmt.Println(f(5, 7))
}
```

### Closures (замыкания)

Замыкание в Go - это анонимная функция (та, у которой нет имени), которая **захватывает переменные из окружающей области видимости**.

```run-go
package main

import "fmt"

func initSeq() func() int {
	i := 0
	return func() int {
		i++
		return i
	}
}

func main() {
	nextInt := initSeq()
	
	fmt.Println(nextInt())
	fmt.Println(nextInt())
	fmt.Println(nextInt())
	
	// var newInts func() int = initSeq()
	newInts := initSeq()
	
	fmt.Println(newInts())
}
```

### Recursion

```run-go
package main

import "fmt"

func fact(n int) int {
	if n == 0 {
		return 1
	}
	
	return n * fact(n-1)
}

func main() {
	fmt.Printf("factorial: %d\n", fact(7))
	
	var fib func(n int) int
	
	fib = func(n int) int {
		if n < 2 {
			return n
		}
		
		return fib(n-1) + fib(n-2)
	}
	
	fmt.Println("fibonacci: ", fib(7))
}
```
