- MediaQuery
Checks changing of viewport size. 
```dart
return switch (MediaQuery.of(context).size.width) {
	final width when width < 500 => Scaffold(
			body: Text("Small"),
		),
	final width when width < 1000 => Scaffold(
			body: Text("Medium", style: title),
		),
	_ => Scaffold(
			body: Text("Large", style: title),
		),
}
```
the Switch expression use in this code.

- Switch expression
Default if-else expression example:
```run-dart
void main() {
	print(_getText(1));
}
String _getText(int param){
	if(param == 1){
		return 'Small';
	} else if(param == 2){
		return 'Medium';
	} else {
		return 'Large';
	}
}
```
Using switch expression:
```run-dart
void main() {
	print(_getText(2));
}
String _getText(int param) => switch(param) {
		1 => 'Small',
		final size when size == 2 => 'Medium',
		_ => 'Large',
	};
```
- LayoutBuilder
Checks changing of definite widget's size via `constraints` of this widget.
```dart
return LayoutBuilder(
	builder: (context, constraints) {
		return switch (constraints.maxWidth) {
			final width when width < 400 => Column(
					children: children,
				),
			_ => Row(
					children: children,
			),
		};
	},
);
```
- OrientationBuilder
The wrapper over LayoutBuilder. It checks a orientation (Portrait or Landscape).
```dart
return OrientationBuilder(
	builder: (context, orientation) {
		return switch (orientation) {
			Orientation.portrait => Center(
				child: Text(
					'Portrait',
					style: title,
				),
			),
			_ => Center(
				child: Text(
					'Landscape',
					style: title,
				),
			),
		};
	},
);
```
- Constraints
There are two types of constraints: Tight (=) and Loose (>).
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
Will be rendered the red box 300 x 300.
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
Will be rendered the blue box 300 x 200 (use min values by default).
Also there is a coupe of the useful widgets: IntrinsicWidth/IntrinsicHeight.
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
Will be rendered the red box with width = 100 and height = infinity. The IntrinsicWidget make needed width for child widget.
- FittedBox
This widget lets to fit child widget in parent with `fit` parameter (BoxFit).
```dart
void main() => runApp(
	Center(
		child: Container(
			width: 400,
			height: 400,
			color: Colors.blue,
			child: const Align(
				child: FittedBox(
					fit: BoxFit.contain,
					child: SizedBox(
						height: 300,
						width: 200,
						child: ColoredBox(
							color: Colors.red,
						),
					),
				),
			),
		),
	),
);
```
- AspectRatio
This widget (as previous) lets to fit (and to scale) a child widget via aspect ratio.
```dart
void main() => runApp(
	Center(
		child: Container(
			width: 400,
			height: 400,
			color: Colors.blue,
			child: const Align(
				child: AspectRatio(
					aspectRatio: 2 / 3,
					child: ColoredBox(
						color: Colors.red,
					),
				),
			),
		),
	),
);
```
- FractionallySizedBox
Another widget which sets width/height of child widget depending on parent widget via using of factors.
```dart
void main() => runApp(
	Center(
		child: Container(
			width: 400,
			height: 400,
			color: Colors.blue,
			child: const Align(
				child: FractionallySizedBox(
					widthFactor: .5, // 50% of parent's width
					heightFactor: .8 // same as with width
					child: ColoredBox(
						color: Colors.red,
					),
				),
			),
		),
	),
);
```
