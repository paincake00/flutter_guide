
Некоторые инструменты командной строки, такие как go tool или git, имеют много подкоманд (subcommands), каждая из которых имеет свой собственный набор флагов.

У go tool это, например, go build и go get.

```go
package main
  
import (
	"flag"
	"fmt"
	"os"
)
  
func main() {
// определяем новую subcommand
fooCmd := flag.NewFlagSet("foo", flag.ExitOnError)
// определяем флаги для этой subcommand
fooEnable := fooCmd.Bool("enable", false, "dsc: enable")
fooName := fooCmd.String("name", "", "name")
  
// и другую subcomannd со своим флагом
barCmd := flag.NewFlagSet("bar", flag.ExitOnError)
barLevel := barCmd.Int("level", 0, "level")

if len(os.Args) < 2 {
	fmt.Println("expected 'bar' or 'foo' subcommands")
	os.Exit(1)
}
  
switch os.Args[1] {
	case "foo":
		fooCmd.Parse(os.Args[2:])
		fmt.Println("subcommand 'foo'")
		fmt.Println(" enable: ", *fooEnable)
		fmt.Println(" name: ", *fooName)
		fmt.Println(" tail: ", fooCmd.Args()
	case "bar":
		barCmd.Parse(os.Args[2:])
		fmt.Println("subcommand 'bar'")
		fmt.Println(" level: ", *barLevel)
		fmt.Println(" tail: ", barCmd.Args())
	default:
		fmt.Println("expected 'bar' or 'foo' subcommands")
		os.Exit(1)
	}
}
```

Вывод для `foo`:
```sh
$ ./main foo -enable -name=fff as 

subcommand 'foo'
 enable:  true
 name:  fff
 tail:  [as]
```

Вывод для `bar`:
```sh
$ ./main bar -level=10

subcommand 'bar'
 level:  10
 tail:  []
```