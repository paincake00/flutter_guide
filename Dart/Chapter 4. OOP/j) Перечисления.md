
Перечисления или `enum` представляют тип данныx, который хранит фиксированный набор константных значений:
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

Перечисления можно создать с некоторыми методами и свойствами:
```run-dart
enum Animal {
  dog('woof'),
  cat('meow'),
  bird('tweet');

  final String sound;

  const Animal(this.sound);

  // Метод для получения ключа (в данном случае звука)
  String get key => sound;
}

void main() {
  Animal animal = Animal.dog;
  print('The sound of ${animal.name} is ${animal.key}'); // Output: The sound of dog is woof
}
```