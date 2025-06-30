
Для firebase необходима iOS 13 (как минимальная версия). 
Для этого надо в `Podfile` явно указать версию в строке: 

`platform :ios, '13.0'`

Потом открыть через Xcode файл `Runner.xcworkspace` и заменить в конце минимальную версию на 13.

Потом:
```sh
cd ios
pod deintegrate
pod repo update
pod install --repo-update
flutter clean
flutter pub get
```

И потом можно запускать.

### Установка

Доп. ресурс: https://firebase.google.com/docs/flutter/setup?hl=ru&platform=ios

Зайти на сайт в firebase и создать приложение.

Установить firebase tools через команду: `curl -sL https://firebase.tools | bash`

Войдите в Firebase, используя свою учетную запись Google, выполнив следующую команду: `firebase login`

Установите интерфейс командной строки FlutterFire, выполнив следующую команду из любого каталога:  ```dart pub global activate flutterfire_cli``` 
(возможно надо будет добавить в `.zshrc` строчку `export PATH="$PATH":"$HOME/.pub-cache/bin"`)

Далее ввести
`flutterfire configure --project=\<id приложения из firebase>`

Дальше будет инициализация (добавятся конфиг файлы)

Затем можно добавить пакет: `flutter pub add firebase_core`

