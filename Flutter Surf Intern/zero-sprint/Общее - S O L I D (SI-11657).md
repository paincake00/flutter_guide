
Source: https://web-creator.ru/articles/solid

SOLID - пять принципов проектирования и написания кода в ООП.

- S - Single responsibility (принцип единственной ответственности)
- O - Open-closed (принцип открытости/закрытости)
- L - Liskov Substitution (принцип Барбары Лисков)
- I - Interface segregation (принцип разделения интерфейса)
- D - Dependency inversion (принцип инверсии зависимости)

### Single Responsibility

Каждый объект должен быть ответственен за выполнение определенной задачи, и код для выполнения данной задачи должен быть полностью инкапсулирован в класс экземпляра.
(ссылается на принцип инкапсуляции)

```dart
class Report {
  String content;

  Report(this.content);
}

class ReportPrinter {
  void print(Report report) {
    print(report.content);
  }
}

void main() {
  var report = Report("Отчет о продажах");
  var printer = ReportPrinter();
  printer.print(report);
}
```

### Open-closed

Класс (модуль, функция и т.д.) должен быть открыт для расширения, но закрыт для изменения. Классы могут менять свое поведение без изменения их исходного кода.
(ссылается на принцип наследования)

```dart
abstract class Shape {
  double area();
}

class Circle implements Shape {
  double radius;

  Circle(this.radius);

  @override
  double area() => 3.14 * radius * radius;
}

class Rectangle implements Shape {
  double width, height;

  Rectangle(this.width, this.height);

  @override
  double area() => width * height;
}

void main() {
  List<Shape> shapes = [Circle(5), Rectangle(4, 6)];
  shapes.forEach((shape) => print(shape.area()));
}
```

### Liskov Substitution 

Функции, которые используют базовый тип, должны уметь использовать подтипы базового типа, не зная о них.
(ссылается на принцип полиморфизма и наследования)

```dart
class Bird {
  void fly() {
    print("Я летаю");
  }
}

class Sparrow extends Bird {}
class Ostrich extends Bird {
  @override
  void fly() {
    throw Exception("Страус не может летать!");
  }
}

void main() {
  Bird sparrow = Sparrow();
  sparrow.fly(); // Работает

  Bird ostrich = Ostrich();
  // ostrich.fly(); // Ошибка, страус не может летать
}
```

### Interface Segregation

Интерфейсы не должны быть перегружены большим числом методов (функционала, который не нужен тем, кто их использует). Их необходимо разделять на более специфические, под определенную задачу.
(ссылается на принцип полиморфизма)

```dart
abstract class Printer {
  void print();
}

abstract class Scanner {
  void scan();
}

class MultiFunctionDevice implements Printer, Scanner {
  @override
  void print() {
    print("Печать документа");
  }

  @override
  void scan() {
    print("Сканирование документа");
  }
}

void main() {
  var mfd = MultiFunctionDevice();
  mfd.print();
  mfd.scan();
}
```

### Dependency Inversion

Абстракция не должна зависеть от реализации, а вот реализация должна зависеть от абстракции. То есть в модули верхних уровней не должны встраиваться зависимости, необходимые для модулей нижних уровней.
(ссылается на принцип абстракции)

```dart
abstract class Database {
  void save(String data);
}

class MySQLDatabase implements Database {
  @override
  void save(String data) {
    print("Сохранение в MySQL: $data");
  }
}

class UserService {
  Database database;

  UserService(this.database);

  void saveUser(String user) {
    database.save(user);
  }
}

void main() {
  var db = MySQLDatabase();
  var userService = UserService(db);
  userService.saveUser("Пользователь 1");
}
```