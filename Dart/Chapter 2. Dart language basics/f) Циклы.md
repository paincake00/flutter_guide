
### *for*

```run-dart
void main() {
	for (int i=1, j=1; i < 10 && j < 10; i++, j++) {
		print(i * j);
	}
}
```
`for` без тела:
```run-dart
void main() {
	int sum = 0;
	for(int i=1; i < 10; sum+=1, i++);
	print(sum);
}
```

### *Цикл do*

```run-dart
void main() {
	int n = 0;
	do{
		print(n);
		n++;
	}
	while (n < 5);
}
```

### *Цикл while*

```run-dart
void main() {
	int n = 0;
	while(n < 5) {
		print(n);
		n++;
	}
}
```

### *Операторы continue и break*

*continue:*
```run-dart
void main() {
	for(int i=0; i<10; i++) {
		if(i%2==1) {
			continue;
		}
		print(i);
	}
}
```
*break:*
```run-dart
void main() {
	int sum = 0;
	for (int i=1; i<10; i++) {
		sum += i;
		if (sum > 10) {
			break;
		}
	}
	print(sum);
}
```