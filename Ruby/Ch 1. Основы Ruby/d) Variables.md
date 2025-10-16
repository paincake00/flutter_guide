
## Правила именования

Соглашения по именованию:
- snake_case - переменные, методы
- CamelCase - классы, модули
- UPPERCASE - константы

Переменные:
```ruby
# Локальная переменная — обычное имя в нижнем регистре с подчёркиваниями
user_count = 5

# Переменная экземпляра — начинается с @
@current_user = "Alice"

# Переменная класса — начинается с @@
@@total_users = 0

# Глобальная переменная — начинается с $
$global_setting = true

# Можно использовать цифры и подчёркивания, но не начинать с цифры
age_2025 = 30
_user_id = 123  # допустимо, хотя редко используется
```

Методы:
```ruby
# Методы
def new_method
	# ...
end
```

Константы:
```ruby
CONSTANT = 1
```

Классы и модули:
```ruby
# Классы и модули
class NewClass
	# ...
end

module NewModule
	# ...
end
```

## Ввод от пользователя

```run-ruby
a = gets.chomp # ждет ввод пользователя, а chomp удаляет символ переноса строки
puts a
```

## Variable Scope

##### Blocks

Блоки это части кода, следуемые за вызовом метода и ограниченные `{}` или `do/end`.

```ruby
total = 0
[1, 2, 3].each {|number| total += number}
puts total # 6
```

```ruby
total = 0
[1, 2, 3].each do |number|
  total += number
end
puts total # 6
```