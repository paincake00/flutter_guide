
### loop

`loop` позволяет создавать бесконечный цикл с выходом через `break`:
```ruby
loop do
	puts "Printing until you cancel via Ctrl+C"
end
```

```ruby
i = 0
loop do
	i = i + 1
	puts i
	if i > 10
		break
	end
end
```

### while и until

```run-ruby
i = 1
while i<5 # выпролняется пока условие истино
	puts i
	i += 1
end
```

```run-ruby
i = 1
until i>4 # выполняется пока условие ложно
	puts i
	i += 1
end
```

Модификаторы с while:
```run-ruby
i = 0
puts i+=1 while i <= 4
```

### for

```run-ruby
for i in [1, 2, 3, 4, 5]
	puts i
end
```

`next` - переход к следующему элементу
`break` - досрочный выход из цикла
`redo` - повтор текущей итерации

```run-ruby
for i in [1, 2, 3, 4, 5, 6, 7]
	next if i%2 == 0
	break if i > 5
	puts i
end
```


### blocks

**Блок — это анонимный кусок кода**, который можно передать методу.

Блок можно задать через фигурные скобки `{}` или ключевые слова `do..end`.

`times` вызывается у целого числа и выполняет переданный блок столько раз, сколько указано в этом числе.
```run-ruby
5.times {
	puts "Привет"
}
```

```run-ruby
5.times do |i|
	puts "Привет " + i.to_s
	puts "Ruby"
end
```

```run-ruby
{a: 1, b: 2}.each {
	|k, v| puts "#{k} - #{v}"
}
```

Дополнительные применения:
```ruby
# Работа с файлами
File.open("file.txt") do
	|file| file.write("Новое предложение")
end

# Таймеры
Timeout.timeout(5) do
	# Код, который должен завершиться за 5 секунд
end
```

### iterators

Итерация по коллекции:
```run-ruby
[1, 2, 3].each {
	|i| puts i
}
# 1
# 2
# 3
```

Итерация по индексам и элементам:
```run-ruby
fruits = ["яблоко", "банан", "апельсин"]

fruits.each_with_index do |fruit, index|
  puts "#{index + 1}. #{fruit}"
end
```

Применение функции-преобразователя:
```ruby
[1, 2, 3].map {
	|i| i**2
}
# => [1, 4, 9]
```

Фильтрация:
```ruby
[1, 2, 3].select {
	|i| i%2==0
}
# => [2]
```

Проверка для всех значений:
```ruby
[2, 4, 6].all? { |x| x.even? }  # => true
[1, 2, 3].all? { |x| x > 0 }    # => true
[].all? { false }               # => true (вакуумная истина)
```

Цепочка вызовов итераторов:
```ruby
100.times.select { |i| i%2==0 }
		 .select { |i| i%3==0 }
		 .each { |i| puts i }
```

### yield и call

Можно вызывать переданный блок (и даже передавать в него значение) через `yield`:
```run-ruby
def repeat_twice
	yield("первый вызов")
	yield("второй вызов")
end

repeat_twice { |msg| puts "Сообщения: #{msg}" }
```

Проверку передан ли блок можно сделать с помощью `block_given?`:
```run-ruby
def greet
	if block_given?
		yield("Денис") 
	else
		puts "Привет, незнакомец"
	end	
end

greet { |name| puts "Привет, #{name}" } # => Привет, Денис
greet # => Привет, незнакомец
```

##### Блок ≠ Proc ≠ Lambda (но они связаны)

- **Блок** — синтаксическая конструкция, передаётся при вызове метода.
- **`Proc`** — объект, который **обёртывает блок**.
- **`Lambda`** — особый вид `Proc` с более строгой проверкой аргументов.

Можно работать с блоками как с объяектами (сохранять их, передавать их дальше). Их оборачивают в `Proc` и передают например как `&block`.

С помощью метода `call`, вызываемого у Proc, можно развернуть и исполнить сам блок:
```run-ruby
def make_proc(&block)
	block
end

my_proc = make_proc { puts "Привет из Proc" }
my_proc.call # => Привет из Proc
```

Можно также проверить передан ли блок (но уже для Proc):
```run-ruby
def greet(&block)
	block.call("Денис") if block
end

greet { |msg| puts "Привет, #{msg}" }
# => Привет, Денис
```

###### Lambda

Proc с более строгой проверкой аргументов:
```ruby
l = ->(a, b) { a + b }
l.call(1, 2)   # => 3
l.call(1)      # => ArgumentError: wrong number of arguments (1 for 2)
```

Есть несколько вариантов вызова лямбды:
```ruby
square = ->(n) { n * n }

puts square.call(5)   # => 25
puts square.(4)       # => 16 (альтернативный синтаксис вызова)
puts square[3]        # => 9  (ещё один способ)
```