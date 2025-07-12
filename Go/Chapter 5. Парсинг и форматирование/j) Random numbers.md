
```run-go
package main

import (
	"fmt"
	"math/rand/v2"
)

func main() {
	fmt.Println(rand.IntN(100)) // [0, n-1]
	
	fmt.Println(rand.Float64())
	fmt.Println((rand.Float64()*5)+5)
	
	// можно использовать с определенным семенем
	s2 := rand.NewPCG(42, 1024) // seed из двух uint64 чисел
	r2 := rand.New(s2)
	fmt.Println("s2: ", r2.IntN(100))
	
	s3 := rand.NewPCG(42, 1024)
	r3 := rand.New(s3)
	fmt.Println("s3: ", r3.IntN(100))
}
```
