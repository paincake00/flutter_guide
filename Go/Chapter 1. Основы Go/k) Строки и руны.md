
Строка в Go - это read-only (имутабельна) срез байтов. При переборе по индексам можно получить байты из этой строки. Но с помощью for range loop можно сразу декодировать в руну (rune) формата UTF-8 каждую взятую из строки кодовую точку Unicode (можно посмотреть через %U и %+q). 

```run-go
package main

import (
    "fmt"
    "unicode/utf8"
)

func main() {

    const s = "[{}()]{}" //"สวัสดี"

    fmt.Println("Len:", len(s))

    for i := 0; i < len(s); i++ {
        fmt.Printf("%x ", s[i])
    }
    fmt.Println()

    fmt.Println("Rune count:", utf8.RuneCountInString(s))

    for idx, runeValue := range s {
        fmt.Printf("%c starts at %d\n", runeValue, idx)
    }

    fmt.Println("\nUsing DecodeRuneInString")
    for i, w := 0, 0; i < len(s); i += w {
        runeValue, width := utf8.DecodeRuneInString(s[i:])
        fmt.Printf("%#U starts at %d\n", runeValue, i)
        w = width

        examineRune(runeValue)
    }
}

func examineRune(r rune) {

    if r == 't' {
        fmt.Println("found tee")
    } else if r == 'ส' {
        fmt.Println("found so sua")
    }
}
```

### Функции для работы со строками:

- **ToUpper()**: преобразует каждый символ строки в верхний регистр

- **ToLower()**: используется для преобразования каждого символа строки в нижний регистр. 

- **HasPrefix()**: проверяет, начинается ли строка с указанной строки или нет

- **HasSuffix**: проверяет, заканчивается ли строка указанной строкой.

- **Contains()**: проверяет, содержит ли строка определенную подстроку

- **ContainsAny()**: проверяет, имеется какой-либо символ строки в другой строке

- **Count()**: сообщает, сколько раз определенная подстрока встречается в строке

- **Join()**: объединяет все элементы массива в одну строку.

- **Replace()**: заменяет в строку одну подстроки на другую.

- **ReplaceAll**: заменяет все вхождения старой подстроки на новую

- **Split()**: возвращает массив подстрок из строки, разделенных заданным разделителем

- **Fields()**: разбивает строку по пробелам или символам новой строки и возвращает массив

- **Trim** / **TrimLeft** / **TrimRight**: удаляет определенные символы в начале и (или) в конце строки

- **TrimFunc** / **TrimLeftFunc** / **TrimRightFunc**: удаляет символы, которые соответствуют определенному условию, в начале и (или) в конце строки

- **TrimSpace**: удаляет все начальные и конечные пробелы.

- **TrimPrefix**: удаляет подстроку из начала строки.

- **TrimSuffix**: удаляет подстроку из конца строки.

- **Index()**: возвращает индекс первого вхождения подстроки в строке.

- **LastIndex()**: ищет последнее вхождение подстроки в строку

- **IndexAny()**: возвращает первый индекс любого из найденных символов подстроки в строке.

- **LastIndexAny()**: возвращает последнее вхождение символа в строке.

```run-go
package main

import (
    "fmt"
    "strings"
)

func main() {
    // Обрезка слева и справа
    fmt.Println(strings.Trim("01.2300", "0")) // 1.23

    // Обрезка только слева
    fmt.Println(strings.TrimLeft("01.2300", "0")) // 1.2300

    // Обрезка только справа
    fmt.Println(strings.TrimRight("01.2300", "0")) // 01.23

    str := "+7-987-654-32-10!"

    // Обрезка по функции — оставляем только цифры (с обеих сторон)
    fmt.Println(strings.TrimFunc(str, func(c rune) bool {
        return c < '0' || c > '9'
    })) // 7-987-654-32-10

    // Обрезка слева по условию
    fmt.Println(strings.TrimLeftFunc(str, func(c rune) bool {
        return c < '0' || c > '9'
    })) // 7-987-654-32-10!

    // Обрезка справа по условию
    fmt.Println(strings.TrimRightFunc(str, func(c rune) bool {
        return c < '0' || c > '9'
    })) // +7-987-654-32-10

    // Обрезка префикса
    fmt.Println(strings.TrimPrefix("+7 987 654 32 10;", "+7")) // 987 654 32 10;

    // Обрезка суффикса
    fmt.Println(strings.TrimSuffix("+7 987 654 32 10;", ";")) // +7 987 654 32 10

    // Удаление пробелов с начала и конца
    fmt.Println(strings.TrimSpace(" 7 987 654 32 10 ")) // 7 987 654 32 10
}
```
