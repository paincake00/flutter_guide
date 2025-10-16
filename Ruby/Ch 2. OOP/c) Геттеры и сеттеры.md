
### Базовая запись, без синтаксического сахара

```ruby
class User
	def initialize(name)
		@name = name
	end
	
	# геттер
	def name
		@name
	end
	
	# сеттер
	def name=(value)
		@name = value
	end
end

user = User.new("Bob")
puts user.name # Bob
user.name = "Alice"
puts user.name # Alice
```

### Сокращенный вариант создания геттеров и сеттеров

`attr_reader` — перечисляет геттеры
`attr_writer` -- перечисляет сеттеры

```ruby
class Counter
	attr_reader :current # создаёт метод current, который возвращает @current
	attr_writer :current # создаёт метод current=, чтобы менять @current
	
	def initialize
		@current = 0
	end
end

c = Counter.new

c.current = 5
puts c.current # 5
```

`attr_accessor` -- в нем можно перечислить переменные, для которых будет создан и геттер, и сеттер

```ruby
class Counter
  attr_accessor :current   # создаёт и current, и current=

  def initialize
    @current = 0
  end
end

c = Counter.new
puts c.current   # 0
c.current = 10
puts c.current   # 10
```