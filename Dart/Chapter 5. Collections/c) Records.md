
`Records` являются анонимным, неизменяемым типом, который, подобно типам коллекций, позволяет объединять несколько объектов в один объект. В отличие от других типов коллекций, records имеют фиксированный размер, могут хранить значения разных типов и типизированы. В других языках программирования есть аналогичная структура данных - кортежи
```dart
void main() {
	var person = ('Tom', 38);
	print(person); // ('Tom', 38)
}
```

### Обращение к элементам кортежа

По умолчанию для обращения к элементам кортежа используются номера элементов в кортеже, которые предваряются символом $ (нумерация начинается с 1):

```dart
void main() {
	var person = ("Tom", 38);
	print(person.$1);   // Tom
	print(person.$2);   // 38
}
```

Например, выражение `person.$1` представляет обращение к первому элементу кортежа person, а `person.$2` - к второму элементу.

Но также для каждого элемента кортежа можно указать имена:

```dart
void main() {
	var person = (name: 'Tom', age: 38);
	print(person.name);
	print(person.age);
}
```

В этом случае сначала указывается имя и через двоеточие значение элемента. Стоит отметить, что в этом случае тип кортежа будет `({int age, String name})`:

```dart
void main() {
	({int age, String name}) person = (name: 'Tom', age: 38);
	print(person.name);
	print(person.age);
}
```

### Bonus:

Unmodifiable list (look like tuple..):
```dart
final unmodifiableList = List<int>.unmodifiable([1, 2, 3]);

unmodifiableList.add(4); // Throws an UnsupportedError
unmodifiableList[0] = 0; // Throws an UnsupportedError
unmodifiableList.removeAt(0); // Throws an UnsupportedError
unmodifiableList.remove(1); // Throws an UnsupportedError
unmodifiableList.clear(); // Throws an UnsupportedError
```
