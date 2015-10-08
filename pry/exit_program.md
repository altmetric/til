# Exit program or `!!!`

When debugging programs with [Pry](http://pryrepl.org/), you might find
yourself placing a breakpoint within a loop or commonly-called function like
so:

```ruby
results = some_huge_array.map do |element|
  result = foo(element)
  binding.pry
end
```

Once you've finished debugging, it can be quite painful to exit out of the
program if there are still many more times `binding.pry` will be invoked (or,
worse still, you're caught in an infinite loop).

To escape from this situation, you can simply enter `!!!` at the prompt:

```
[1] pry(main)> !!!
```

This is an alias for the Pry command `exit-program` and will cleanly exit your
program.
