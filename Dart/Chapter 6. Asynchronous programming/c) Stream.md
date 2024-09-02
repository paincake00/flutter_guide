
## *CHEAT SHEET*
---
```run-dart
void main() {
  getNums().listen((item) =>
    print(item.toString()),
  );
}

Stream getNums() async* {
  for (int i = 0; i < 5; i++) {
    await Future.delayed(Duration(milliseconds: 500));
    yield i;
  }
}
```
---

---
```run-dart
import 'dart:async';

void main() {
  StreamController<int> controller = StreamController();
  getNums(controller);
  controller.stream.listen(
    (item) => print(item),
  );
}

Future getNums(StreamController controller) async {
  for (int i = 0; i < 5; i++) {
    await Future.delayed(Duration(milliseconds: 500));
    controller.sink.add(i); // sink принимает как sync, так async объекты
  }
}
```
---

A stream is a sequence of asynchronous events. It is like an asynchronous Iterable—where, instead of getting the next event when you ask for it, the stream tells you that there is an event when it is ready.

The function is marked with the `async` keyword, which is required when using the **await for** loop.
The following example tests the previous code by generating a simple stream of integers using an `async*` function:

```run-dart
Future<int> sumStream(Stream<int> stream) async {
  var sum = 0;
  await for (final value in stream) { // await for
    sum += value;
  }
  return sum;
}

Stream<int> countStream(int to) async* { // async* for stream
  for (int i = 1; i <= to; i++) {
    yield i;
  }
}

void main() async {
  var stream = countStream(10);
  var sum = await sumStream(stream);
  print(sum); // 55
}
```

### Working with streams
```dart
Future<int> lastPositive(Stream<int> stream) =>
    stream.lastWhere((x) => x >= 0);
```

### Methods that process a stream
```dart
Future<T> get first;
Future<bool> get isEmpty;
Future<T> get last;
Future<int> get length;
Future<T> get single;
Future<bool> any(bool Function(T element) test);
Future<bool> contains(Object? needle);
Future<E> drain<E>([E? futureValue]);
Future<T> elementAt(int index);
Future<bool> every(bool Function(T element) test);
Future<T> firstWhere(bool Function(T element) test, {T Function()? orElse});
Future<S> fold<S>(S initialValue, S Function(S previous, T element) combine);
Future forEach(void Function(T element) action);
Future<String> join([String separator = '']);
Future<T> lastWhere(bool Function(T element) test, {T Function()? orElse});
Future pipe(StreamConsumer<T> streamConsumer);
Future<T> reduce(T Function(T previous, T element) combine);
Future<T> singleWhere(bool Function(T element) test, {T Function()? orElse});
Future<List<T>> toList();
Future<Set<T>> toSet();
```

## Methods that modify a stream
```dart
Stream<R> cast<R>();
Stream<S> expand<S>(Iterable<S> Function(T element) convert);
Stream<S> map<S>(S Function(T event) convert);
Stream<T> skip(int count);
Stream<T> skipWhile(bool Function(T element) test);
Stream<T> take(int count);
Stream<T> takeWhile(bool Function(T element) test);
Stream<T> where(bool Function(T event) test);
```

# Creating streams in Dart

Создание потока можно сделать с помощью `StreamController`:
```dart
void controller = new StreamController<String>();

controller.add('Item1'); // добавляем первый элемент в поток 
```
Далее делаем установки слушателя. Он будет вызывать определенные действия для новых данных, полученных из потока:
```dart
void controller = new StreamContoller<String>();

controller.stream.listen((item) => print(item));

controller.add('Item1');
controller.add('Item2');
controller.add('Item3');
```
`listen()` возвращает `StreamSubscription` — объект подписки на поток. Вызов его метода `.cancel()` завершает подписку, освобождая ресурсы, и предупреждая вызов вашей прослушивающей функции после того, как это стало ненужным.
```dart
void controller = new StreamContoller<String>();

StreamSubscription subscription = controller.stream.listen((item) => print(item));

controller.add('Item1');
controller.add('Item2');
controller.add('Item3');

await Future.delayed(Duration(milliseconds: 500));

subscription.cancel();
```

### Пример использования в Flutter:

Класс модели:
```dart
class Model {
	int _counter = 0;
	StreamController _streamController = new StreamController<int>();

	Stream<int> get counterUpdates => _streamController.stream;

	void incrementCounter() {
		_counter++;
		_streamController.add(_counter);
	}
}
```

В `main.dart`:
```dart

class _MyHomePageState extends State<MyHomePage> {  
	int _counter = 0;  
	StreamSubscription streamSubscription;  
	
	@override  
	void initState() {    
		streamSubscription = 
			MyApp.model.counterUpdates.listen((newVal) => 
				setState(() { 
					_counter = newVal;
				}));   
		super.initState();  
	}  
	
	// Хотя этот State не будет уничтожен, пока приложение работает,   
	// хороший стиль требует освобождать подписки явно  
	
	@override  
	void dispose() {      
		streamSubscription?.cancel();      
		super.dispose();    
	}
```
Изменение счетчика (и добавление в поток):
```dart
floatingActionButton: new FloatingActionButton(            
	onPressed: MyApp.model.incrementCounter,    
	tooltip: 'Increment',    
	child: new Icon(Icons.add),    
),
```

### StreamBuilder

Вместо использования `initState()` и `setState()` для наших нужд Flutter поставляется с удобным виджетом `StreamBuilder`.
```dart
body: Center(
	child: StreamBuilder<int>(
		initialData: 0,
		stream: MyApp.model.counterUpdates,
		builder: (context, snapshot) {
			if (snapshot != null && snapshot.hasData) {
				return Text(
					snapshot.data.toString();
				);
			}
			return CircularProgressIndicator();
		},
	),
),
```