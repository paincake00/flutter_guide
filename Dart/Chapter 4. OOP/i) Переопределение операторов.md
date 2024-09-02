
Переопределение операторов заключается в определении в классе, для объектов которого мы хотим определить оператор, специального метода:
`возвращаемый_тип operator оператор(параметр) {  }`

Пример, в котором перегружаем операторы `+`, `<`, `>`:
```run-dart
void main() {
	Counter c1 = Counter(5);
	Counter c2 = Counter(6);
	
	Counter c3 = c1 + c2;
	print(c3.value);
	
	print("c3 > c1 - ${c3 > c1}");
	print("c1 < c2 - ${c1 < c2}");
}
class Counter {
	int value;
	Counter(this.value);
	
	Counter operator +(Counter other) => Counter(this.value + other.value);
	
	bool operator <(Counter other) => this.value < other.value;
	
	bool operator >(Counter other) => this.value > other.value; 
}
```

Следует отметить, что мы не можем переопределить абсолютно все имеющиеся в Dart операторы, а только некоторые из них, а именно: `< + | [] > / ^ []= <= ~/ & ~ >= * << == – % >>`
