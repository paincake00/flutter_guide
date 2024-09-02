For example, import font files in .ttf: `OpenSans-ExtraBold.ttf` and `OpenSans-Regular.ttf`. 

In `pubspec.yaml` add this fonts (may to indicate weight, style and etc):
```yaml
fonts:
    - family: Open Sans
      fonts:
        - asset: assets/fonts/OpenSans-ExtraBold.ttf
          weight: 800
        - asset: assets/fonts/OpenSans-Regular.ttf
          weight: 400
```

End now enough to write the next code in build method:
```dart
@override
Widget build(BuildContext context) {
    return MaterialApp(
      theme: ThemeData(
        colorScheme: ...,
        textTheme: Theme.of(context).textTheme.apply(
              fontFamily: 'Open Sans',
            ),
      ),
      home: ...,
    );
  }
```

