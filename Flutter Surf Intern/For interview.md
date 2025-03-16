
### ООП

ООП - это парадигма программирования, завязанная на классах и объектах, и их взаимодействиях.

Принципы:
- Инкапсуляция (принцип, основывающийся на сокрытии данных класса и на предоставлении доступа к этим классам через публичные методы. Необходим для независимой работы классов)
- Наследование (принцип, позволяющий одному классу наследовать свойства и поведения другого). Есть еще альтернативные подходы для отношений между классами и объектами, которые противопоставляются наследованию: 
	1) Композиция - класс содержит объект другого класса, и последний НЕ может существовать отдельно от первого. Классы: Car и Engine
	2) Агрегация - класс содержит объект другого класса, и последний может существовать отдельно от первого. Классы: University и Student
- Полиморфизм (принцип, позволяющий для разных сущностей выполнять одни и те же действия, независимо от их реализации. Необходим для задания поведения для объектов, с которым можно гибко работать)
- Абстракция (принцип, выделяющий общие свойства и поведение объектов, опуская детали реализации. Необходим для построения общей структуры классов и их отношениями)

### Паттерны проектирования

Они нужны для того, чтобы сделать код более читабельным, поддерживаемым и масштабируемым

Порождающие (завязанные на способах инициализации объектов классов):
- Абстрактная фабрика (общий интерфейс (фабрика) для создания семейства взаимосвязанных объектов, не вдаваясь в конкретную их реализацию)
- Строитель (предоставляет единый код, описывающий правила создания сложных объектов для кастомной сборки)
- Фабричный метод (определяет интерфейс для создания объектов, позволяя его подклассам решать какой класс инстанцировать)
- Prototype (создание копии объекта)
- Singleton (создание объекта, который должен существовать в единственном экземпляре)

Структурные (создание структуры из объектов, задающие более сложные классы и объекты):
- Адаптер (позволяет объектам с несовместимыми интерфейсами работать вместе)
- Фасад (создает удобный интерфейс для работы со сложной системой классов (например, фреймворк))
- Мост (отделяет абстракцию от реализации у классов, позволяя изменять их независимо)

Поведенческие (способы взаимодействия между классами и объектами, их поведение):
- Цепочка обязанностей (Chain of responsibility) (избавляет отправителя запроса от выбора получателя, передавая запрос по цепочке потенциальных обработчиков)
- Strategy (определяет набор алгоритмов, которые инкапсулируют в себе конкретную логику, для выбора поведения объекта (например, кнопки))

### Типы

**`final`/`const`** — модификаторы неизменяемости.
**`var`/`dynamic`** — модификаторы типизации.

Вместе обычно не используются.

### Виджеты

Три дерева:
- `Widget Tree` состоит из `Widget`, которые используются для описания пользовательского интерфейса
- `Element Tree` состоит из `Element`, которые управляют жизненным циклом виджета, содержат информацию об иерархии, и связывают виджеты и объекты рендеринга.
- `Render Tree` состоит из `RenderObject`, которые используются для определения размеров, положения, геометрии, определения зон экрана, на которые могут повлиять жесты.

Widget Lifecycle:

![Flutter Widget Lifecycle|400](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fdmycivygvvok2g4l1vd3.png)

### Algorithms

Вывести определенное количество чисел Фибоначчи: 

```run-dart
void main() {
  const n = 7;

  for (var i = 1; i <= n; i++) {
    print('fibonacci($i) = ${fibonacci(i)}');
  }
}

/// Computes the n Fibonacci number.
int fibonacci(int n) {
  return n < 2 ? n : (fibonacci(n - 1) + fibonacci(n - 2));
}
```

Является ли строка палиндромом:

```run-dart
void main() {
  String str = 'maram';
  print(palindrome(str));
}

bool palindrome(String input) {
  int len = input.length;
  for (var i = 0; i < len ~/ 2; i++) {
    if (input[i] != input[len-i-1]) {
      return false;
    }
  }
  return true;
}
```

Бинарный поиск (O(logn)):

```run-dart
void main() {
  final list = [1, 3, 4, 5, 7, 8, 9];
  
  int value = 1;
  
  print('Index of value: ${binarySearch(value, list, 0, list.length-1)}');
}

int binarySearch(int value, List<int> list, int start, int end) {
  int index = (start + end) ~/ 2;
  print(index);
  if (start > end) return -1;
   
  if (list[index] > value) {
    return binarySearch(value, list, start, index - 1); 
  } else if (list[index] < value) {
    return binarySearch(value, list, index + 1, end);
  } else {
    return index;
  }
}
```

Быстрая сортировка (O(nlogn)):

```run-dart
void main() {
  List<num> unsortedList = [38, 18, 29, 97, 23, 23, 1];

  print(QuickSort<num>().sort(unsortedList));
}

class QuickSort<T extends Comparable<T>> {
  List<T> sort(List<T> list) {
    if (list.length > 1) {
      _quickSort(list, 0, list.length-1);
    }
    
    return list;
  }
  
  void _quickSort(List<T> list, int start, int end) {
    if (start >= end) {
      return;
    }
    
    T mid = list[(start+end) ~/ 2];
    int left = start;
    int right = end;
    while(left <= right) {
      while (list[left].compareTo(mid) < 0) {
        left++;
      }
      while (list[right].compareTo(mid) > 0) {
        right--;
      }
      if (left <= right) {
        T temp = list[left];
        list[left] = list[right];
        list[right] = temp;
        left++;
        right--;
      }
    }
    
    _quickSort(list, start, right);
    _quickSort(list, left, end);
  }
}
```

Реализация связанного списка (поиск - O(n), добавление/удаление элемента (с конца или начала) - O(1)):

```run-dart
void main() {
  final linkedList = LinkedList<int>();

  linkedList.add(4);
  linkedList.add(6);
  linkedList.add(2);
  linkedList.add(4);

  print('Length: ${linkedList.length}');

  print(linkedList.toString());
}

class _Node<T> {
  T _value;

  _Node<T>? _next;

  _Node(this._value, this._next);

  T get value => _value;

  _Node<T>? get next => _next;

  set next(_Node<T>? val) => _next = val;
}

class LinkedList<T> {
  _Node<T>? _head = null;
  _Node<T>? _tail = null;
  
  int _length = 0;
  
  int get length => _length;
  
  void add(T val) {
    _Node<T> newNode = _Node<T>(val, null);
    
    if (length == 0) {
      _head = newNode;
      _tail = newNode;
    } else {
      _tail!.next = newNode;
      _tail = newNode;
    }
    
    _length++;
  }
  
  String toString() {
    final sb = StringBuffer();
    
    sb.write('[');
    
    for (var curr = _head; curr != null; curr = curr.next) {
      sb.write(curr.value.toString());
      if (curr.next != null) {
        sb.write(', ');
      }
    }
    
    sb.write(']');
    
    return sb.toString();
  }
}
```

Пузырьковая сортировка (O(n^2)):

```run-dart
void main() {
  final list = [4, 2, 5, 7, 1, 9, 2];
  
  print(bubbleSort(list).toString());
}

List<int> bubbleSort(List<int> list) {  
  for (var i = list.length - 1; i >= 0; i--) {
    for (var j = 1; j <= i; j++) {
      if (list[j-1] > list[j]) {
        int temp = list[j-1];
        list[j-1] = list[j];
        list[j] = temp;
      }
    }
  }
  
  return list;
}
```