
### *Операции сравнения*
- ==
```dart
int a = 10;
int b = 4;
bool c = a == b;        // false
bool d = a == 10;       // true
```
- !=
```dart
int a = 10;
int b = 4;
bool c = a != b;        // true
bool d = a != 10;       // false
```
- \</>
```dart
int a = 10;
int b = 4;
bool c = a < b;        // false
bool d = a > 11;      // false
bool e = b >= 4;     // true   
```

### *Логические операции*

```dart
bool a = true;
bool b = false;

bool c = false;
c = !b; // c = true
c = a || b; // c = true
c = a && b; // c = false
```

### *Операции присваивания*

```dart
int a = 5;
a += 10; // 15
a -= 3; // 12
a *= 2; // 24
a ~/= 6; // 4
a <<= 4; // 64
a >>= 2; // 16
print(a); // 16
```
