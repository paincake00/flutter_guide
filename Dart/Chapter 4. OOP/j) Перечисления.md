
Перечисления или `enum` представляют тип данныx, который хранит фиксированный набор константных з1начений:
```run-dart
enum Operation {
	add,
	subtract,
	multiply,
}
void main() {
	print(Operation.multiply);
	print(Operation.subtract.index);
}
```

Обычно переменная перечисления выступает в качестве хранилища состояния, в зависимости от которого производятся некоторые действия. Так, рассмотрим применение перечисления на более реальном примере:
```run-dart
enum Operation {
	add,
	subtract,
	multiply,
}

void main() {
	executeOperation(4, 5, Operation.add);
	executeOperation(2, 2, Operation.subtract);
	executeOperation(9, 8, Operation.multiply);
}

void executeOperation(int x, int y, Operation op) {
	
	int result = switch(op) {
		Operation.add => x + y,
		Operation.subtract => x - y,
		Operation.multiply => x * y,
	};
	
	print("Результат: $result");
} 
```
