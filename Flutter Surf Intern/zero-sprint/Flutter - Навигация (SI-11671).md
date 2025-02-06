
### 1. Сравнение императивного и декларативного подходов к навигации

**Императивный подход** - мы указываем "как" надо реализовать.

Пример: Navigator

(+) Контроль и гибкость
(-) Тяжело поддерживать при расширении


**Декларативный подход** - мы указываем "что" надо реализовать.

Пример: Navigator (c именованными маршрутами), пакеты auto_route или go_router

(+) Меньше контроля, но хорошая масштабируемость
(-) Частичная сложность в понимании/реализации


### 2. Базовые методы навигации

В Flutter для навигации используются методы `Navigator.push()` и `Navigator.pop()`:

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: FirstScreen(),
    );
  }
}

class FirstScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('First Screen')),
      body: Center(
        child: ElevatedButton(
          child: Text('Go to Second Screen'),
          onPressed: () {
            Navigator.push(
              context,
              MaterialPageRoute(builder: (context) => SecondScreen()),
            );
          },
        ),
      ),
    );
  }
}

class SecondScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Second Screen')),
      body: Center(
        child: ElevatedButton(
          child: Text('Go Back'),
          onPressed: () {
            Navigator.pop(context);
          },
        ),
      ),
    );
  }
}
```


### 3. Передача данных между экранами с использованием именованных маршрутов

Используем `Navigator.pushNamed`:

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
	  // Настраиваем роуты
      initialRoute: '/',
      routes: {
        '/': (context) => FirstScreen(),
        '/second': (context) => SecondScreen(),
      },
    );
  }
}

class FirstScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('First Screen')),
      body: Center(
        child: ElevatedButton(
          child: Text('Go to Second Screen'),
          onPressed: () {
            Navigator.pushNamed( // переходим по роуту
              context,
              '/second',
              arguments: 'Hello from First Screen',
            );
          },
        ),
      ),
    );
  }
}

class SecondScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
	// Можно получить данные из аргументов
    final String message = ModalRoute.of(context)!.settings.arguments as String;
    return Scaffold(
      appBar: AppBar(title: Text('Second Screen')),
      body: Center(
        child: Text(message),
      ),
    );
  }
}
```

### 4. Navigator.pushReplacement() и Navigator.pushNamedAndRemoveUntil()

- **Navigator.pushReplacement()**: заменяет текущий экран на новый.
- **Navigator.pushNamedAndRemoveUntil()**: добавляет новый экран и удаляет все предыдущие экраны вплоть до указанного.

```dart
...
onPressed: () {
    Navigator.pushReplacementNamed(context, '/second');
},
...
```

```dart
...
onPressed: () {
	// переходим на '/' и удаляем все предыдущие роуты из стека
    Navigator.pushNamedAndRemoveUntil(context, '/', (route)=>false);
},
...
```
или
```dart
...
onPressed: () {
    Navigator.pushNamedAndRemoveUntil(
	    context, 
	    '/newRoute',
	    ModalRoute.withName('/targetRoute'), // удаление до указанного роута
    );
},
...
```

### 5. Почему многие методы навигации возвращают Future

Методы навигации, такие как `Navigator.push()`, возвращают `Future`, потому что они асинхронные. Это позволяет вам ожидать завершения навигации и выполнять действия на основе результата, например, когда пользователь возвращается с другого экрана.

### 6. Передача данных между экранами с использованием методов Navigator

```dart
import 'dart:async';
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: FirstScreen(),
    );
  }
}

class FirstScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('First Screen')),
      body: Center(
        child: ElevatedButton(
          child: Text('Go to Second Screen'),
          onPressed: () async {
	        // получение данных при возврате
            final result = await Navigator.push(
              context,
              MaterialPageRoute(builder: (context) => SecondScreen()),
            );
            print(result); // Обработка результата
          },
        ),
      ),
    );
  }
}

class SecondScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Second Screen')),
      body: Center(
        child: ElevatedButton(
          child: Text('Return with data'),
          onPressed: () {
            Navigator.pop(context, 'Hello from Second Screen!');
          },
        ),
      ),
    );
  }
}
```

### 7. Обработка событий навигации и нажатия кнопки "назад"

Вы можете обрабатывать нажатие кнопки "назад" с помощью виджета `WillPopScope`, который позволяет вам перехватывать событие выхода.

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(home: FirstScreen());
  }
}

class FirstScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('First Screen')),
      body: Center(
        child: ElevatedButton(
          child: Text('Go to Second Screen'),
          onPressed: () {
            Navigator.push(context, MaterialPageRoute(builder: (context) => SecondScreen()));
          },
        ),
      ),
    );
  }
}

class SecondScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return WillPopScope(
      onWillPop: () async {
	    // Каккая-то логика обработки при выходе
  
        // Вернуть true, если хотите разрешить выход с экрана через жест или кнопку "назад"
		// Вернуть false, если хотите заблокировать выход (надо использовать ручной выход)
        return false;
      },
      child: Scaffold(
        appBar: AppBar(title: Text('Second Screen')),
        body: Center(
          child: ElevatedButton(
				onPressed: () {
					Navigator.pop(context); // ручной выход
				},
				child: Text('Close'),
			);,
        ),
      ),
    );
  }
}
```