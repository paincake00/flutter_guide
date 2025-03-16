
Source: https://medium.com/@amani9920/lifting-state-up-callbacks-in-flutter-d36a5a3319f8

Во Flutter, виджеты неизменяемы. То есть, один раз они создаются и их свойства не могут быть изменены. 

Если необходимо состояние обновлять, то тогда используется StatefulWidget, имеющий изменяемое состояние. Но оно отделено от самого виджета, и управляется классом State.
Всякий раз, когда состояние StatefulWidget меняется, объект State запускает метод build, чтобы перестроить widget tree с новым состоянием.

Lifting State Up (Поднятие состояния) - это паттерн проектирования во Flutter, включающий в себя перемещение состояния (state) вверх от дочернего к родительскому виджету. Это позволяет родительскому виджету контролировать состояние своих детей, и передавать его "вниз" по необходимости.

Callbacks - это функции, которые в качестве аргумента в другие функции, и вызываются в ответ на определенное событие. 
Во Flutter, callbacks часто используются для передачи данных или для обновления состояния у разных виджетов.

Вот простой код, реализующий Lifting State Up и Callbacks:

```dart
class Counter extends StatefulWidget {
	const Counter({super.key});
	
	@override
	State<Counter> createState => _CounterState();
}

class _CounterState extends State<Counter> {
	int _count = 0;
	
	void incrementCounter() {
		setState(() {
			_count++;
		});
	}
	
	@override
	Widget build(BuildContext context) {
		return Center(
			child: Column(
				children: [
					CounterDisplay(count: _count),
					CounterIncrementor(onPressed: incrementCounter),
				],
			),
		);
	}
}
```

```dart
class CounterDisplay extends StatelessWidget {
	final int count;
	
	const CounterDisplay({
		super.key,
		required this.count,
	});
	
	@override
	Widget build(BuildContext context) {
		return Text(
			count.toString(),
		);
	}
}
```

```dart
class CounterIncrementor extends StatelessWidget {
	final Function() onPressed;
	
	const CounterIncrementor({
		super.key,
		required this.onPressed,
	});
	
	@override
	Widget build(BuildContext context) {
		return ElevatedButton(
			onPressed: onPressed,
			child: const Text('+'),
		);
	}
}
```

