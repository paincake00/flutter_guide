
`text/template` - пакет для генерации текстового вывода на основе шаблона.

```run-go
package main

import (
	"os"
	"text/template"
)

func main() {
	// создание шаблона с именем t1
	t1 := template.New("t1")
	// добавление в Parse самого шаблона
	t1, err := t1.Parse("Value is {{.}}\n")
	if err != nil {
		panic(err)
	}
	
	// Аналог: Must будет возвращать шаблон, а в случае ошибки - панику
	t1 = template.Must(t1.Parse("Value: {{.}}\n"))
	
	t1.Execute(os.Stdout, "Some text")
	t1.Execute(os.Stdout, 5)
	t1.Execute(os.Stdout, []string{
		"Go",
		"C++",
		"C#",
		"Rust",
	})
	
	// кастомная функция для создания шаблона
	Create := func(name, t string) *template.Template {
		return template.Must(template.New(name).Parse(t))
	}
	
	// для получения данных о структуре использутеся {{.FieldName}}
	t2 := Create("t2", "Name: {{.Name}}\n")
	
	t2.Execute(os.Stdout, struct {
		Name string
	}{"John"})
	
	t2.Execute(os.Stdout, map[string]string{
        "Name": "Mickey Mouse",
    })
    
    // {{if .}}...{{else}} - проверяет пустая ли строка или нет
    // Символ `-` после {{if . -}} убирает пробелы вокруг, 
    // чтобы не было лишних переносов
    t3 := Create("t3", "{{if . -}} yes {{else -}} no {{end}}\n")
    t3.Execute(os.Stdout, "not empty")
    t3.Execute(os.Stdout, "")
    
	t4 := Create("t4", "Range: {{range .}}{{.}} {{end}}\n")
	t4.Execute(os.Stdout, []string{
		"Go",
		"C++",
		"C#",
		"Rust",
	})
}
```
