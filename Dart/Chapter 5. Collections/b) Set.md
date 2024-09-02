
Set - неупорядоченный набор уникальных элементов (без дубликатов).

```dart
enum PlaceType {
	hotel,
	restaurant,
	uniquePlace,
	park,
	museum,
	cafe
}
var filterPlaces = <PlaceType>{
	PlaceType.cafe,
	PlaceType.hotel,
	PlaceType.restaurant,
};

/// Добавить тип в фильтр:
filterPlaces = filterPlaces.union({PlaceType.museum});
filterPlaces = filterPlaces.union({PlaceType.hotel});

/// Удалить тип из фильтра
filterPlaces = filterPlaces.difference({PlaceType.park});

/// Output: Cafe, Hotel, Museum
```
### Методы класса Set

Основные методы множеств:

- `add(E value)`: добавляет элемент в множество

- `addAll(Iterable<E> iterable)`: добавляет в множество другую коллекцию

- `clear()`: удаляет все элементы из множества

- `contains(Object element)`: возвращает true, если элемент содержится в множестве

- `containsAll(Iterable<Object?> other)`: возвращает true, если все элементы коллекции other содержатся в множестве

- `difference(Set<Object> other)`: возвращает разность текущего множества и множества other в виде другого множества

- `intersection(Set<Object> other)`: возвращает пересечение текущего множества и множества other в виде другого множества

- `remove(Object value)`: удаляет объект из множества

- `removeAll(Iterable<Object> elements)`: удаляет из множества все элементы коллекции elements

- `union(Set<E> other)`: возвращает объединение двух множеств - текущего и множества other

- `join([String separator = "" ])`: объединяет все элементы множества в строку. Можно указать необязательный параметр separator, который будет разделять элементы в строке

- `skip(int count)`: возвращает коллекцию, в которой отсутствуют первые count элементов

- `take(int count)`: возвращает коллекцию, которая содержит первые count элементов

- `where(bool test(E element))`: возвращает коллекцию, элементы которой соответствуют некоторому условию, которое передается в виде функции

- `forEach(void action(E element))`: вызывает для каждого элемента множества функцию action

