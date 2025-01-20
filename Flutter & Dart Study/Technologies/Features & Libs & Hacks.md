**fmv** - a simple cli to manage Flutter SDK (include Dart SDK) versions per project. Support channels, releases, and local cache for fast swicthing between versions.

Installing:
- Use this package as an executable
You can install the package from the command line:
```
dart pub global activate fvm
```
The package has the following executables:
```
$ fvm
```
- Use this package as a library
With Dart:
```
$ dart pub add fvm
```
With Flutter:
```
$ flutter pub add fvm
```
After, enough add dependence in pubspec.yaml (and run an implict `dart pub get`).

Example:
This will install version 1.17.4 and cache locally:
```
> fvm install 1.17.4
```
Go into the project directory:
```
> cd path/to/project
```
Set the project to use the version that you have installed:
```
> fvm use 1.17.4
```

### [Flutter & Dart snippets](https://codewithandrea.com/articles/vscode-shortcuts-extensions-settings-flutter-development/#5-flutter-&-dart-snippets)

The Dart and Flutter plugins include many snippets that you can use to add common Flutter widgets quickly.

You may already be familiar with these:

- `stless`: Insert a `StatelessWidget`
- `stful`: Insert a `StatefulWidget`
- `stanim`: Insert a `StatefulWidget` with an `AnimationController`

For generation:

```sh
dart run build_runner build --delete-conflicting-outputs
```