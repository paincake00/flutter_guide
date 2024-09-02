
Generics или обобщения позволяют добавить программе гибкости и уйти от жесткой привязки к определенным типам.
Пример использования:
```run-dart
class Person<T> {
	T id;
	String name;
	Person(this.id, this.name);
}
void main() {
	Person p1 = Person("first", "George");
	print(p1.id.runtimeType); // String
	Person<int> p2 = Person<int>(2, "Tom");
	print(p2.id.runtimeType); // int 
}
```
Причем название параметра может быть произвольным, но обычно используются заглавные буквы, часто буква T. 
С помощью поля `runtimeType` мы можем получить конкретный тип данных переменной.

Мы можем определять generic-методы и функции:
```run-dart
void main() {
	log(20);
	log(34);
	log("Tom");
}
void log<T>(T a) {
	print("${DateTime.now()} val = $a");
} 
```
### *Ограничение обобщений*

Иногда необходимо использовать обобщения, однако принимать любой тип в функцию или класс вместо параметра T нежелательно. Например, есть класс Account, представляющий банковский счет:
```dart
class Account {
	int id;
	int sum;
	Account(this.id, this.sum);
}
```
Но у класса Account может быть много наследников: DepositAccount (депозитный счет), DemandAccount (счет до востребования) и т.д.
Тогда проблему можно решить так:
```dart
class Transaction<T extends Account> {
	T fromAccount;
	T toAccount;
	int sum;
	Transaction(this.fromAccount, this.toAccount, this.sum);
	void execute() {
		if (fromAccount.sum > sum) {
			fromAccount.sum -= sum;
			toAccount.sum += sum;
			print("Перевод выполнен");
		}
		else {
			print("Недостаточно средств на счёте ${fromAccount.id}");
		}
	}
}
```
С помощью выражения `<T extends Account>` указываем, что используемый тип T обязательно должен быть классом Account или его наследником.
### *Наследование обобщенных классов*

Как и обычные классы, обобщенные классы могут наследоваться. Если мы хотим сохранить обобщенность унаследованного функционала, то производный класс также определяется обобщенным:
```run-dart
void main() {
	Employee e = Employee(123, "Tom", "Ozon");
	e.display();
}
class Person<T> {
	T id;
	String name;
	Person(this.id, this.name);
	void display() => print("id: $id\nname: $name");
}
class Employee<T> extends Person<T> {
	String company;
	Employee(super.id, super.name, this.company);
	@override
	void display() {
		super.display();
		print("works in $company");
	}
}
```
