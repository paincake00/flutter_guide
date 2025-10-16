
Объявление массива:
```ruby
[1, 'string', 99.9]
# => [1, "string", 99.9]

Array.new(10, 0)
# => [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]

%i(one two three)
# => [:one, :two, :three]

%w(one two three)
# => ["one", "two", "three"]
```

Доступ к элементам массива:
```ruby
a = [1, 'string', 99.9]

a[1]
# => "string"

a.[](1)
# => "string"

a[4] = 88.8

a
# => [1, "string", 99.9, nil, 88.8]
```

- Array# `<<` - добавление элементов в конец.
- Array# `delete_at` - удаление по индексу.
- Array# `-` - разность массивов.
- Array# `+` - конкатенация массивов.