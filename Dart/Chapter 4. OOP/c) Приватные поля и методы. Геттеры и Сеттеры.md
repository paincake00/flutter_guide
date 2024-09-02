
Чтобы сделать поля приватными, необходимо добавить перед их именем символ подчеркивания (_). И тут надо учитывать, что приватность применяется, если класс определен в отдельной библиотеке/файле.
```dart
class Person {
	String _name = "";
	int _age = 0;

	Person(String name, int age) {
		this._name = name;
		this._age = age;
	}
	void display() => print("Name: $_name \tAge: $_age");
}
```
Стоит отметить, что то же самое обстоятельно касается определения приватных методов - если их имя начинается с подчеркивания, то они тоже являются приватными, и их нельзя вызвать вне класса:
```dart
class Person {
	String _name = "";
	int _age = 0;

	Person(String name, int age) {
		this._name = name;
		this._age = age;
	}
	void _printPerson() => print("Name: $_name \tAge: $_age");
	void display() => _printPerson();
}
```
В данном случае метод _printPerson является приватным, и вне текущего файла к нему обратиться нельзя.

### *Приватные конструкторы*

Также могут быть приватными именованные конструкторы. Например:
```dart
class Person {
	String _name;
	int _age;

	Person(String name, int age): this._create(name, age);

	Person._create(this._name, this._age);
	void display() => print("Name: $_name \tAge: $_age");
}
```
Здесь конструктор `Person._create()` определен как приватный - для этого перед именем конструктора также ставится символ подчеркивания. Этот конструктор устанавливает значения приватных переменных:
`Person._create(this._name, this._age);`
Внутри класса мы можем вызвать этот конструктор
`Person(String name, int age): this._create(name, age);`
Вне файла данного класса приватный конструктор не доступен.

### *Геттеры*

Геттеры (getters) предназначены для возврата значения. Геттер представляет специальный метод, который использует ключевое слово get перед названием свойства и возвращает некоторое значение:
```run-dart
class Person{
	String _name;
	int _age;

	String get name {return _name;}
	int get age => _age;

	Person(this._name, this._age);
}

void main() {
	Person p = Person("Tom", 20);
	print(p.name); // the first getter 
	print(p.age); // the second getter
}
```

### *Сеттеры*

Сеттеры позволяют установить значение поля. Сеттер представляет специальный метод, который начинается с ключевого слова set, за которым следует название свойства. Затем идет один параметр, который представляет устанавливаемое значение:
```run-dart
class Person {
	String _name;
	int _age;

	Person(this._name, this._age);
	
	String get name => _name;
	int get age => _age;
	
	set age(int value) {
		if (value > 0 && value < 111) {
			_age = value;
		}
	}
}

void main() {
	Person p = Person("Tom", 10);
	
	p.age = 25;
	print(p.age);
	
	p.age = 999;
	print(p.age);
}
```

### *Вычисляемые свойства*

Свойства могут быть вычисляемыми - они не представляют значения какого-то конкретного поля, а динамически вычисляются. Например:
```run-dart
class Person{
	String _name;
	int _age;

	String get name {return _name;}
	int get age => _age;

	bool get isChild => _age < 18;

	Person(this._name, this._age);
}

void main() {
	Person p = Person("Tom", 20);
	print(p.name); 
	print(p.age);
	print(p.isChild);
}
```
