
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

`any()` and `every()`:

```dart
void main() {
	var list = ['Tom', 'Bob', 'Sam', 'Kate'];

	print(list.every((element)=>element.length > 2)); // true
	print(list.every((element)=>element.length > 3)); // false

	print(list.any((element)=>element.length > 3)); // true
	print(list.any((element)=>element.length > 4)); // false
}
```
