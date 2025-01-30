
Key - это идентификатор виджета и его места в дереве. Он важен для обновления Element.

### Назначение Key

Ключи необходимы для следующих случаев:

1. **Уникальная идентификация**: Ключи позволяют однозначно определить виджет в дереве виджетов.
2. **Сохранение состояния**: Ключи помогают сохранять состояние виджетов при обновлении дерева виджетов.
3. **Определение родительских отношений**: Ключи позволяют определить родительские отношения между виджетами.

### Разновидности ключей

- `GlobalKey` используется для доступа к состоянию виджета из любого места в приложении. Это позволяет вам управлять состоянием виджета, даже если он находится на разных уровнях дерева виджетов.
- `LocalKey` используется для создания ключей, которые имеют локальную область видимости в дереве виджетов. Включает `ValueKey`, `ObjectKey` и `UniqueKey`. 

### Local Keys

1. **UniqueKey**: это специальный тип ключа, который всегда генерирует уникальное значение при каждом создании. С помощью можно принудительно пересоздать виджет при перестройке.

2. **ValueKey**: используется для идентификации виджетов по их значению. Например, если у вас есть список виджетов с текстом, вы можете использовать `ValueKey` с текстом в качестве ключа. 

К примеру:
```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Text(
          'Hello, World!',
          key: ValueKey('hello_world'), // Используем ValueKey
        ),
      ),
    );
  }
}
```
В этом примере мы используем `ValueKey` с текстом `'hello_world'` в качестве ключа для виджета `Text`.

3. **ObjectKey**: используется для идентификации виджетов по объекту. Например, если у вас есть список объектов и вы хотите связать каждый объект с виджетом, вы можете использовать `ObjectKey` с объектом в качестве ключа.

Реализация автоскролла для дропдаун меню с использованием ValueKey:

```dart
...
child: Column(
	crossAxisAlignment: CrossAxisAlignment.start,
	children: [
		...dropdowns.mapIndexed(
			(index, dropdown) => Column(
				children: [
					AutoScrollTag(
						key: ValueKey(index),
						controller: scrollController,
						index: index,
						child: _DropdownTile(
							dropdown: dropdown,
							onExpansionEnded: 
								() => onExpansionEnded?.call(index),
							status: isAuthorized
								? isCardExist
								? Love20StatusType.authorizedWithCard
								: Love20StatusType.authorizedWithoutCard
								: Love20StatusType.notAuthorized,
						),
					),
					const Divider(
						height: 0,
						thickness: 1,
						color: grayLine,
					),
				],
			),
		),
	],
),
...
```