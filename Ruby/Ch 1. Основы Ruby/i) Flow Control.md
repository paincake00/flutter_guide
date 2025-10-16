
В ruby истиной для условий может являться все кроме false и nil.

```ruby
i = 1
if i > 0 # выполняется когда условие является истиным
	puts i # 1
end

unless i < 0 # выполняется когда условие явялется ложным
	puts i # 1
end
```

elsif:
```ruby
if x == 3 
	puts "x is 3" 
elsif x == 4 
	puts "x is 4" 
end

# Можно писать в одну строку
if x == 3 then puts "x is 3" end
```

Модификаторы:
```ruby
puts "x is 3" if x == 3

puts "x is not 3" unless x == 3
```

Тернарный оператор:
```ruby
i = 1
i > 0 ? "положительный" : "неположительный"
```

case выражение:
```ruby
a = 5

res = case a
	when 5 
		puts "a is 5" 
	when 6 
		puts "a is 6" 
	else 
		puts "a is neither 5, nor 6"
	end

puts res
```

Оператор `===`:
```ruby
1 === 1 # true
String === "some_str" # true

case var
when String
	var.upcase
when Integer
	var * 2
end
```