
`fx.Module` — это **способ сгруппировать зависимые компоненты** (providers, invokes, decorators) в один “блок” и зарегистрировать их в приложении Fx.

```go
// authfx/module.go
package authfx

import (
    "go.uber.org/fx"
)

var Module = fx.Module("authfx",
    fx.Provide(NewAuthService),
    fx.Invoke(RegisterHandlers),
)
```

```go
// main.go
package main

import (
    "go.uber.org/fx"
    "myapp/authfx"
    "myapp/db"
)

func main() {
    app := fx.New(
        db.Module,
        authfx.Module,
    )
    app.Run()
}
```