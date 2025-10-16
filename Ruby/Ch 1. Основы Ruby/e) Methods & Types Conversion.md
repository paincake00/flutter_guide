
### Метод с дефолтным значением:

```ruby
def hi(name="ruby") # по умолчанию
	result = "hello " + name 
	return result
end

hi("user")
# => hello user

hi
# => hello ruby

hi "user"
# => hello user

# возвращается последнюю строку в методе по умолчанию
def hi_1(name)
	"hello " + name
end
```

### Преобразование типов

![[Pasted image 20251001214413.png|500]]

`.is_a?(<type>)` - может проверять является ли объект экземпляром определенного класса (`<type>`) или любого из его подклассов:

```ruby
arr = [1, 2, 3]
puts arr.is_a?(Array) # true
puts arr.is_a?(Object)  # => true (Array наследуется от Object)
puts arr.is_a?(Hash)    # => false

hash = { a: 1 }
puts hash.is_a?(Hash)   # => true
puts hash.is_a?(Array)  # => false
```

### Методы с `?`

Методы, заканчивающиеся на `?`, **должны возвращать логическое значение** (`true` или `false`). Они отвечают на **вопрос**.

```ruby
5.even? # false
"hello".empty? # false
[1, 2].include?(2) # true
nil.nil? # true
```

Инициализация собственного метода-предиката:
```ruby
class User
	def active?
		status == 'active'
	end
end

user = User.new
user.active?
```

### Методы с `!`

Нужен для изменения конкретного переданного объекта, а не создания нового. Может также применяться для выбрасывания исключения вместо простого вывода `false`/`nil`.

```ruby
str = "Hello"
str.upcase    # => "HELLO"   (новая строка)
str           # => "Hello"   (оригинал не изменился)

str.upcase!   # => "HELLO"   (изменил саму строку)
str           # => "HELLO"
```