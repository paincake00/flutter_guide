
### [Flutter & Dart snippets](https://codewithandrea.com/articles/vscode-shortcuts-extensions-settings-flutter-development/#5-flutter-&-dart-snippets)

The Dart and Flutter plugins include many snippets that you can use to add common Flutter widgets quickly.

You may already be familiar with these:

- `stless`: Insert a `StatelessWidget`
- `stful`: Insert a `StatefulWidget`
- `stanim`: Insert a `StatefulWidget` with an `AnimationController`



### Command

analyze:
```sh
flutter analyze
```

code format (in the root of project):
```sh
dart format .
```

For generation (for all):
```sh
dart run build_runner build --delete-conflicting-outputs
```
or (for new):
```
dart run build_runner build
```



### Flutter | Dart 

Debug и Hot Reload/Restart в терминале:

`r` - hot reload
`R` - hot restart
`F8` - step over
`F7` - step into
`shift + F7` - step out
`F5` - resume