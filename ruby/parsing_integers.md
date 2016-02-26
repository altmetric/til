# Parsing integers

When attempting to parse third-party values as integers (e.g. from an XML feed
or user input), there are a few options available in Ruby.

## `to_i`

The first is to use
[`String#to_i`](http://ruby-doc.org/core/String.html#method-i-to_i):

```ruby
"123".to_i
#=> 123
```

It ignores leading zeroes:

```ruby
"0123".to_i
#=> 123
```

Will round non-integer numbers:

```ruby
"123.3".to_i
#=> 123
```

Ignores non-numerals after a number:

```ruby
"123abc".to_i
#=> 123
```

Returns 0 if there are non-numerals at the start of the input:

```ruby
"Alice says 123".to_i
#=> 0
```

Returns 0 if there are no numbers at all:

```ruby
"abc".to_i
#=> 0
```

Gracefully works with values that are already integers:

```ruby
123.to_i
#=> 123
```

## `Integer`

If you wish to be strict with parsing (in particular, you want to know when a
user passes an invalid number), you can use
[`Integer`](http://ruby-doc.org/core/Kernel.html#method-i-Integer)
instead which is a method on
[`Kernel`](http://ruby-doc.org/core/Kernel.html) in Ruby:

```ruby
Integer("123")
#=> 123
```

It interprets leading zeroes as radix indicators:

```ruby
Integer("0123")
#=> 83
```

Unless you explicitly specify a base as the second argument:

```ruby
Integer("0123", 10)
#=> 123
```

It will raise an exception for non-integer numbers:

```ruby
Integer("123.3")
# ArgumentError: invalid value for Integer(): "123.3"
# from (pry):10:in `Integer'
```

And for any other values that are not integers:

```ruby
Integer("123abc")
# ArgumentError: invalid value for Integer(): "123abc"
# from (pry):13:in `Integer'

Integer("Alice says 123")
# ArgumentError: invalid value for Integer(): "Alice says 123"
# from (pry):14:in `Integer'

Integer("abc")
# ArgumentError: invalid value for Integer(): "abc"
# from (pry):15:in `Integer'
```

Gracefully works for values which are already integers:

```ruby
Integer(123)
#=> 123
```

Unless you also specify a base:

```ruby
Integer(123, 10)
# ArgumentError: base specified for non string value
# from (pry):18:in `Integer'
```
