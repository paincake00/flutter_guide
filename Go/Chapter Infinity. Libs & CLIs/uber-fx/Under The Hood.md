
the article: https://habr.com/ru/companies/kaspersky/articles/699994/


Функция `main` и boilerplate (шаблонный код) для инициализации `fx`:

```go
func main() {
	fx.New(CreateApp()).Run()
}

func CreateApp() fx.Options {
	return fx.Options(
		// fx.Supply(),
		fx.Provide(
			// . . .
		),
		fx.Invoke(
			// . . .
		),
	)
}
```

А дальше и начинается структурирование:  
`fx.Provide` — объявление компонентов (зависимостей)
`fx.Invoke` — инициализация, код

Декларация компонентов. Перечисляем конструкторы (именно функции) компонентов в `fx.Provide`:

```go
func CreateApp() fx.Options {
	return fx.Options(
		fx.Provide(
			fxshow.NewRepo1,
			fxshow.NewRepo2,
			fxshow.NewUserRepo,
			fxshow.NewUserManager,
			fxshow.NewCache,
			fxshow.NewDataCombiner,
			fxshow.NewWeb,
		),
		fx.Invoke(
			// . . .
		),
	)
}
```

Все перечисленные конструкторы теперь перейдут под управление DI-контейнера `uber.fx`.

#### DI-контейнер

**Основная его задача**: избавить разработчика от необходимости вручную расставлять аргументы во всех конструкторах и создавать экземпляры компонентов в нужной последовательности.

Компоненты могут образовывать такой граф из зависимостей:
![[Pasted image 20250721192410.png|500]]

В зависимости от вызовов, компоненты образуют слоистую структуру, где управляющие (нижние) компоненты отделяются от частных (верних) с помощью интерфейсов:

![[Pasted image 20250721193642.png|600]]

Кстати, чтобы проверить граф на целостность (например, что все зависимости присутствуют), то можно использовать простой unit-тест:
```go
func TestValidateApp(t *testing.T) {
	err := fx.ValidateApp(CreateApp())
	require.NoError(t, err)
}
```

Можно также при необходимости зарегистрировать результат конструктора (который возвращает структуру) как реализацию определенного интерфейса:
```go
func CreateApp() fx.Options {
	return fx.Options(
		fx.Provide(
			fxshow.NewRepo1,
			fxshow.NewRepo2,
			fx.Annotate(
				fxshow.NewUsersRepo, // конструктор
				fx.As(new(fxshow.UserAccessor)), // интерфейс
			),
			fxshow.NewUserManager,
			fxshow.NewCache,
			fxshow.NewDataCombiner,
			fxshow.NewWeb,
		),
	)
}
```

#### Performance

Контейнер производит «склейку» ваших компонентов и резолвит («разрешает», если говорить по-русски) зависимости через reflection только один раз. На общую производительность вашей логики или вашей основной работы сервиса он не повлияет.

#### Запуск `fx.Invoke`

Вызов функций производится в `fx.Invoke`: 

```go
func CreateApp() fx.Option {
	return fx.Options(
			fx.Invoke(
				preloadCache,
				IprintGraph,
				registerRunners,
			),
		),
	)
}

func preloadCache(cc fxshow.Cacher) error {
	// ....
}

func printGraph(diGraph fx.DotGraph) {
	// ....
	// ....
}

func registerRunners(cc *fxshow.Cache,
	w *fxshow.Web, lc fx.Lifecycle) {
	// ....
	// ....
}
```

#### Lifecycle

Можно создать компонент, запуском и остановкой которого можно управлять через жизненный цикл `fx.Lifecycle`:

```go
func CreateApp() fx.Option {
	return fx.Options(
		fx.Invoke(
			func(web *fxshow.Web, lc fx.Lifecycle) error {
				lc.Append(
					fx.Hook{
						OnStart: func(ctx context.Context) (err error) {
							// ....
							return err
						},
						OnStop: func(ctx context.Context) (err error) {
							// ....
							return err
						},
					},
				)
				return nil
			},
		),
	)
}
```

Lifecycle вызывает последовательно функции Start при запуске приложения и точно также, но в обратном порядке, все функции Stop при остановке.

В случае непредвиденной ошибки у `fx` есть зависимость Shutdowner, у которой есть метод, который инициирует полноценную остановку:
```go
fx.Invoke(
   func(
      webSrv fxshow.Weber,
      shutdowner fx.Shutdowner,
   ) {
      // ...
      webSrv.OnFatalError(
         func(err error) {
            // ...
            _ = shutdowner.Shutdown()
         },
      )
      // ...
   },
)
```


### Заключение

На что стоит обращать внимание:

- иметь общий вид приложения в коде;
- декларативно регистрировать компоненты;
- отделять инициализацию от основной работы;
- оформлять «стейджи» инициализации приложения — разделять и властвовать;
- делать процедуры запуска и остановки в виде паттерна Start/Stop;
- обрабатывать сигнал на остановку, корректно почистив за собой в Stop;
- не забыть централизованно подключить логгер;