
`strconv` - для парсинга чисел из строк.

```run-go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	f, _ := strconv.ParseFloat("1.234", 64)
	fmt.Println(f)
	
	// 0 - значит система счисления 
	// определяется из префикса строки автоматически
	i, _ := strconv.ParseInt("123", 0, 64)
	fmt.Println(i)
	
	d, _ := strconv.ParseInt("0x1c8", 0, 64)
	fmt.Println(d)
	
	u, _ := strconv.ParseUint("789", 0, 64)
	fmt.Println(u)
	
	k, _ := strconv.Atoi("135") // "ASCII to Integer"
	fmt.Println(k)
	
	_, e := strconv.Atoi("wat")
	fmt.Println(e)
}
```
