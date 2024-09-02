
Определение:
```run-dart
void main() {
	hello();
	printPerson("Tom", 19);
	printPerson("Bob");
}
void hello() => print("Hello");

void printPerson(String name, [int age = 18]){ // default parameter
	print("Name: $name");
	print("Age: $age\n");
}
```
Если необязательный параметр представляет nullable-тип, то есть может принимать значение null, тогда можно не указывать для него значение по умолчанию - в этом случае значение по умолчанию будет null:
```run-dart
void main() {
	printPerson(name: "Tom", age: 35);
	printPerson(name: "Alice");
}
void printPerson({String name = "Undefined", int? age}) {
	print("Name: $name");
	if (age != null) {
		print("Age: $age");
	}
}
```

### *Обязательные параметры*

```run-dart
void main() {
	printPerson(age: 29, name: "Tom");
	printPerson(name: "Kate");
}
void printPerson({required String name, int age = 22}) {
	print("Name: $name");
	print("Age: $age\n");
}
```

### *Возврат функции*

```run-dart
void main() {
	print(sum(2, 5));
	mult(3, 5);
}

int sum(int a, int b) => a + b;

mult(int a, int b) => print(3 * 5);

```

