# Private attributes

We've increasingly been declaring private attributes in Ruby classes using
`attr_reader` or `attr_accessor` after a `private` declaration (c.f.
[Barewords](http://devblog.avdi.org/2012/10/01/barewords/)). However,
@tomstuart pointed out that this actually raises a warning if you run Ruby
with warnings enabled:

```ruby
class Monkey
  private

  attr_accessor :name
end
```

```console
$ ruby -w monkey.rb
monkey.rb:4: warning: private attribute?
```

To work around this, you can use the `private` keyword to explicitly name
private methods:

```ruby
class Monkey
  attr_accessor :name
  private :name, :name=
end
```
