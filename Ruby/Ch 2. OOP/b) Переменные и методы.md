
### Ключевое слово `self` и методы

`self` - указывает либо на текущий экземпляр, либо на сам класс - все зависит от его использования:

- внутри **метода экземпляра** → `self` = текущий экземпляр;
- внутри **тела класса или модуля** → `self` = сам класс (или модуль);
- внутри **метода класса** → `self` = объект класса (т.е. сам класс как значение).

Метод класса обозначается через `self.method`:
```ruby
class A
	# метод экземпляра
	def method_obj
		puts self # выведется объект
	end
  
	# метод класса
	def self.method_class
		puts self # выведется класс
	end
end
  
a = A.new
a.method_obj
A.method_class
  
# OUTPUT
# <A:0x0000000102e76d20>
# A
```

### Метод new и initialize

```ruby
class Counter
	# так задается конструктор
	def initialize(initial = 0)
		@current = initial
	end
	  
	def show
		@current
	end
	
	def next
		@current += 1
	end
end
  
c = Counter.new(5)
c.next
puts c.show
# 6
```

### Переменные и их область видимости

Переменные бывают:
- локальные - только внутри метода, блока, файла (если просто написана)
- @экземпляра - внутри всех методов экземпляра
- @@класса - внутри определения и методов всех экземпляров класса
- $глобальные - везде (не используют, так как нарушает принцип инкапсуляции)

1) **Локальная переменная** — видна только внутри метода или блока

```ruby
def greet
	name = "Alice"
	puts "Hello, #{name}"
end

greet # Hello, Alice
puts name  # => Ошибка! name не видно за пределами метода
```

2) **Переменная экземпляра** (`@`) — уникальна для каждого объекта

```ruby
class User
  def initialize(name)
    @name = name    # переменная экземпляра
  end

  def greet
    puts "Hi, I'm #{@name}"
  end
end

u1 = User.new("Alice")
u2 = User.new("Bob")

u1.greet   # => Hi, I'm Alice
u2.greet   # => Hi, I'm Bob
```

3) **Переменная класса** (`@@`) — общая для всех экземпляров

```ruby
class Counter
  @@count = 0   # переменная класса

  def initialize
    @@count += 1
  end

  def self.total
    @@count
  end
end

a = Counter.new
b = Counter.new

puts Counter.total   # => 2
```

4) **Глобальная переменная** (`$`) — видна везде (и в классах, и в методах)

```ruby
$greeting = "Hello, world!"   # глобальная переменная

class Greeter
  def say
    puts $greeting
  end
end

Greeter.new.say   # => Hello, world!
puts $greeting    # => Hello, world!
```

