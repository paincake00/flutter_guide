
### *Создание объекта. Конструктор*

```run-dart
class Person {
	String? name;
	int? age;
	void display() {
		print("Name: $name \t age: $age");
	}
}

void main() {
	Person pers = Person();
	pers.display();

	pers.name = "Tom";
	pers.age = 20;
	pers.display();
}
```

### *Каскадная нотация*

Каскадная нотация - операция `..` позволяет выполнять последовательность операций на объектом:
```run-dart
class Person {
	String name = "";
	int age = 0;
	void display() {
		print("Name: $name \tAge: $age");
	}
}

void main() {
	Person pers = Person()
		..name = "Tom"
		..age = 20
		..display();
}
```
