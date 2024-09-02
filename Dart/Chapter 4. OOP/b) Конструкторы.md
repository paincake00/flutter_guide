
Для создания объекта класса и инициализации состояния объекта применяются специальные методы, которые называются конструкторами.
```dart
class Person {
	String name = "";
	int age = 0;

	Person(String name, int age) {
		this.name = name;
		this.age = age;
	}
}
```
Но также можно сократить запись конструктора через запись:
`Person(this.name, this.age);`
И тогда можно не указывать значения для полей по умолчанию.

### *Именованные конструкторы*

По умолчанию мы можем определить только один общий конструктор. Если же нам необходимо использовать в классе сразу несколько конструкторов, то в этом случае нужно применять именованные конструкторы (named constructors). Для этого к имени класса через точку добавляется дополнительный идентификатор (произвольное имя):
```run-dart
class Person {
	String name;
	int age;

	Person.undefined(): this("undefined", 18);

	Person.withName(String name): this(name, 18); // call the constructor Person()

	Person(this.name, this.age);  // сокращенный конструктор

	void display() {
		print("Name: $name \tAge: $age");
	}
}

void main() {
	Person p1 = Person.undefined();
	p1.display();
	
	Person p2 = Person.withName("Tom");
	p2.display();

	Person p3 = Person("Geogre", 20);
	p3.display();
}
```

### *Обязательные и необязательные параметры*

```run-dart
class Person {
	String name;
	int age;

	Person({required this.name, this.age=18});

	void display() {
		print("Name: $name \tAge: $age"); 
	}
}

void main() {
	Person p1 = Person(name: "Tom");
	p1.display();
}
```

### *Инициализаторы*

Инициализаторы представляют способ инициализации полей класса:
```run-dart
class Person {
	String name = "";
	int age = 0;

	Person(String name, int age): this.name = name, this.age = age {
		print("Pesron $name was created");
	}
	void display() => print("Name $name \tAge: $age");
}
void main() {
	Person p = Person("Tom", 18);
	p.display();
}
```
Список инициализации указывает после параметров конструктора через двоеточие до открывающей фигурной скобки. Обычно списки инициализации используют параметры конструктора для установки значений полей. При этом конструктор может выполнять какую-то другую работу.

### *Константный конструктор*

Классы могут содержать константные конструкторы. Такие конструкторы призваны создавать объекты, которые не должны изменяться. Константные конструкторы предваряются ключевым словом `const`:
```run-dart
class Person {
	final String name;
	final int age;
	
	const Person(this.name, this.age);
	void display() => print("Name: $name \tAge: $age");
}
void main() {
	const Person p = Person("Tom", 20);
	p.display();
}
```

### *Фабричные конструкторы*

По умолчанию конструкторы класса создают и возвращают новый объект этого класса. Но, возможно, мы захотим использовать и возвращать из конструктора уже имеющийся объект. Для этого применяются фабричные конструкторы, которые предваряются ключевым словом factory.
```run-dart
class Person {
	String name;
	int age;
	
	factory Person(String name, int age) {
		if (name.length < 2) {
			name = "Undefined";
		} 
		if (age < 1 || age > 111) {
			age = 18;
		}
		return Person._create(name, age);
	}
	
	Person._create(this.name, this.age);
	
	void display() => print("Name: $name \tAge: $age");
}
void main() {
	Person p1 = Person("Tom", -1);
	p1.display();
}
```
По сути, это инициализация объекта внутри класса с применением дополнительных операций над входными данными.
Особенно полезен фабричный конструктор при реализации singletons - объектов, которые должен существовать в едином виде (например, объект подключения к БД).
