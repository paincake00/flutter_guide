
Расширения класса позволяют добавить функциональность в уже имеющийся класс. Это может быть полезно, если мы используем класс и хотим добавить в него некоторую функциональность, однако исходный код этого класса нам недоступен. Общий синтаксис расширения класса: 
``extension on имя_класса { // код расширения }``

Например, добавим для встроенного типа `int` метод, который возводит число в определенную степень:
```run-dart
extension on int {
	double pow(int n) {
		double result = 1;
		for (int i = 0; i < n.abs(); i++) {
			result *= this;
		}
		if (n < 0) result = 1 / result;
		return result;
	}
}
void main() {
	int num = 2;
	print(num.pow(2));
	print(num.pow(-2));
}
```

Пример расширения для Context, для удобного обращения к ThemeData и ColorScheme:
```dart
import 'package:flutter/material.dart';

extension BuildContextTheme on BuildContext {
  ThemeData get theme => Theme.of(this);
  ColorScheme get colorScheme => theme.colorScheme;
}
```