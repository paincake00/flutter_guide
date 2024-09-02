
- Map - collection of key-value pairs.
```run-dart
void main() {
	var map = <int, String>{
		1: "Tom",
		2: "Bob",
		3: "Sam",
	};
	print(map);
}
```

Свойства map:

```run-dart
void main() {
	var map = { 1: "Tom", 2: "Bob", 3: "Sam"};

    print("Length: ${map.length}"); // Length: 3

    print("Empty: ${map.isEmpty}"); // Empty: false

	print(map.entries);     
	// (MapEntry(1: Tom), MapEntry(2: Bob), MapEntry(3: Sam))

    print(map.keys);        // (1, 2, 3)

    print(map.values);      // (Tom, Bob, Sam)
}
```

Методы map:

- `addAll(Map<K, V> other)`: добавляет в Map другой объект Map
 
- `addEntries(Iterable<MapEntry<K, V>> newEntries)`: добавляет в Map коллекцию Iterable<MapEntry<K, V>>

- `clear()`: удаляет все элементы из Map

- `containsKey(Object key)`: возвращает true, если Map содержит ключ key

- `containsValue(Object value)`: возвращает true, если Map содержит значение value

- `forEach()`: перебирает все элементы и для каждого из них вызывает определенную функцию

- `V putIfAbsent(K key, V ifAbsent())`: если ключ key есть в Map, то возвращает значение по этому ключу. Если ключ отсутствует, то добавляет в Map значение, которое возвращается функцией isAbsent.

- `remove(Object key)`: удаляет из Map элемент с ключом key