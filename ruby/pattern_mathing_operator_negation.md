# Negation of pattern-matching operator

The use of the [pattern-matching operator][0] is very common in Ruby, whenever
we need to verify if a string matches a certain pattern:

```ruby
/hay/ =~ 'haystack'   #=> 0
'haystack' =~ /hay/   #=> 0
/a/   =~ 'haystack'   #=> 1
/u/   =~ 'haystack'   #=> nil
```

If we need the negative version of this operator, instead of using `=~` :

```ruby
month = Time.now.strftime("%B")
puts 'The month does not start with M.' if !(month =~ /\AM/)
```

we can use the cleaner version `!~` :
```ruby
month = Time.now.strftime("%B")
puts 'The month does not start with M.' if month !~ /\AM/
```

[0]: http://docs.ruby-lang.org/en/2.1.0/regexp_rdoc.html
