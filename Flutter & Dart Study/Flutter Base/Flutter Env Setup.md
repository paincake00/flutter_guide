
<center>macOS</center>

---
#### Java

Установка jdk производится по пути:
`/Library/Java/JavaVirtualMachines/`
Например: `/Library/Java/JavaVirtualMachines/jdk-23.jdk`

Посмотреть версию текущей java `/usr/libexec/java_home -v`

#### Gradle

Установка SDKMAN!: `curl -s https://get.sdkman.io | bash`

Добавление SDKMAN! в профиль, чтобы он был доступен в каждой сессии:
`source "$HOME/.sdkman/bin/sdkman-init.sh"`

Посмотреть версии gradle: `sdk list gradle`

Установить gradle определенной версии: `sdk install gradle <version>`

Использовать gradle: `sdk use gradle <version>`

Установка как версии по дефолту: `sdk default gradle <version>`

Посмотреть версию: `gradle -v`