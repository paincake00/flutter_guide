
### Объявление класса

```ruby
class A
	def a
		puts 'a'
	end
end

# Создание объекта
instance = A.new
# Вызов метода
instance.a # выведется "a"
```

### Наследование

```ruby
class B < A # наследуется B от A
	def b
		puts 'b'
	end
end

instance_new = B.new

instance_new.b
# b

instance_new.a
# a
```

Переопределение метода.
`super` - позволяет вызвать реализацию этого же метода из родительского класса. 
```ruby
class B < A
	def a
		super # вызывается метод a из класса A
		puts '+ b'
	end
end

instance_new.a
# a
# + b
```

Посмотреть всех предков у класса. `BasicObject` - класс в самом исток.
```ruby
B.ancestors

# => [B, A, Object, Kernel, BasicObject]
```

### Инкапсуляция

Сначала идут методы публичные, потом `protected` и потом уже приватные.

```ruby
class A
	def a
		puts 'a'
	end
	
	private # все методы после этого модификатора - приватные
	
	def b
		puts 'b'
	end
	
	def c
		puts 'c'
	end
end
```

- `public` подходит для методов, которые вызывают пользователи класса.

- `protected` подходит для методов, которые вызываются между объектами одного и того же класса или его наследников.
```ruby
class Account
	def equal?(other)
		balance == other.balance # вызов протектных методов у объектов
	end
	
	protected
	
	def balance
		@balance ||= 100 # если @balance еще nil, то будет присвоено 100 по умолчанию
	end
end

a1 = Account.new
a2 = Account.new

a1.equal?(a2)
```

- `private` подходит для методов, которые могут использоваться только внутри этого объекта (не класса даже).
```ruby
class User
  def print_secret
    secret   # можно — неявный вызов
  end

  private

  def secret
    "shhh"
  end
end

u = User.new
u.print_secret  # работает
u.secret        # ошибка
```