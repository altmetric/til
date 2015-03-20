# Module function

We were recently working on some Ruby functionality where we needed to
define a couple of methods at the class level and @mudge pointed out
that we could use the not so well known `module_function`, like so:

```ruby
module Foo
  module_function

  def bar
    "hello world"
  end
end
```
```ruby
Foo.bar  # => "hello world"
```

As you can see, the methods are defined as instance methods, and so
that allows us to also include the methods onto other objects:

```ruby
class Baz
  include Foo

  def foobar
    bar.upcase
  end
end
```
```ruby
Baz.new.foobar #=> "HELLO WORLD"
```

However, one thing to note is that the included instance methods are defined as private:

```ruby
Baz.new.bar #=> NoMethodError: private method `bar' called for#<Object:0x007fc9860dfb18>
```

If you want to make them public, you can pass the method names to `module_function`:

```ruby
module_function :foo
```
