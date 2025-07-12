
base64 кодирование необходимо для перевода бинарных данных в текстовую строку, чтобы их можно передавать в виде текста, который везде одинаково может быть обработан.

```run-go
package main

import (
	"fmt"
	b64 "encoding/base64"
)

func main() {
	data := "abc123!?$*&()'-=@~"
	
	sEnc := b64.StdEncoding.EncodeToString([]byte(data))
	fmt.Println(sEnc)
	
	sDec, _ := b64.StdEncoding.DecodeString(sEnc)
	fmt.Println(string(sDec))
	fmt.Println()
	
	// использование URL-совместимого формата для base64
	uEnc := b64.URLEncoding.EncodeToString([]byte(data))
	fmt.Println(uEnc)
	uDec, _ := b64.URLEncoding.DecodeString(uEnc)
	fmt.Println(string(uDec))
}
```

**URL-совместимое base64 кодирование** — это версия base64, в которой символы `+` и `/` заменены на `-` и `_`, чтобы строка могла безопасно использоваться в **URL и файлах cookie** без необходимости URL-эскейпинга (`%2B`, `%2F` и т.д.)