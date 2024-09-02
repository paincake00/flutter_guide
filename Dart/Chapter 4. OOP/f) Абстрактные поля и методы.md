```run-dart
abstract class Shape {
	void calculateArea(); // abstract method
}
class Rectangle extends Shape {
	int width;
	int height;
	Rectangle(this.width, this.height);
	
	@override
	void calculateArea() {
		int area = width * height;
		print("area = $area");	
	}
}
void main() {
	Shape rect = Rectangle(20, 30);
	rect.calculateArea();
}
```
Абстрактные классы похожи на обычные классы (также могут определять поля, методы, конструкторы) за тем исключением, что мы не можем создать напрямую объект абстрактного класса, используя его конструктор. Как правило, абстрактные классы объявляют некоторый общий функционал, который по своему реализуют классы-наследники.
### *Интерфейс*

Интерфейс представляет синтаксический контракт, которому должны следовать реализующие этот интерфейс классы (необходимо реализовать через `@override` все поля и методы интерфейса). То есть, если класс-интерфейс определяет какие-нибудь поля и методы, то класс, реализующий данный интерфейс, должен также определить эти поля и методы.
```run-dart
interface class Worker {
	String company = "Microsoft";
	Worker(this.company);
	void work() => print("company = $company");
}
interface class Student {
	String university = "Stanford";
	Student(this.university);
	void study() => print("university = $university");
}
class WorkingStudent implements Worker, Student {
	String name;
	@override
	String company;
	@override
	String university;
	WorkingStudent(this.name, this.company, this.university);
	@override
	void study() => print("$name studies in $university");
	@override
	void work() => print("$name works in $company");
}
void main() {
	WorkingStudent p = WorkingStudent("John", "Yandex", "VSU");
	p.study();
	p.work();
}
```

### *Абстрактные интерфейсы*
Иногда интерфейс нужен просто, чтобы определить некоторый функционал без реализации и обозначить, что данный тип имеет определенный набор методов. В этом случае мы можем определить интерфейсы как абстрактные:
```run-dart
abstract interface class Person {
	late String name;
	void say();
}
class Worker {
	@override
	String name;
	@override
	void say() => print("Worker says: GOYDAA!");
	Worker(this.name);
}
void main() {
	Worker w = Worker("Tom");
	w.say();
}
```
### *Наследование класса VS реализация интерфейса (разница между использованием интерфейса и абстрактного класса)*

При наследовании дочерний класс не обязан определять те же поля и методы, которые есть в родительском классе (за исключением абстрактных методов). А при реализации интерфейса дочерний класс должен определять все поля и методы родительского класса (интерфейса). 

