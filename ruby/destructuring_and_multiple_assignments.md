# Destructuring and multiple assignments

When destructuring an array (or [something that can be coerced to an
array][0]), one might also want to retain the full array, e.g.

```ruby
first, second, third = 3.times.map { SomeObject.new }
all = [first, second, third]
```

It is possible to do both of these things at the same time by combining
destructuring with multiple assignments:

```ruby
first, second, third = all = 3.times.map { SomeObject.new }
```

[0]: https://github.com/altmetric/til/blob/master/ruby/destructuring_objects.md
