
Dependencies:

```yaml
dependencies:
	auto_route:
dev_dependencies:
	build_runner:
	auto_route_generator:
```

Запуск генерации:
```
flutter pub run build_runner build
```

```
flutter pub run build_runner build --delete-conflicting-outputs
```

### Guide

Создаем конфиг файл с классами (например, `router.dart`), указав импорт класса-виджета. 

Первый пример (для более гибкой настройки через наследование от `RootStackRouter`):

```dart
@AutoRouterConfig(replaceInRouteName: 'ScreenWidget|Screen|Widget,Route')
class AppRouter extends RootStackRouter {

  @override
  RouteType get defaultRouteType => const RouteType.material();

  @override
  List<AutoRoute> get routes => [
    AutoRoute(page: HomeScreen.page, path: '/home'),
    AutoRoute(page: ProfileScreen.page, path: '/profile'),
  ];
}
```
В поле `replaceInRouteName` указывается строчка, где до запятой перечисляются окончания в названиях классах/виджетах (через `|`), из которых будут созданы роуты, а после запятой уже пишется на что окончание будет заменено.
- `HomeScreenWidget` → `HomeRoute`
- `ProfileScreen` → `ProfileRoute`
- `SettingsWidget` → `SettingsRoute`

Второй способ более упрощенный, где все роуты можно сразу перечислить в теле аннотации:

```dart
@MaterialAutoRouter(
  replaceInRouteName: 'Screen,Route',
  routes: <AutoRoute>[
    AutoRoute(page: HomeScreen, path: '/home'),
    AutoRoute(page: ProfileScreen, path: '/profile'),
  ],
)
class $AppRouter {}
```
Все маршруты будут использовать `MaterialPageRoute`.

Добавляем в наше приложение router через MaterialApp.router:
```dart
final _appRouter = AppRouter()

. . .

Widget build(BuildContext context){
    return MaterialApp.router(
        routerDelegate: _appRouter.delegate(),
        routeInformationParser: _appRouter.defaultRouteParser(),
    ),
}
```

`routerDelegate`:

Основные задачи:
    - Управляет текущим состоянием навигации (например, какой экран сейчас отображается).
    - Реагирует на команды навигации, такие как `push`, `pop`, или `replace`.
    - Обеспечивает связь между системой маршрутизации и виджетами приложения.
Пример: Если вы вызываете `context.router.push(HomeRoute())`, `RouterDelegate` обрабатывает эту команду и обновляет состояние навигации, чтобы отобразить экран `Home`.

`routeInformationParser`:

Основные задачи:
    - Разбирает информацию о маршруте (например, URL) и преобразует ее в объект маршрута (например, `HomeRoute`).
    - Может использоваться для обработки глубоких ссылок (deep links) или восстановления состояния навигации при перезапуске приложения.



В классе-виджете прописываем аннотацию `@RoutePage()` (дополнительно можно в атрибуте `name` указать имя роута) и запускаем генерацию.

`AutoRouter` отвечает за отображение текущего активного маршрута, который определяется состоянием навигации.

1. **Дочерние маршруты (children)** показываются **внутри** родительского экрана:
    
    - Обычно в специальном контейнере (например, TabView, Nested Router)
    - Родительский экран остается видимым (частично или полностью)
    - Требуется, чтобы родитель имел способ отобразить потомка
2. **Соседние маршруты (siblings)** **заменяют** текущий экран:
    
    - Полноэкранная замена текущего экрана
    - Переход происходит как между независимыми экранами
    - Подходит для последовательной навигации вперед/назад