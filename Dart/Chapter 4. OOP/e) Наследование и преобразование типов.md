### *Наследование*

Наследование является одним из ключевых моментов объектно-ориентированного программирования, позволяя передавать одним классам функционал других. В языке Dart наследование реализуется с помощью ключевого слова `extends`.
`class Employee extends Person {`
### *Конструкторы и ключевое слово super*

```run-dart
void main() {
	Employee p = Employee("Tom", 20, "Yandex");
	p.display();
}
class Person {
	String name = "";
	int age = 0;
	Person(this.name, this.age);
	void display() => print("Name: $name \tAge: $age");
}
class Employee extends Person {
	String company = "";
	Employee(String name, int age, this.company) : super(name, age);
	// in new Dart versions >
	// Employee(super.name, super.age, this.company); 
}
```

Для именованных конструкторов все также (+ `@override`):
```run-dart
void main() {
	Employee p = Employee.withName("Tom");
	p.display();
}
class Person {
	String name = "";
	int age = 18;
	Person(this.name, this.age);
	
	Person.withName(this.name);
	void display() => print("Name: $name \tAge: $age");
}
class Employee extends Person {
	String company = "Undefined";
	Employee(String name, int age, this.company) : super(name, age);
	
	Employee.withName(String name) : super.withName(name);
	
	@override
	void display() {
		super.display();
		print("Company: $company");
	}
}
```

### *Преобразование типов. Неявные преобразования*

Объект производного класса одновременно является объектом базового класса. Поэтому Dart автоматически неявно может преобразовать объекты производных типов в базовый тип. Пусть класс `Person` это родитель для класса `Employee`. Тогда неявное преобразование можно достичь, например, так:
`Person p = Employee("SomeName", 22)`
Или пример с аргументами функции:
```dart
void printName(Person person) {
	print(person.name);
}
void main() {
	Person tom = Person("Tom");
	Employee bob = Employee("Bob", "Google");
	Student sam = Student("Sam", "IBM");
	printName(tom);
	printName(bob); // неявное преобразование к Person
	printName(sam); // неявное преобразование к Person
}
```
### *Явные преобразования и оператор as*

Преобразования от производного класса (Employee/Student) к базовому (Person) производятся автоматически. Но что будет противоположном направлении - от базового к производному? Подобные преобразовании нам надо выполнять явно - с помощью оператора as: `объект as тип`
Пример:
```dart
void printCompany(Person pers) {
	Employee employee = pers as Employee;
	print(employee.company);
}
void main() {
	Person bob = Employee("Bob", "Google");
	printCompany(bob); // Google
}
```
Однако с этим связана другая проблема - теоретически в функцию printCompany мы можем передать объект Person, который не является объектом Employee.
### *Проверка типа и оператор is*

Оператор `is` возвращает `true`, если объект представляет определенный тип: `объект is тип`

Также есть противоположный оператор - `is!`, который возвращает `true`, если объект НЕ представляет определенный тип: `объект is! тип`
