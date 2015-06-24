# Treating arrays as sets

Even though Ruby has a [set implementation in its standard
library](http://ruby-doc.org/stdlib-2.2.2/libdoc/set/rdoc/Set.html), you might
often find yourself wanting to use a regular array as a set. For example, you
might want to push an element into an array without introducing duplicates:

```ruby
user_ids = [1, 2]
user_ids << new_id unless things.include?(new_id)
```

However, arrays implement several set operations themselves:

* [set intersection](https://en.wikipedia.org/wiki/Intersection_(set_theory)) via [`&`](http://ruby-doc.org/core-2.2.0/Array.html#method-i-26);
* [set difference](https://en.wikipedia.org/wiki/Complement_(set_theory)) via [`-`](http://ruby-doc.org/core-2.2.0/Array.html#method-i-2D);
* [set union](https://en.wikipedia.org/wiki/Union_(set_theory)) via [`|`](http://ruby-doc.org/core-2.2.0/Array.html#method-i-7C).

You can take advantage of these to avoid pushing duplicates like so:

```ruby
user_ids = [1, 2]
user_ids |= [new_id]
```

This works because it is functionally equivalent to:

```ruby
user_ids = [1, 2]
user_ids = user_ids | [new_id]
```

If `new_id` is a duplicate—say `2`—then this expands like so:

```ruby
user_ids = [1, 2]
user_ids = user_ids | [2]
user_ids = [1, 2]
```

If it is a new value—say `3`—then it expands like so:

```ruby
user_ids = [1, 2]
user_ids = user_ids | [3]
user_ids = [1, 2, 3]
```

You can also use set difference in the same way to remove a list of elements:

```ruby
user_ids = [1, 1, 1, 2, 3, 4]
user_ids -= [1]
#=> [2, 3, 4]
```
