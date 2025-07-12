
Для структур можно использовать теги структур (struct tags), которые позволяют прикреплять к полям структур метаданные.

В `encoding/json` тегами указываются имена для JSON-файла.

Сама JSON-строка, которая создается при сериализации, является срезом из байтов. 

К тому же немного об интерпретируемых и неинтерпретируемых строках:

| Интерпретируемая строка   | "..."   | Поддерживаются escape-последовательности (`\n`, `\t`, `\"`)   |
| ------------------------- | ------- | ------------------------------------------------------------- |
| Неинтерпретируемая строка | \`...\` | Никаких escape-последовательностей, можно писать многострочно |


```run-go
package main

import (
	"fmt"
	"encoding/json"
	"os"
	"strings"
)

type response1 struct {
	Page int
	Fruits []string
}

type response2 struct {
	// поля с заглавной буквы, чтобы могли быть экспортированы в json
	Page int `json:"page"`
	Fruits []string `json:"fruits"`
}

func main() {
	// json.Marshal - кодирование в JSON
	// Перевод в JSON-строку базовых типов данных
	bolB, _ := json.Marshal(true)
	fmt.Println(string(bolB))
	
	intB, _ := json.Marshal(1)
	fmt.Println(string(intB))
	
	fltB, _ := json.Marshal(2.34)
	fmt.Println(string(fltB))
	
	strB, _ := json.Marshal("gopher")
	fmt.Println(string(strB))

    slcD := []string{"apple", "peach", "pear"}
    slcB, _ := json.Marshal(slcD)
    fmt.Println(string(slcB))

    mapD := map[string]int{"apple": 5, "lettuce": 7}
    mapB, _ := json.Marshal(mapD)
    fmt.Println(string(mapB))
    
	res1D := &response1{
		Page: 1,
		Fruits: []string{"apple", "peach", "pear"},
	}
	res1B, _ := json.Marshal(res1D)
	fmt.Println(string(res1B))
	
	res2D := &response2{
		Page: 1,
		Fruits: []string{"apple", "peach", "pear"},
	}
	res2B, _ := json.Marshal(res2D)
	fmt.Println(string(res2B))
	
	// JSON-строка, которую надо декодировать
	byt := []byte(`{"num":6.13,"strs":["a","b"]}`)
	
	// Мапа, в которую будет записан JSON
	var dat map[string]interface{}
	
	if err := json.Unmarshal(byt, &dat); err != nil {
		panic(err)
	}
	fmt.Println(dat)
	
	// явное приведение типа
	num := dat["num"].(float64)
	fmt.Println(num)
	
	strs := dat["strs"].([]interface{})
	str1 := strs[0].(string)
	fmt.Println(str1)
	
	str := `{"page": 1, "fruits": ["apple", "peach"]}`
	res := response2{}
	// распарсинг в структуру (десериализация)
	json.Unmarshal([]byte(str), &res)
	fmt.Println(res)
	fmt.Println(res.Fruits[0])
	
	// Для работы с потоками, HTTP-подключениями, файлами, WebSockets и т.д.
	// лучше использовать Encoder и Decoder
	enc := json.NewEncoder(os.Stdout)
	d := map[string]int{"apple": 5, "lettuce": 7}
	enc.Encode(d) // после енкода будет выведено в консоль т.к. Stdout
	
	dec := json.NewDecoder(strings.NewReader(str))
	res1 := response2{}
	dec.Decode(&res1)
	fmt.Println(res1)
}
```
