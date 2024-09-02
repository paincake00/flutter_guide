
Имя переменной представляет произвольное название, которое может содержать алфавитно-цифровые символы и символ подчеркивания и которое не должно совпадать с одним из ключевых слов языка: `abstract, else, import, super, as, enum, in, switch, assert, export, interface, sync, async, extends, is, this, await, extension, library, throw, break, external, mixin, true, case, factory, new, try, catch, false, null, typedef, class, final, on, var, const, finally, operator, void, continue, for, part, while, covariant, Function, rethrow, with, default, get, return, yield, deferred, hide, set, do, if, show, dynamic, implements, static`

### *Значение переменной*

С помощью операции присваивания переменной можно присвоить значение, которое соответствует ее типу.
```run-dart
void main() {
	String name = "Tom";
	print(name);
	name = "Bob";
	print(name);
}
```

### *var & dynamic*

Для определения переменной также можно использовать ключевое слово **var**:
`var name = "Tom";`
В этом случае Dart сам выводит тип переменной, исходя из присвоенного ей значения.
Также есть ключевое слово `dynamic`:
`dynamic name = "Tom";`
В отличие от var, dynamic позволяет изменять тип переменной повторно.

### *const & final*

```dart
void main() {
	const name1 = "Tom";
	final name2 = "Bob";
	print(name1); // Tom
	print(name2); // Bob
	// name1 = "JoJo"; // Ошибка - значение константы нельзя изменять!
	// name2 = "JoJO"; // Аналогичная ошибка..
}
```
Главное различие между **const** и **final** состоит в том, что значение **const** должно быть определено при _компиляции_, а значение константы **final** определяется во время _выполнения_.
Допустим, мы хотим вывести на консоль текущую дату и время. Для получения текущих даты и времени в Dart мы можем использовать выражение `DateTime.now()`. Например, сохраним дату и время в константу и выведем на консоль:
```dart
void main() {
	final today = DateTime.now();
	print(today);
}
```
Дата и время и определяются во время выполнения программы.
Если же мы попробуем в примере выше вместо `final` использовать `const`, то программа не скомпилируется и не запустится:
```dart
void main() {
	const today = DateTime.now();  // ! Error !
	print(today);
}
```

### *Примитивные типы данных*

- **bool**
```dart
bool yes = true;
bool no = false;
```

- **int**
Тип int представляет целые числа, которые занимают не более 64 бит, точный размер зависит от платформы. Например, для настольных/мобильных приложений значения варьируются от -263 до 263 - 1.
```dart
int x = 8;
var y = -5;
```

- double
Тип double представляет числа с плавающей точкой, которые занимают в памяти 64 бита.
```dart
double x = 8.8;
var y = -5.3;
var z = 0.09;
double x = 9; // x = 9.0
```

- String
Строки представлены типом String, который представляет последовательность символов в кодировке UTF-16. Для определения строки можно использовать одинарные и двойные кавычки:
```dart
String tom = "Tom";
String sam = 'Sam';
var kate = "Kate";
var alice = 'Alice';
String multiline = '''
			This is multiline
			string
			''';
```
С помощью интерполяции мы можем вводить в строку значения других переменных:
```dart
String name = "Tom";
int age = 35;
String info = "Name: $name Age: $age";
```
