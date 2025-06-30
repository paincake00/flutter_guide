
Интерфейс в Go — это абстракция поведения: он описывает, какие методы должен иметь тип, чтобы его можно было использовать в каком-то контексте.

В Go интерфейсы реализуются **неявно**. Достаточно описать для конкретной структуры все методы (совпадающие по параметрам и возвращаемому типу), которые указаны в интерфейсе, чтобы его реализовать.

```run-go
package main
import (
	"fmt"
	"math"
)

type geometry interface {
	area() float64
	perim() float64
}

type rect struct {
	width, height float64
}

type circle struct {
	radius float64
}

func (r rect) area() float64 {
	return r.width * r.height
}

func (r rect) perim() float64 {
	return 2*r.width + 2*r.height
}

func (c circle) area() float64 {
	return math.Pi * c.radius * c.radius
}

func (c circle) perim() float64 {
	return 2 * math.Pi * c.radius
}

func measure(g geometry) {
	fmt.Println(g)
	fmt.Println(g.area())
	fmt.Println(g.perim())
}

func detectCircle(g geometry) {
	// Позволяет узнать текущий тип параметра с типом интерфейса
	if c, ok := g.(circle); ok {
		fmt.Println("circle with radius", c.radius)
	}
}

func main() {
	r := rect{width: 4, height: 5}
	c := circle{radius: 5}

	// rect и circle реализуют все методы, поэтому их можно передать в функции
	measure(r)
	measure(c)
	
	detectCircle(r)
	detectCircle(c)
}
```

### Реализация нескольких интерфейсов
```run-go
package main
import "fmt"

type Writer interface {
	write(string)
}

type Reader interface {
	read()
}

func writeToStream(writer Writer, text string) {
	writer.write(text)
}

func readFromStream(reader Reader) {
	reader.read()
}

type File struct {
	text string
}

func (f *File) write(text string) {
	f.text = text
	fmt.Println("Записано: ", text)
}

func (f *File) read() {
	fmt.Println("Содержится: ", f.text)
}

func main() {
	file := &File{}
	writeToStream(file, "new_data")
	readFromStream(file)
}
```

### Вложенные интерфейсы

Чтобы структура File соответствовала интерфейсу ReaderWriter, она должна реализовать методы read и write, то есть методы обоих вложенных интерфейсов:
```run-go
package main
import "fmt"

type Writer interface {
	write(string)
}

type Reader interface {
	read()
}

type ReaderWriter interface {
	Writer
	Reader
}

func writeToStream(writer ReaderWriter, text string) {
	writer.write(text)
}

func readFromStream(reader ReaderWriter) {
	reader.read()
}

type File struct {
	text string
}

func (f *File) write(text string) {
	f.text = text
	fmt.Println("Записано: ", text)
}

func (f *File) read() {
	fmt.Println("Содержится: ", f.text)
}

func main() {
	file := &File{}
	writeToStream(file, "new_data")
	readFromStream(file)
	writeToStream(file, "another_data")
	readFromStream(file)
}
```

### Полиморфизм

Структуры реализуют собственную (многообразную) логику для методов интерфейса.
```run-go
package main
import "fmt"

type Vehicle interface {
	move()
}

type Car struct {
	model string
}

type Aircraft struct {
	model string
}

func (c Car) move() {
	fmt.Println("едет", c.model)
}

func (a Aircraft) move() {
	fmt.Println("летит", a.model)
}

func main() {
	toyota := Car{model: "Toyota"}
	boeing := Aircraft{model: "Boeing"}
	
	vahicles := [...]Vehicle{toyota, boeing}
	for _, v := range vahicles {
		v.move()
	}
}
```
