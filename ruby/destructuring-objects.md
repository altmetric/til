# Destructuring objects

As destructuring in Ruby uses `to_ary` under the hood, it is possible to
implement destructuring for your own objects:

```ruby
class User
  attr_reader :name, :age

  def initialize(name, age)
    @name = name
    @age = age
  end

  def to_ary
    [name, age]
  end
end

alice = User.new('Alice', 40)
name, age = alice

name
#=> 'Alice'
age
#=> 40
```
