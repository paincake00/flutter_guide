
There are two implementation of lists (Есть две реализации списков).

### Изменяемые списки

- List - indexable collection of objects with a specific length (Список - индексируемая коллекция объектов с специфической длиной).
```dart
void main() {
	final list = <String>['1', '2', '3'];

	list.addAll(['4', '5']); // 1 2 3 4 5

	list.insertAll(2, ['99', '100']); // 1 2 99 100 3 4 5

	list.removeAt(4); // 1 2 99 100 4 5

	list.setAll(1, ['11', '12']); // 1 11 12 100 4 5
}
```

#### Свойства списков:

```run-dart
void main() {
	var list = ['Tom', 'Bob', 'Sam'];
	print(list.first); // Tom
	print(list.last); // Sam
	print(list.isEmpty); // false
	print(list.isNotEmpty); // true
	print(list.length); // 3

	var reversed = list.reversed;
	print(reversed); // Sam, Bob, Tom
}
```

#### Методы списков:

- `add(E value)`: добавляет элемент в конец списка

- `addAll(Iterable<E> iterable)`: добавляет в конец списка другой список

- `clear()`: удаляет все элементы из списка

- `indexOf(E element)`: возвращает первый индекс элемента

- `insert(int index, E element)`: вставляет элемент на определенную позицию

- `remove(Object value)`: удаляет объект из списка (удаляется только первое вхождение элемента в список)

- `removeAt(int index)`: удаляет элементы по индексу

- `removeLast()`: удаляет последний элемент списка

- `removeRange(int start, int end)`: удаляет диапазон элементов между индексами start и end

- `removeWhere(bool test(E element))`: удаляет элементы, которые удовлетворяют условию, передаваемому в виде функции test

- `forEach(void f(E element))`: производит над элементами списка некоторое действие, которое задается функцией-параметром, аналоги цикла for..in

- `sort()`: сортирует список

- `sublist(int start, [ int end ])`: возвращает часть списка от индекса start до индекса end

- `contains(Object element)`: возвращает true, если элемент содержится в списке

- `join([String separator = "" ])`: объединяет все элементы списка в строку. Можно указать необязательный параметр separator, который будет разделять элементы в строке

- `skip(int count)`: возвращает коллекцию, в которой отсутствуют первые count элементов

- `take(int count)`: возвращает коллекцию, которая содержит первые count элементов

- `where(bool test(E element))`: возвращает коллекцию, элементы которой соответствуют некоторому условию, которое передается в виде функции

### Неизменяемые списки

fixed-length list (Список фиксированной длины):
```dart
final fixedLengthList = List<int>.filled(5, 0); // [0, 0, 0, 0, 0]

fixedLengthList.length = 0; /// Throws an error...
fixedLengthList.add(1); /// Throws an error...
fixedLengthList[0] = 1; /// Does not throw an error...
fixedLengthList[1] = 2; /// Does not throw an error...
```

Еще один способ создания фиксированного списка представляет конструктор `List.generate()`:
```run-dart
void main() {
	var list = List.generate(4, (int index) => index + 1, growable: false);

	list[0] = 20;

	print(list); // [20, 2, 3, 4]
}
```

Coping of list and some operations:
```dart
final integers = List.generate(100, (index) => index);

final list = List<int>.of(integers);

/// Посчитать квадраты чисел в списке
final list = integers.map((e) => e * e).toList();

/// Отфильтровать числа, которые делятся на 3
final list = integers.where((e) => e % 3 == 0).toList();
/// Посчитать сумму всех
final list = integers.fold(0, (previousValue, element) => previousValue + element);
```
#### spread-оператор `...`

spread-оператор `...` позволяет разложить список на отдельные элементы. Это может быть удобно, если мы хотим наполнить один список элементами из другого списка. Например:

```dart
void main() {
	var employees = ['Tom', 'Bob', 'Kate'];
	var people = ['Alice', ...employees, 'Sam'];

	print(people); // ['Alice', 'Tom', 'Bob', 'Kate', 'Sam']
}
```

#### LINKED LIST

- LinkedList - specialized list with elements with two references each (inherit of LinkedListEntry).
```dart
final list = LinkedList<LinkedListElement>();

list.add(LinkedListElement(1)); // 1
list.add(LinkedListElement(2)); // 1 -> 2
list.elementAt(1).insertBefore(LinkedListElement(3)); // 1 -> 3 -> 2
list.elementAt(2).insertAfter(LinkedListElement(4)); // 1 -> 3 -> 2 -> 4
```