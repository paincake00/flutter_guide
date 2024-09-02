Любая функция в языке Dart представляет тип Function и фактически может выступать в качестве отдельного объекта. Например, мы можем определить объект функции, присвоить ему динамически ссылку на какую-нибудь функцию и вызвать ее:
```run-dart
void main() {
	Function func = hello;
	func(); // Hello
	func = bye;
	func(); // Goodbye
}
void hello() => print("Hello");
void bye() => print("Goodbye");
```
Ещё пример:
```run-dart
void main() {
	operation(2, 5, sum);
	operation(10, 9, subtract);
	operation(3, 4, multiply);
}
void operation(int a, int b, Function func) {
	int result = func(a, b);
	print(result);
}
int sum(int a, int b) => a + b;
int subtract(int a, int b) => a - b;
int multiply(int a, int b) => a * b;
```

### *Функция как результат другой функции*

```run-dart
void main() {
	Function message = defineTime(11);
	message();
	message = defineTime(15);
	message();
}
Function defineTime(int hour) {
	if (hour < 12) return morning;
	else return evening;
}
void morning() => print("Good morning");
void evening() => print("Good evening");
```
