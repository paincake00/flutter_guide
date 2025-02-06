
Source 1: https://docs.flutter.dev/cookbook/testing/unit
Source 2: https://pub.dev/packages/mocktail/example

Unit-тесты удобны для проверки поведения одной функции, метода или класса.

Пакет `test` обеспечивает основную основу для написания модульных тестов, а пакет `flutter_test` предоставляет дополнительные утилиты для тестирования виджетов.

## 1. Add the test dependency

```sh
flutter pub add dev:test
```

## 2. Create a test file

Создайте два файла: `counter.dart` и `counter_test.dart`.

Файл counter.dart содержит класс, который вы хотите протестировать, и находится в папке lib. Файл `counter_test.dart` (всегда должен заканчиваться на `_test.dart`) содержит сами тесты и живет внутри тестовой папки.

Такая структура у приложения:
```
counter_app/
  lib/
    counter.dart
  test/
    counter_test.dart
```

## 3. Create a class to test

Далее вам нужно "unit" для тестирования. Помните: "unit" - это другое название функции, метода или класса.

Создадим класс для тестирования:
```dart
class Counter {
	int value = 0;
	
	void increment() => value++;
	
	void decrement() => value--:
}
```

## 4. Write a test for our class

Внутри файла `counter_test.dart` запишите первый unit-тест. Тесты определяются с помощью `test` - функции верхнего уровня, и вы можете проверить правильность результатов с помощью `expect` - функции верхнего уровня:

```dart
import 'package:counter_app/counter.dart'; 
import 'package:test/test.dart';

void main() {
	test('Counter value should be incremented', () {
		final counter = Counter();
		
		counter.increment();
		
		expect(counter.value, 1);
	});
}
```

## 5. Combine multiple tests in a `group`

Если вы хотите запустить серию связанных тестов, используйте `group` функцию пакетов `flutter_test` для категоризации тестов:
```dart
import 'package:counter_app/counter.dart';
import 'package:test/test.dart';

void main() {
  group('Test start, increment, decrement', () {
    test('value should start at 0', () {
      expect(Counter().value, 0);
    });

    test('value should be incremented', () {
      final counter = Counter();

      counter.increment();

      expect(counter.value, 1);
    });

    test('value should be decremented', () {
      final counter = Counter();

      counter.decrement();

      expect(counter.value, -1);
    });
  });
}
```

## 6. Run the tests

**VS Code**:
1. Откройте файл counter_test.dart
2. Перейдите в «**Run**» > «**Start Debuging**». Вы также можете нажать соответствующую комбинацию клавиш для вашей платформы.

**Terminal**:

```sh
flutter test test/counter_test.dart
```

Чтобы запустить все тесты, которые вы поместили в одну `group`, выполните следующую команду из корня проекта:

```sh
flutter test --plain-name "Test start, increment, decrement"
```


# Mocktail

Пакет необходимый для реализации Mock-объектов (тестовых) и работы с ними.

Mocktail даёт удобный способ создавать и настраивать mock-объекты, «заглушать» их поведение (задавать кастомное через `when`), а также проверять взаимодействие вашего кода с ними.

Пример использования `mocktail`:
```dart
import 'package:mocktail/mocktail.dart';
import 'package:test/test.dart';

class Food {}

class Chicken extends Food {}

class Tuna extends Food {}

// A Real Cat class
class Cat {
  String sound() => 'meow!';
  bool likes(String food, {bool isHungry = false}) => false;
  void eat<T extends Food>(T food) {}
  final int lives = 9;
}

// A Mock Cat class
class MockCat extends Mock implements Cat {}

void main() {
  group('Cat', () {
    setUpAll(() {
      // Здесь мы регистрируем «запасные значения» (fallbackValue) 
      // для типов Chicken и Tuna, чтобы mocktail понимал, 
      // какие объекты могут попадать в any() и captureAny().
      registerFallbackValue(Chicken());
      registerFallbackValue(Tuna());
    });

    late Cat cat;

	// Выполняем перед каждым тестом. 
	// Создаём новый MockCat.
    setUp(() {
      cat = MockCat();
    });

    test('example', () {
      // «Заглушаем» (stub) метод sound() у MockCat, указывая, 
      // что при вызове будет возвращаться строка "purr".
      when(() => cat.sound()).thenReturn('purr');

      // Проверяем, что при вызове sound() 
      // теперь действительно вернётся "purr".
      expect(cat.sound(), 'purr');

      // Убеждаемся, что метод sound() 
      // был вызван ровно один раз.
      verify(() => cat.sound()).called(1);

      // «Заглушаем» метод likes(), чтобы при вызове 
      // с параметрами (fish, any isHungry) вернуть true.
      when(
        () => cat.likes('fish', isHungry: any(named: 'isHungry')), // мы ожидаем любое значение для именованного параметра «isHungry» (true/false)
      ).thenReturn(true);
	  
	  // Проверяем
      expect(cat.likes('fish', isHungry: true), isTrue);

      // Убеждаемся, что вызван был один раз
      verify(() => cat.likes('fish', isHungry: true)).called(1);

      // Вызываем eat() дважды, с разными типами еды.
      cat
        ..eat(Chicken())
        ..eat(Tuna());

	  // Проверяем, что кот ел Chicken один раз.
      verify(() => cat.eat<Chicken>(any())).called(1);
      // Проверяем, что кот ел Tuna один раз.
      verify(() => cat.eat<Tuna>(any())).called(1);
      // Убеждаемся, что не было вызова eat<Food>(),
      // то есть общий тип не вызывался.
      verifyNever(() => cat.eat<Food>(any()));
    });
  });
}
```

# Testing with WidgetTester (For Flutter)

Source: https://medium.com/@arunb9525/a-complete-guide-to-widget-testing-in-flutter-ensuring-ui-reliability-and-quality-802fd1c91aa3

```dart
import 'package:flutter/material.dart';  
  
class CounterApp extends StatefulWidget {  
	@override  
	_CounterAppState createState() => _CounterAppState();  
}  

class _CounterAppState extends State<CounterApp> {  
	int _counter = 0;  
	void _incrementCounter() {  
		setState(() {  
			_counter++;  
		});  
	}  
	
	@override  
	Widget build(BuildContext context) {  
		return MaterialApp(  
			home: Scaffold(  
				appBar: AppBar(title: Text('Counter App')),  
				body: Center(  
					child: Column(  
						mainAxisAlignment: MainAxisAlignment.center,  
						children: <Widget>[  
							Text('Counter: $_counter'),  
							ElevatedButton(  
								onPressed: _incrementCounter,  
								child: Text('Increment'),  
							),  
						],  
					),  
				),  
			),  
		);  
	}  
}
```

```dart
import 'package:flutter/material.dart';  
import 'package:flutter_test/flutter_test.dart';  
import 'counter_app.dart'; // The path to your CounterApp widget  
  
  
  
void main() {  
	testWidgets('Counter increments when the button is pressed', (WidgetTester tester) async {  
	// Step 1: Build the app  
	await tester.pumpWidget(CounterApp());  
	// Step 2: Find the necessary widgets  
	final counterText = find.text('Counter: 0');  
	final incrementButton = find.text('Increment');  
	// Step 3: Verify initial state  
	expect(counterText, findsOneWidget);  
	// Step 4: Tap the increment button  
	await tester.tap(incrementButton);  
	// Step 5: Rebuild the widget after the state change  
	await tester.pump();  
	// Step 6: Verify the updated state  
	expect(find.text('Counter: 1'), findsOneWidget);  
	});  
}
```

Объяснение:

- tester.pumpWidget(CounterApp()): Этот метод создает и отображает дерево виджетов для виджета CounterApp.

- find.text('Counter: 0'): Это поиск виджета, отображающего текст «Counter: 0».

- tester.tap(incrementButton): Это имитирует нажатие на кнопку «Increment».

- tester.pump(): Это запускает восстановление дерева виджетов, позволяя Flutter отражать любые изменения состояния после нажатия кнопки.

- expect(find.text('Counter: 1'), findsOneWidget): Это утверждение проверяет, что текст «Counter: 1» появляется после нажатия кнопки, подтверждая, что счетчик увеличился.