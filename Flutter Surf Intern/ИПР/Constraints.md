Source: https://docs.flutter.dev/ui/layout/constraints

Constraints - это ограничения, которые накладываются на размер и позицию виджета в макете.

- Виджеты получают constraints от родителей (`minWidth`, `minHeight`, `maxWidth` and `maxHeight`).
- Затем они уже передают constraints уже своим детям, и спрашивают их какого размера они хотят быть.
- После позиционирует их (горизонтально - по оси X, вертикально - по оси Y).
- Наконец, виджет говорит родителям уже собственный размер (с учетом ограничений).

Во Flutter, виджеты отображаются их базовыми объектами RenderBox. 
Есть три типа боксов (с точки зрения того, как они обрабатывают ограничения):

- Те, что пытаются принимать наибольший возможный размер (используются Center, ListView)
- Те, что пытаются принимать размер как и у их детей (Transform, Opacity)
- Те, что пытаются принимать определенный размер (Image, Text)

Некоторые виджеты, такие как Container, могут менять тип бокса. Например, Container стремится быть наибольшего размера, но при указании параметров width/height, пытается соответствовать заданному размеру.

## Примеры

#### exp 1

![[Pasted image 20241220111951.png]]
```dart
Container(width: 100, height: 100, color: Colors.red)
```

Здесь Container хочет принимать заданные размеры, но экран принуждает его быть таким же как размеры экрана.

#### exp 2

![[Pasted image 20241220112308.png]]
```dart
Center(
	child: Container(width: 100, height: 100, color: Colors.red),
)
```

Экран принуждает Center иметь размеры как у экрана. Center говорит, что Container может иметь любой размер, но не больше экрана. Container может принимать заданные через width/height размеры.

#### exp 3

![[Pasted image 20241220114015.png]]
```dart
Center( 
	child: Container( 
		padding: const EdgeInsets.all(20), 
		color: red, 
		child: Container(color: green, width: 30, height: 30), 
	),
)
```

Красный container содержит зеленый container, который хочет быть с width 30 и height 30. Поэтому верхний контейнер принимает размер, чтобы все удовлетворяло padding. 

#### exp 4

![[Pasted image 20241220115805.png]]
```dart
Center(
	child: ConstrainedBox(
		constraints: const BoxConstraints(
			minWidth: 70, 
			minHeight: 70, 
			maxWidth: 150, 
			maxHeight: 150,
		),
		child: Container(color: red, width: 10, height: 10),
	),
)
```

Center позволяет ConstrainedBox принимать любой размер. ConstrainedBox накладывает дополнительные ограничения (через constraints) на Container, который хотел иметь размер 10Х10. В итоге Container имеет минимально возможный размер - 70.

#### exp 5

Есть также UnconstrainedBox, который позволяет дочернему виджету быть любого размера (но это чревато ошибками):
```dart
UnconstrainedBox(
	child: Container(color: red, width: 4000, height: 50),
)
```
Дочерний виджет будет вылезать за экран и показывать предупреждения.

Похожий виджет OverflowBox позволяет делать тоже самое, но показывать допустимый размер дочернего виджета, чтобы не появлялось предупреждений:
```dart
OverflowBox(
	minWidth: 0,
	minHeight: 0,
	maxWidth: double.infinity, 
	maxHeight: double.infinity, 
	child: Container(color: red, width: 4000, height: 50),
)
```

Также есть еще один виджет LimitedBox, который предотвращает появление ошибок с бесконечной шириной дочерних элементов: 
```dart
UnconstrainedBox(
	child: LimitedBox(
		maxWidth: 100,
		child: Container(
			color: Colors.red, 
			width: double.infinity, 
			height: 100,
		),
	),
)
```

#### Другие виджеты, отвечающие за layout

FittedBox - виджет, который масштабирует и позиционирует дочерний виджет в соответствии с fit.

LayoutBuilder - виджет, предоставляющий дочерним виджетам constraints родителей. 

Expanded - виджет, расширяющий дочерний для заполнения доступного пространства (Row, Column или Flex).

Flexible - виджет, позволяет своему дочернему элементу иметь такую же или меньшую ширину, чем сам Flexible, в то время как Expanded заставляет его дочерний иметь точно такую же ширину, как и Expanded.

## Tight vs loose constraints

Есть два типа constraints: Tight (=) and Loose (>).
Tight:
```dart
return const SizedBox(
	width: 300,
	height: 300,
	child: ColoredBox(
		color: Colors.red,
	),
);
```
Будет отображен бокс 300 x 300.
Loose:
```dart
return ConstrainedBox(
	constraints: BoxConstraints(
		maxHeight: 300,
		minHeight: 200,
		maxWidth: 300,
		minWidth: 300,
	),
	child: ColoredBox(
		color: Colors.blue,
	),
);
```
Будет отображен бокс 300 x 200 (использовались минимальные размеры по умолчанию).

## IntrinsicWidth / IntrinsicHeight

Еще пара полезных виджетов: IntrinsicWidth/IntrinsicHeight.
```dart
return IntrinsicWidth(
	child: ConstrainedBox(
		constraints: const BoxConstraints(
			minHeight: double.infinity,
			minWidget: double.infinity,
			maxWidth: double.infinity,
			maxHeight: double.infinity,
		),
		child: Container(
			width: 100,
			height: 100,
			color: Colors.red,
		),
	), 
);
```
Бокс будет принимать width = 100 и height = infinity. IntrinsicWidget задает необходимую ширину дочернему виджету.
