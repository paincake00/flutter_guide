
## Класс Future

Ключевым классом для определения асинхронных задач является класс Future. Класс Future представляет результат отложенной операции, которая завершит свое выполнение в будущем. Результатом операции может быть некоторое значение или ошибка.

Объект Future может находиться в двух состояниях: незавершенном (Uncompleted) и завершенном (Completed).

```run-dart
Future<void> getMessage() {
	return Future.delayed(Duration(seconds: 3), () => print("res"));
}

void main() {
	getMessage();
	print('Checking...');
}
```

### Конструкторы Future

Для создания объекта Future можно использовать один из его конструкторов:

- `Future(FutureOr<T> computation())`: создает объект future, который с помощью метода Timer.run запускает функцию computation асинхронно и возвращает ее результат.

- Тип `FutureOr<T>` указывает, что функция computation должна возвращать либо объект `Future<T>` либо объект типа `T`. Например, чтобы получить объект `Future<int>`, функция computation должна возвращать либо объект `Future<int>`, либо объект `int`    

- `Future.delayed(Duration duration, [FutureOr<T> computation()])`: создает объект Future, который запускается после временной задержки, указанной через первый параметр Duration. Второй необязательный параметр указывает на функцию, которая запускается после этой задержки.
 
- `Future.error(Object error, [StackTrace stackTrace])`: создает объект Future, который содержит информацию о возникшей ошибке.
 
- `Future.microtask(FutureOr<T> computation())`: создает объект Future, который с помощью функции scheduleMicrotask запускает функцию computation асинхронно и возвращает ее результат.

- `Future.sync(FutureOr<T> computation())`: создает объект Future, который содержит результат немедленно вызываемой функции computation.

- `Future.value([FutureOr<T> value])`: создает объект Future, который содержит значение value.