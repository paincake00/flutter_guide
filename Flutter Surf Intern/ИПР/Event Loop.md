
Source: https://metanit.com/dart/tutorial/7.5.php

Event loop (цикл событий) - это механизм, позволяющий обрабатывать асинхронные операции и события в приложении.

#### Структура

В единственном потоке приложения задаются две очереди - MicroTask Queue и Event Queue.

-  MicroTask Queue - очередь, принимающая микрозадачи (microtasks). Микрозадачи - это задачи, которые должны быть выполнены как можно скорее (выполнение callback функций, обработка ошибок во время исполнения асинхронной операции и т.д.). Они не блокируют поток.

-  Event Queue - очередь, принимающая события - долговременные задачи, которые выполняются в ответ на что-то (сетевой запрос, обращение к файловой системе, запуск анимации и т.д.).

#### WorkFlow

1) Запускается приложение и создается поток.
2) Инициализируются две очереди: MicroTask Queue и Event Queue.
3) Далее выполняется main() и сначала немедленно исполняются все синхронные задачи.
4) Если Dart встречает вызовы асинхронных функций (возвращают Future), то помещает их в MicroTask Queue или Event Queue.
5) Если эта очередь MicroTask Queue имеет какие-нибудь задачи, то цикл событий помещает их в основной поток для выполнения.
6) Когда синхронные задачи и задачи из MicroTask Queue завершили выполнение, цикл событий начинает выбирать задачи из очереди Event Queue и помещает их в основной поток, где они выполняются синхронно.
7) Если в очередь MicroTask Queue поступит новая микрозадача, то цикл событий выполняет ее до любой последующей задачи из очереди Event Queue.
8) Этот процесс продолжается до тех пор, пока очереди не станут пустыми.

#### Code example

```dart
void main() {
	print('1 sync');
	Future(() => print('2 event queue')).then(
		value => print('3 sync'),
	);
	Future.microtask(() => print("4 microtask queue"));
	Future.microtask(() => print("5 microtask queue"));
	Future.delayed(
		Duration(seconds: 1), 
		() => print("6 event queue"),
	);
	Future(() => print("7 event queue")).then(
		(value) => Future(
			() => print("8 event queue"),
		),
	);
	Future(() => print("9 event queue")).then(
		(value) => Future.microtask(
			() => print("10 microtask queue"),
		),
	);
	print("11 synchronous");
}
```

Output:
```
1 synchronous
11 synchronous
4 microtask queue
5 microtask queue
2 event queue
3 synchronous
7 event queue
9 event queue
10 microtask queue
8 event queue
6 event queue
```