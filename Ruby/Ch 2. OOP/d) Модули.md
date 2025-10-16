
Модуль - особая структура для группировки методов, констант и вложенных модулей, которая используется для создания пространств имён (namespaces) и повторного использования кода, в частности, через включение (mixing) в классы. 

В отличие от классов, модули нельзя инстанциировать (создать объекты), а их методы и константы локальны в пределах модуля.

### Обращение к модулю

Обращение (например, к константе) производится через имя модуля и оператор `::` :
```ruby
module Config
	API_URL = "https://api.example.com"
end

puts Config::API_URL # https://api.example.com
```

### Модуль как namespace
 
Классы добавляют в модули редко, это в основном нужно для библиотек, чтобы названия классов твоего кода и библиотек не конфликтовали. А в своем коде просто пишут разные название у классов.
```ruby
module Namespace
	class A
		def self.hello
			"Hello"
		end
	end
end

puts Namespace::A.hello # Hello
```
### Модуль как mixin (примесь)

Это случай применения модулей для добавления логики в какой-то класс. 
Можно поделить на варианта добавления:

1. Добавление методов в качестве методов экземпляра

```ruby
module Greetings
	def hello
		"Hello"
	end
end

class Person
	include Greetings
end

puts Person.new.hello # Hello
```

2. Добавление методов в качестве методов класса

```ruby
module Greetings
	def hello
		"Hello"
	end
end

class Person
	extend Greetings
end

puts Person.hello # Hello
```

### Статические методы модуля

У модуля можно создать и вызвать статическую логику. Способ очень похож как у классов:
```run-ruby
module MathTools
	def self.square(x)
		x * x
	end
end

puts MathTools.square(2) # 4

# либо привычный вариант
puts MathTools::square(5) # 25
```