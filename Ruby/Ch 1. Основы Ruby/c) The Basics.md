
## Literals

Литералы, которыми можно вводить фиксированные значения в коде.

```ruby
'Hello, world!'          # string literal (mutable)
375                      # integer literal
3.141528                 # float literal
true                     # boolean literal
{ 'a' => 1, 'b' => 2 }   # hash literal
[ 1, 2, 3 ]              # array literal
:sym                     # symbol literal (immutable)
nil                      # nil literal
```

## Strings

Инициализация строки:
```ruby
# Ex. 1: with double quotes
"The man said, 'Hi there!'"

# Ex. 2: with single quotes and escaping
'The man said, \'Hi there!\''
```

Строка, заданная через двойные кавычки, позволяет использовать string interpolation через`#{...}`.

```run-ruby
b = 3
a = "ten #{b}"

puts a
```

Многострочный текст:
```run-ruby
doc = <<DOC
Это многострочный и очень
большой текст
который не влезает
на одну строку
DOC

puts doc
```

## Symbols

```ruby
# Examples of symbols
:name
:a_symbol
:"surprisingly, this is also a symbol"
```

## Numbers

```ruby
# Example of integer literals
1, 2, 3, 50, 10, 4345098098

# Example of float literals
1.2345, 2345.4267, 98.2234

1.even? # => false

1.odd? # => true
```

## nil

```run-ruby
x = nil # nil literal used here
puts x
```

Проверка на nil:
```run-ruby
puts "Some String".nil?

if nil
	puts "First case completed"
end

if 1 # some value (not nil or false)
	puts "Second case completed"
end
```

## Operations

##### For Integers

```ruby
# adding
puts 1 + 1
# subtracting
puts 1 - 1
# multiplying
puts 2 * 3
# division
puts 5 / 2
# modulo
puts 5 % 2
# remainder
puts 16.remainder(5) # остаток = 1
# divmod
puts 16.divmod(5) # [3, 1]
```

Разница между способами деления:
![[Pasted image 20251003124223.png|500]]

##### For Floats

Division:
```run-ruby
puts 15.0 / 4 # 3.75
```

Multiplying:
```run-ruby
puts 9.75 * 4.32 # 42.120000000000005
```

##### Equality Comparison

```run-ruby
puts 4 == 4 # true
puts 's' == 's' # true
puts '4' == 4 # false
```

##### String Concatenation

```run-ruby
puts 'foo' + 'foo'
puts "foo" + "bar"
puts "foo" + 1.to_s() # привести к string
```


## Type Conversation

```run-ruby
1.to_s
# => "1"

4.to_s(2) # двоичная система счисления
# => 100

"1".to_i
# => 1

99.9.to_s
# => "99.9"

"99.9".to_f
# => 99.9
```
