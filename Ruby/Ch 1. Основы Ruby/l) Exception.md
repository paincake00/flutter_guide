
Исключение — это **объект**, представляющий ошибку или необычное событие.

В Ruby все исключения — это **экземпляры класса `Exception` или его подклассов**.
```
Exception
 └── StandardError
      ├── ArgumentError
      ├── TypeError
      ├── RuntimeError
      ├── NoMethodError
      ├── NameError
      ├── ZeroDivisionError
      └── ... и многие другие
 └── SystemExit
 └── SignalException
 └── Interrupt
```

### Как выбрасывать исключение?

С помощью ключевого слова **`raise`** (синоним — `fail`).
```ruby
# 1. Просто RuntimeError с сообщением
raise "Что-то пошло не так!"

# 2. Указать тип исключения
raise ArgumentError, "Неверный аргумент"

# 3. Создать объект вручную
raise MyCustomError.new("Подробности")

# 4. Перебросить текущее исключение (внутри rescue)
raise
```

### Перехват исключений

Это можно делать через блоки `begin`/`rescue`:
```ruby
def method
	begin
		1 / 0
	rescue
		puts 'got an error'
	end
end
```

Можно опускать `begin`:
```ruby
def method
	1 / 0
	rescue
		puts 'got an error'
	end
end
```

### retry, ensure, else

```ruby
i = 0
begin
	1/i
	puts i
rescue
	i += 1
	retry
end
```

```ruby
i = rand(0..1)
begin
	1/i
rescue
	puts "Ошибка"
else
	puts "Ошибок нет"
ensure
	puts "Выполнится в любом случае"
end
```

### Получение объекта исключения

```ruby
begin
	work
rescue => e
	puts "got #{e.class} with message #{e.message}"
end
```

Получить исключение определенного типа:
```ruby
begin
	work
rescue RuntimeError => e
	puts "got #{e.class} with message #{e.message}"
end
```