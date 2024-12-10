Благодаря интерфейсам мы можем уйти от проблемы множественного наследования (это приводит к diamond problem, когда класс наследует методы с одинаковыми именами от нескольких суперклассов) и реализовать в одному классе функционал сразу нескольких классов:
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
Но видно, что класс WorkingStudent ничего нового не определяет, просто повторяет код полей и методов. Mixins помогают решить эту проблему. Mixins - это способ определения кода, который может быть повторно использован в нескольких иерархиях классов. Они предназначены для массового предоставления реализации членов. 
Mixin не может: содержать конструктор, применять другие mixins, наследоваться от классов.

Если мы хотим, чтобы mixin мог также использоваться как обычный класс, то он определяется с помощью слов `mixin class`. Но если мы хотим создать чистый mixin, то для определения используется просто `mixin`.
```dart
mixin class A{}      // класс-миксин
mixin B{}            // чистый миксин
class C with A, B {} // применение миксинов
```
Перепишем предыдущий пример с использованием mixins:
```run-dart
mixin class Worker {
	String company = "Microsoft";
	void work() => print("company = $company");
}
mixin Student {
	String university = "Stanford";
	void study() => print("university = $university");
}
class WorkingStudent with Worker, Student {
	String name;
	WorkingStudent(this.name, String company, String university) {
		super.company = company;
		super.university = university;
	}
}
void main() {
	WorkingStudent p = WorkingStudent("John", "Yandex", "VSU");
	p.work();
	p.study();
}
```

Так как Worker - класс-mixin, мы можем использовать его независимо - создавать его объекты, изменять его поля, вызывать его методы и т.д., например:
```run-dart
mixin class Worker {
	String company = "Microsoft";
	void work() => print("company = $company");
}
void main() {
	Worker w = Worker();
	w.work();
	w.company = "Apple";
	w.work();
}
```

Дополнительные возможности:
- если нас не устраивает полученная функциональность классов-mixins, то мы можем её переопределить точно так же с помощью `@override`.
- мы можем комбинировать применение mixins с наследованием.
