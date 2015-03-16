# Trailing dots

When chaining method calls (obvious [Law of
Demeter](http://c2.com/cgi/wiki?LawOfDemeter) violations aside) and attempting
to comply with [Rubocop](https://github.com/bbatsov/rubocop), you may find
yourself breaking things up with newlines like so:

```ruby
foo.some_chainable_call
   .some_other_chainable_call
   .a_final_call
```

However, the above only works in Ruby versions later than 1.9 as Ruby 1.8.7
will throw syntax errors:

    syntax error, unexpected '.', expecting kEND

If working on a library that requires 1.8.7 compatibility, you can use the
following alternative:

```ruby
foo.some_chainable_call.
    some_other_chainable_call.
    a_final_call
```

You can then configure Rubocop to use a different [dot position
style](https://github.com/bbatsov/rubocop/blob/master/config/default.yml#L238-L243):

```yaml
Style/DotPosition:
  EnforcedStyle: trailing
```
