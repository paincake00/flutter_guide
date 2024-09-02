
Flutter Provider — это библиотека для управления состоянием приложения в Flutter. Она помогает легко передавать данные между виджетами и управлять ими, минуя необходимость использования Stateful-виджетов.
## Context

Прежде надо понимать, что такое `BuildContext`.

![[Pasted image 20240617145131.png]]

При сборке нового виджета рабочая среда Flutter вызывает метод создания элемента. Элемент будет содержать всю необходимую информацию о виджете: сам виджет, дочерний и родительский виджеты, размер и т.д. 
И при обращении к BuildContext context мы получаем экземпляр класса Element данного виджета, и можем уже определить точное его расположение в дереве виджетов (для работы с ним). 

Грубо говоря, виджеты это экземпляры классов, часто вложенные друг друга и составляющие дерево. 
Если надо будет получить данные от работающего в данный момент экземпляра класса (или виджета), то необходимо обратиться к контексту (BuildContext context). 

## Counter App with Provider

Сначала устанавливаем зависимость в `pubspec.yaml` через `flutter pub add provider` или вручную через написание `provider: `. 

Опишем модель, которая будет хранить данные или управлять ими:
```dart
import 'package:flutter/material.dart';

class CounterModel extends ChangeNotifier { // смесь с ChangeNotifier 
  int _counter = 0;
  int get counter => _counter;

  void incrementCounter() {
    _counter++;
    notifyListeners(); // вызывает перестройку для слушателей
  }
}
```

В корне приложения укажем провайдер (в нашем случае, `ChangeNotifierProvider`):
```dart
import 'package:counter_via_provider/models/counter_model.dart';
import 'package:counter_via_provider/pages/home_page.dart';
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';

void main() {
  runApp(
    ChangeNotifierProvider( // сам провайдер
      create: (context) => CounterModel(), // передача экземпляра модели
      child: const MyApp(),
    ),
  );
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});
  
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'Flutter Demo',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
        useMaterial3: true,
      ),
      home: const MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}
```

И главное окно: 
```dart
import 'package:counter_via_provider/models/counter_model.dart';
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';
  
class MyHomePage extends StatelessWidget {
  final String title;
  
  const MyHomePage({super.key, required this.title});
  
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(title),
        centerTitle: true,
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            const Text(
              'You have pushed the button this many times:',
            ),
            Text(
	        // получение значения с перестрокой виджетов (listen:true - default)
              Provider.of<CounterModel>(context).counter.toString(),
              style: Theme.of(context).textTheme.headlineMedium,
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
	    // получение объекта без перестройки виджетов
          final counter = Provider.of<CounterModel>(context, listen: false);
		// вызов метода инкремента  
          counter.incrementCounter();
        },
        tooltip: 'Increment',
        child: const Icon(Icons.add),
      ),
    );
  }
}
```

`Provider.of<T>(Buildcontext context, bool? listen)` <- в данном синтаксисе метод `Provider.of` получает провайдер типа `T` из `context` с условием прослушивания `listen` (true - с перестройкой виджетов, false - без перестройки виджетов). 

Заменить статический метод выше можно на методы контекста:
- `context.watch<T>()`, заставляет виджет меняться, прослушивая `T`
- `context.read<T>()`, возвращает `T` без прослушивания
- `context.select<T, R>(R selector(T value))`, позволяет прослушивать и менять виджеты, выбирая определенные данные `T`. Возвращает тип определенных данных `R`. В качестве примера: `context.select<Person, String>((Person p) => p.name);`
Таким образом, можно изменить код:
```dart
...
Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(title),
        centerTitle: true,
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            const Text(
              'You have pushed the button this many times:',
            ),
            Text(
	        // с прослушиванием => с перестройкой
              context.watch<CounterModel>().counter.toString(),
              style: Theme.of(context).textTheme.headlineMedium,
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
	    // без прослушивания
          final counter = context.read<CounterModel>();
		// вызов метода инкремента  
          counter.incrementCounter();
        },
        tooltip: 'Increment',
        child: const Icon(Icons.add),
      ),
    );
  }
...
```

Также если необходимо использовать несколько провайдеров (или моделей) в проекте, то можно использовать `MultiProvider`:
```dart
MultiProvider(
  providers: [
    Provider<Something>(create: (context) => Something()),
    Provider<SomethingElse>(create: (context) => SomethingElse()),
    Provider<AnotherThing>(create: (context) => AnotherThing()),
  ],
  child: someWidget,
)
```

Также есть `ProxyProvider`, `StreamProvider`, `FutureProvider`, `ValueListenableProvider`.

## Как избежать перерисовок в Flutter с помощью Provider

Перерисовки в Flutter происходят, когда виджет перерисовывается на экране. Это происходит во время изменения состояния приложения или при вызове метода `setState()` Перерисовки могут замедлить работу приложения и потреблять большое количество ресурсов устройства.

Как раз методы выше с параметром `listen:true` потребляли много ресурсов, заставляя каждый перерисовывать все главное окно для обновления значения при нажатии на кнопку.

Для этого есть следующие классы: `Selector`, `Consumer` и `ValueNotifier`.

## Consumer

`Consumer` — это виджет, который позволяет перерисовывать только ту часть экрана, которая изменилась. Он используется для обновления только той части экрана, которая зависит от состояния `Provider`. 

```dart
class Task {
  String title;
  
  Task(this.title);
}
  
class TaskProvider with ChangeNotifier {
  List<Task> _tasks = [];
  
  List<Task> get tasks => _tasks;
  
  void addTask(Task task) {
    _tasks.add(task);
    notifyListeners();
  }
}
  
class TaskScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Column(
        children: [
          Expanded(
          // отрисовка списка тасков
            child: Consumer<TaskProvider>( // вызов класса
              builder: (_, provider, __) => ListView.builder(
                itemCount: provider.tasks.length,
                itemBuilder: (_, index) => ListTile(
                  title: Text(provider.tasks[index].title),
                ),
              ),
            ),
          ),
        ],
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          showDialog(
            context: context,
            builder: (_) => AlertDialog(
              title: Text('Add Task'),
              content: TextField(
                onChanged: (value) {
                // добавление таска
                  Provider.of<TaskProvider>(context, listen: false)
                      .addTask(Task(value));
                },
              ),
            ),
          );
        },
        child: Icon(Icons.add),
      ),
    );
  }
}
```

## Selector

`Selector` — это виджет, который позволяет выбирать только необходимые данные из `Provider`. Он позволяет избежать перерисовки всего экрана при изменении какого-то одного поля в `Provider`.

```dart
class Counter {
	int count = 0;
	
	void increment() {
		count++;
	}
}

class CounterProvider with ChangeNotifier {
	Counter _counter = Counter();
	
	Counter get counter => _counter;
	
	void increment() {
		_counter.increment();
		notifyListeners();
	}
}

class TaskScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Selector<CounterProvider, int>(
	        selector: (_, provider) => provider.counter.count,
	        builder: (_, value, __) => Text(value.toString()),
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
	        Provider.of<CounterProvider>(context, listen: false).increment();
        },
        child: Icon(Icons.add),
      ),
    );
  }
}
```

## ValueNotifier

`ValueNotifier` — это класс, который позволяет уведомлять слушателей только при изменении значения. Это позволяет избежать частых перерисовок, если изменения не влияют на отображение на экране.

```dart
class Theme {
  Color primaryColor;
  Theme(this.primaryColor);
}
  
class ThemeProvider with ChangeNotifier {
  ValueNotifier<Theme> _theme = ValueNotifier(Theme(Colors.blue));
  
  ValueNotifier<Theme> get theme => _theme;
  
  void changeThemeColor(Color color) {
    _theme.value = Theme(color);
  }
}
  
class ThemeScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Consumer<ThemeProvider>(
          builder: (_, provider, __) => Container(
            width: 100,
            height: 100,
            color: provider.theme.value.primaryColor,
          ),
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          Provider.of<ThemeProvider>(context, listen: false)
              .changeThemeColor(Colors.green);
        },
        child: Icon(Icons.color_lens),
      ),
    );
  }
}
```