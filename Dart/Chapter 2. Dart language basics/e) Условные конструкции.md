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
