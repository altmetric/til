# Destructuring method arguments

When dealing with objects that respond to `to_ary` such as arrays or hashes in
Ruby, you can destructure them on assignment like so:

```ruby
head, *tail = [1, 2, 3, 4]
head
#=> 1
tail
#=> [2, 3, 4]
```

This is also commonly used in block parameters to destructure nested
data structures (e.g. an array of arrays):

```ruby
[[1, 'a'], [2, 'b'], [3, 'c']].map do |(number, letter)|
  letter * number
end
#=> ["a", "bb", "ccc"]
```

However, it can also be used to destructure arguments in method definitions:

```ruby
def rest((head, *tail))
  tail
end

rest [1, 2, 3]
#=> [2, 3]
```
