# Regular expression intersection

As in many programming languages, [character classes in Ruby regular
expressions][0] allow you to specify a whitelist or blacklist of characters
for a match:

```ruby
# match any letter from a to z
/[a-z]/

# match anything *except* the letters from a to z
/[^a-z]/
```

However, it is also possible to create more complicated character classes by
using the `&&` operator within a character class to perform a set intersection
on its arguments. This is particularly useful when using a predefined class
through POSIX bracket classes or [character properties][1]:

```ruby
# match any letter a to z *except* the letters d to f
/[a-z&&[^d-f]]/

# match any alphanumeric character *except* the numbers 4 to 9
/[[[:alnum:]]&&[^4-9]]/

# match any punctuation *except* a forward slash
%r{[\p{Punct}&&[^/]]}
```

[0]: http://ruby-doc.org/core-2.1.1/Regexp.html#class-Regexp-label-Character+Classes
[1]: http://ruby-doc.org/core-2.1.1/Regexp.html#class-Regexp-label-Character+Properties
