
Unit-тесты можно делать с помощью пакета testing. Файл с тестами должен оканчиваться на `_test.go` по конвенции.

Запуск через `go test -v`. 

```go
package main
  
import (
	"testing"
)
  
func IntMin(a, b int) int {
	if a < b {
		return a
	}
	return b
}
  
func TestIntMinBasic(t *testing.T) {
	ans := IntMin(2, -2)
	if ans != -2 {
		t.Errorf("IntMin(2, -2) = %d; want: -2", ans)
	}
}
```

Результат:
```
=== RUN   TestIntMinBasic
--- PASS: TestIntMinBasic (0.00s)
PASS
ok      starter 0.155s
```

Метод `t.Run()` позволяет запускать подтесты в основной функции теста.

```go
package main
  
import (
	"fmt"
	"testing"
)
  
func IntMin(a, b int) int {
	if a < b {
		return a
	}
	return b
}

func TestIntMinTableDriven(t *testing.T) {
	type mock struct {
		a, b int
		want int
	}
	
	var tests = []mock{
		{0, 1, 0},
        {1, 0, 0},
        {2, -2, -2},
        {0, -1, -1},
        {-1, 0, -1},
	}
	
	for _, tt := range tests {
		testname := fmt.Sprintf("%d, %d", tt.a, tt.b)
		t.Run(testname, func(t *testing.T) {
			ans := IntMin(tt.a, tt.b)
			if ans != tt.want {
				t.Errorf("got %d, want %d", ans, tt.want)
			}
		})
	}
}
```

Результат:
```
=== RUN   TestIntMinTableDriven
=== RUN   TestIntMinTableDriven/0,_1
--- PASS: TestIntMinTableDriven/0,_1 (0.00s)
=== RUN   TestIntMinTableDriven/1,_0
--- PASS: TestIntMinTableDriven/1,_0 (0.00s)
=== RUN   TestIntMinTableDriven/2,_-2
--- PASS: TestIntMinTableDriven/2,_-2 (0.00s)
=== RUN   TestIntMinTableDriven/0,_-1
--- PASS: TestIntMinTableDriven/0,_-1 (0.00s)
=== RUN   TestIntMinTableDriven/-1,_0
--- PASS: TestIntMinTableDriven/-1,_0 (0.00s)
--- PASS: TestIntMinTableDriven (0.00s)
PASS
ok      starter 0.160s
```

### Бенчмарки

Бенчмарки нужны для определения производительности кода.

```go
package main
  
import (
	"testing"
)
  
func IntMin(a, b int) int {
	if a < b {
		return a
	}
	return b
}

func BenchmarkIntMin(b *testing.B) {
	for b.Loop() {
		IntMin(1, 2)
	}
}
```

Тест-раннер будет автоматически выполнять это тело цикла много раз, чтобы определить разумную оценку времени выполнения одной итерации.

Результат:
```
goos: darwin
goarch: arm64
pkg: starter
cpu: Apple M3
BenchmarkIntMin-8       1000000000               1.000 ns/op
PASS
ok      starter 1.157s
```