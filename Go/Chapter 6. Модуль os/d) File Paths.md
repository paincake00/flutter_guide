
Пакет `filepath` предоставляет функции для разбора и построения путей к файлам таким образом, чтобы он был переносимым между операционными системами; например, dir/file в Linux против dir\file в Windows.

Метод `Join` принимает неограниченное число аргументов - строк. Он также нормализует путь, удаляя лишние разделители.

Метод `Dir` может разделять переданный путь и получать директории.

Метод `Base` похож на предыдущий, но может получать файл из пути (по сути берет последнее из пути каталога).

Метод `Split` может возвращать из пути и директории, и файлы.

`IsAbs` - проверяет абсолютный ли путь (начинается с корня или нет).

`Ext` - позволяет отделить расширение (после точки) у названия файла.

`strings.TrimSuffix` - наоборот возвращает только имя файла, отсекая его расширение.

`Rel` - находит относительный (relative) путь. Цель (target) относительно базы (base).

```run-go
package main

import (
	"fmt"
	"strings"
	"path/filepath"
)

func main() {
	path := filepath.Join("dir1", "dir2", "filename")
	fmt.Println("path: ", path)
	
	fmt.Println(filepath.Join("dir//", "filename"))
	fmt.Println(filepath.Join("dir1/../dir1", "filename"))
	
	fmt.Println("Dir(path): ", filepath.Dir(path))
	fmt.Println("Base(path): ", filepath.Base(path))
	
	fmt.Println(filepath.IsAbs("/dir1/dir2"))
	fmt.Println(filepath.IsAbs("dir1/dir2"))
	
	filename := "config.json"
	
	ext := filepath.Ext(filename)
	fmt.Println(ext)
	fmt.Println(filename)
	
	fmt.Println(strings.TrimSuffix(filename, ext))
	
	// если относительный путь невозможен относительно базы -> ошибка
	rp, err := filepath.Rel("a/b", "a/b/t/file.json")
	if err != nil {
		panic(err)
	}
	fmt.Println(rp)
	
	rp, err = filepath.Rel("a/b", "a/c/t/file.json")
	if err != nil {
		panic(err)
	}
	fmt.Println(rp)
}
```

