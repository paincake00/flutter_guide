
```run-dart
class Person {
	late String name; // отложенная инициализация
	late int age;
	
	Person(String name, int age) {
		if(name != "admin") this.name = name;
		else this.name = "Undefined";
		
		if(age > 0 && age < 111) this.age = age;
		else this.age = 18;
	}
	void display() => print("Nmame: $name \tAge: $age");
}

void main() {
	Person p = Person("Tom", 20);
	p.display();
}
```
Ключевое слово `late` указывает, что переменная будет инициализирована впоследствии, то есть это так называемая отложенная инициализация. Но если попытаться получить значение до инициализации, то произойдет ошибка.
### *late-константы*

Слово late также можно комбинировать с final для определения констант, которые инициализируются в отложенном режиме. Например:
```run-dart
class Person {
	final String name;
	late final int age;
	
	Person(this.name);
	void setAge(int age) {
		if(age>0 && age<111) this.age = age;
		else this.age = 18;
	}
	void display() => print("Name: $name \tAge: $age");
}
void main() {
	Person p = Person("Joseph");
	p.setAge(20);
	p.display();
}
```

### *Оператор ?*

```dart
Person? person;
person.age = 18; // ошибка так как объект не инициализирован
```
Оператор `?` (null-aware access operator) проверяет, чтобы переменная с объектом не была пустая (null):
```dart
Person? p;
p?.age = 18; // не выполнится, т.к. p == null
```

### *Оператор ?.*

Особую форму этого оператора представляет оператор `?..` (null-aware cascade operator), который позволяет использовать каскадную нотацию, если переменная не равна null:
```dart
void setDefault(Person? person) {
	person
		?..name = "Tom"
		..age = 18
		..display();
}
```

### *Оператор !*

Если мы точно уверены, что эта переменная в процессе работы программы не получит значение `null`, то в этом случае мы можем принимать оператор ! (null assertion operator), который ставится после названия объекта.
```dart
String get(String? text) {
	if(text == null) {
		text = "qwerty";
	}
	return text!;
}
```

### *Оператор !.*

```dart
bool isValid(String? text) {
	if(text == null) {
		text = "";
	}
	return text!.length > 6;
}
```