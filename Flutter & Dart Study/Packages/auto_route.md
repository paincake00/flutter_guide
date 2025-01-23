
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

Создаем конфиг файл с классами, указав импорт класса-виджета:

```dart
@AutoRouterConfig()
class AppRouter extends _$AppRouter {
	@override
	List<AutoRoute> get routes => <AutoRoute>[
		...
	];
}
```

В классе-виджете прописываем аннотацию `@RoutePage()` и запускаем генерацию.