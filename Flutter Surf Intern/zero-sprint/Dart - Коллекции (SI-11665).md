
### 1. Знание коллекций: List, Map, Set и их отличия

- **List**: Упорядоченная коллекция элементов, позволяющая дублирование. Индексированные.
- **Map**: Коллекция пар "ключ-значение". Каждый ключ уникален, а значения могут повторяться.
- **Set**: Неупорядоченная коллекция уникальных элементов. Не допускает дублирования.

### 2. Способы создания базовых коллекций

#### **List**:

```dart
List<String> fruits = ['Apple', 'Banana', 'Cherry'];
List<int> numbers = List<int>.from([1, 2, 3]);
```
#### **Map**:

```dart
Map<String, int> fruitMap = {
  'Apple': 1,
  'Banana': 2,
  'Cherry': 3
};
Map<int, String> numberMap = Map.from({1: 'One', 2: 'Two'});
```

#### **Set**:

```dart
Set<String> fruitSet = {'Apple', 'Banana', 'Cherry'};
Set<int> numberSet = Set.from([1, 2, 3]);
```

### 3. Конструкторы коллекций

- **List**: `List.from()`, `List.filled()`, `List.generate()`.
- **Map**: `Map.from()`, `Map.of()`, и `Map.identity()`.
- **Set**: `Set.from()`, `Set.of()`.

#### **Map.identify()**

`Map.identity()` создает новую `Map`, где ключи и значения сравниваются по ссылке, а не по значению. Это означает, что при использовании `Map.identity()` ключи должны быть уникальными экземплярами объектов, чтобы их можно было использовать в качестве ключей.

```dart
void main() {
  var map1 = Map.identity();
  var key1 = Object();
  var key2 = Object();
  
  map1[key1] = 'Value for key1';
  map1[key2] = 'Value for key2';
  
  print(map1[key1]); // Output: Value for key1
  print(map1[key2]); // Output: Value for key2
  
  // При сравнении по значению, ключи key1 и key2 уникальны
  print(map1.containsKey(key1)); // Output: true
  print(map1.containsKey(Object())); // Output: false
}
```

#### **Map.of()**

`Map.of()` создает новую `Map` с заданными парами "ключ-значение", но только если у вас есть уникальные ключи. Это упрощенный способ создания карты, который позволяет вам передавать пары ключей и значений в конструктор.

```dart
void main() {
  var fruitMap = Map.of({
    'Apple': 1,
    'Banana': 2,
    'Cherry': 3,
  });
  
  print(fruitMap); // Output: {Apple: 1, Banana: 2, Cherry: 3}
}
```

#### **Set.of()**

```dart
void main() {
  var numberSet = Set.of([1, 2, 3, 3, 4, 5]); // в отличии от from другой тип данных
  
  print(numberSet); // Output: {1, 2, 3, 4, 5} (дубликаты удаляются)
}
```

### 4. Отличие const коллекции от final

- **const**: Создает неизменяемую коллекцию, которая не может быть изменена после создания. Используя `const`, вы создаете коллекцию на этапе компиляции.
- **final**: Позволяет создать переменную, которая не может быть переназначена, но коллекция сама по себе может быть изменена (например, можно добавлять или удалять элементы).

### 5. Создание неизменяемой коллекции (immutable)

#### **Immutable List**:

```dart
final List<String> immutableList = const ['Apple', 'Banana', 'Cherry'];
```

Еще примеры создания неизменяемых списков:

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

#### **Immutable Map**:

```dart
final Map<String, int> immutableMap = const {
  'Apple': 1,
  'Banana': 2,
  'Cherry': 3
};
```

#### **Immutable Set**:

```dart
final Set<String> immutableSet = const {'Apple', 'Banana', 'Cherry'};
```

### 6. Использование операторов spread ("..."), control flow и циклы внутри коллекций

Spread-оператор **позволяет распаковывать элементы одного массива внутри другого**. С его помощью обычно копируют или сливают массивы

**Пример с оператором spread, control flow, cycles**:

```dart
List<int> additionalNumbers = [4, 5, 6];
List<int> combinedList = [
  ...numbers,
  ...additionalNumbers,
  if (additionalNumbers.isNotEmpty) 7,
  ...[for (int i = 1; i <= 5; i++) i * i],
];
```

### 7. Линейное преобразование одной коллекции в другую

**Пример преобразования из List в List**:

```dart
List<String> names = ['1', '2', '3'];
List<int> numbers = names.map(int.parse).toList();
```

### 8. Фильтрация значений по предикату

**Пример фильтрации четных чисел из List**:

```dart
List<int> evenNumbers = numbers.where((number) => number.isEven).toList();
```

### 9. Проверка существования элемента в коллекции

**Пример проверки существования элемента**:

```dart
bool hasApple = fruitSet.contains('Apple');
```