
Переменные среды (env) являются универсальным механизмом передачи информации о конфигурации в программы Unix.

built-in способом:
```go
package main
  
import (
	"fmt"
	"os"
	"strings"
)
  
func main() {
	os.Setenv("FOO", string(1))
	  
	fmt.Println("FOO: ", os.Getenv("FOO")
	fmt.Println("BAR: ", os.Getenv("BAR"))
	  
	fmt.Println()
	// перебрать все пары ключ-значение в env
	for _, e := range os.Environ() {
		pair := strings.SplitN(e, "=", 2)
		fmt.Println(pair[0])
	}
}
```

Output:
```
FOO:  
BAR:  

MallocNanoZone
USER
COMMAND_MODE
__CFBundleIdentifier
LOGNAME
SSH_AUTH_SOCK
HOME
SHELL
TMPDIR
__CF_USER_TEXT_ENCODING
XPC_SERVICE_NAME
XPC_FLAGS
ORIGINAL_XDG_CURRENT_DESKTOP
SHLVL
PWD
OLDPWD
HOMEBREW_PREFIX
HOMEBREW_CELLAR
HOMEBREW_REPOSITORY
INFOPATH
JAVA_HOME
ZSH
PAGER
LESS
LSCOLORS
LS_COLORS
SDKMAN_DIR
SDKMAN_CANDIDATES_API
SDKMAN_PLATFORM
SDKMAN_CANDIDATES_DIR
GRADLE_HOME
TERM_PROGRAM
TERM_PROGRAM_VERSION
LANG
COLORTERM
GIT_ASKPASS
VSCODE_GIT_ASKPASS_NODE
VSCODE_GIT_ASKPASS_EXTRA_ARGS
VSCODE_GIT_ASKPASS_MAIN
VSCODE_GIT_IPC_HANDLE
TERM
_
PATH
FOO
```

### Чтение из файла

Получение переменных из файла `.env` с помощью библиотеки `github.com/joho/godotenv`:
```go
package main
  
import (
	"fmt"
	"os"
	  
	"github.com/joho/godotenv"
)
  
// Добавить модуль: go get github.com/joho/godotenv
  
func main() {
	err := godotenv.Load()
	if err != nil {
		fmt.Println("Не удалось загрузить .env")
	}
	
	fmt.Printf("DB_USER=%s\n", os.Getenv("DB_USER"))
	fmt.Printf("DB_PASSWORD=%s\n", os.Getenv("DB_PASSWORD"))
	fmt.Printf("PORT=%s\n", os.Getenv("PORT"))
}
```

Output:
```sh
DB_USER=admin
DB_PASSWORD=secret
PORT=8080
```

После использования эти переменные среды очищаются из `os.Environ`.