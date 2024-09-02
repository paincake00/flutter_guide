
### *Анонимные функции*

```run-dart
void main() {
	Function sum = (a, b) => a + b;
	print(sum(2, 3));
	Function multiply = (a, b) {
		return a * b;
	};
	print(multiply(5, 6));
}
```

### *Вложенные функции*

```run-dart
void main() {
	void showMessage() {
		void hello() {
			print("Hello!");
		}
		hello();
		hello();
	}
	showMessage();
}
```
