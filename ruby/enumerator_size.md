# Enumerator#size

We've been recently wrapping a couple of external APIs with [Enumerators](http://ruby-doc.org/core-2.2.1/Enumerator.html)
and we were having some small issues when asking an enumerator for its
size. So, let's say you have something like this:

```ruby
result_from_api = [1, 2, 3]

wrapper = Enumerator.new do |yielder|
  result_from_api.each do |n|
    yielder << n
  end
end
```

Then, if you call `#size`, you'd get a `nil`:

```ruby
wrapper.size # => nil
```

And that's correct as Ruby does not know if the enumeration has
actually finished. However, after looking at the documentation we noticed
that `Enumerator#new` can take as a first argument a block to tell
Ruby how to calculate the size. So, the previous example now becomes:

```ruby
result_from_api = [1, 2, 3]

wrapper = Enumerator.new(->(){ result_from_api.size }) do |yielder|
  result_from_api.each do |n|
    yielder << n
  end
end
```

Then calling `#size` gives us a more useful value:

```ruby
wrapper.size => 3
```
