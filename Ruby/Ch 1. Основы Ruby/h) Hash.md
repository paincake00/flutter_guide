
Объявление Hash:
```ruby
hash = {
  'key' => 'value',
  12    => 'another value',
  :a    => 13,
  :b    => 14
}

hash[12]
# => "another value"

hash[:a]
# => 13
```

Создание пустого хеша и добавление элементов:
```ruby
hash_new = Hash.new
hash_new == {}
# => true

new[:c] = 15
hash_new
# => {:c=>15}
```

Символы в качестве ключей для хеша:
```ruby
hash_2 = {a: 1, b: 2, c: 3}
hash_2[:c]
# => 3
```

Хэш как аргумент метода:
```run-ruby
def a(opt = {})
	puts "opt - #{opt}"
end

a(one: 1, two: 2)
# opt - {:one=>1, :two=>2}
```

Методы `merge` и `merge!` для hash:
```ruby
h1 = {a:1, b:2, c:3}
h2 = {c:5, d:6}
h1.merge(h2)
# => {:a=>1, :b=>2, :c=>3, :c=>5, :d=>6}
h1
# => {:a=>1, :b=>2, :c=>3}

h1.merge!(h2) # изменит h1
h1
# => {:a=>1, :b=>2, :c=>3, :c=>5, :d=>6}
```