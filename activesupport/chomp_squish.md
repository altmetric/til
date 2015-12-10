# From `chomp` to `squish`: methods for removing whitespace in Ruby and ActiveSupport

If you have a text string and need to get rid of some whitespace, Ruby's standard library has a couple of methods to help you:

### [`chomp`][chomp]

Removes a given record separator (by default `\n`, `\r` or `\r\n`) from the end of the string. It can be useful in cases such as `IO.readlines` where the line separator is returned at the end of each line.

If the string doesn't have the record separator at the end, then the string won't be modified at all.

**Gotchas:** if your string ends in multiple line breaks, `chomp` will be default only remove one of them. You can specify a separator of an empty string (`''`) to get chomp to remove multiple line separators:

```ruby
"Hello World\r\n".chomp         # => "Hello World"
"Hello World\r\n\r\n".chomp     # => "Hello World\r\n"
"Hello World\r\n\r\n".chomp('') # => "Hello World"
# you're not limited to 'proper' record separators, either...
"Hello World".chomp(' World')   # => "Hello"
```

### [`chop`][chop]

Removes the last character of a string, whatever it is (unless it's an `\r\n` combination, in which case they're both removed). In most cases `chomp` is preferable, as you can specify the string ending that you want removed.

### [`strip`][strip]

Removes all whitespace at the beginning and end of a string.

```ruby
"    hello    ".strip   #=> "hello"
"\tgoodbye\r\n".strip   #=> "goodbye"
```

## ActiveSupport-supplied methods

If those methods aren't enough for you, ActiveSupport supplies a couple more, both of which are useful when including multi-line strings in `heredoc` syntax:

### [`strip_heredoc`][strip_heredoc]

Gets rid of 'indentation' within a multi-line string, by removing enough whitespace from the start of each line so that at least one line has text in column 0.

```ruby
puts <<-USAGE.strip_heredoc
    This command does such and such.

    Supported options are:
      -h         This message
      ...
USAGE

# => "This command does such and such.\n\nSupported options are..."
```

### [`squish`][squish]

Removes all whitespace from the beginning and end of a string (like `strip`), but also replaces any blocks of whitespace inside the string into a single space. This is especially useful when constructing SQL queries:

```ruby
query = <<-SQL.squish
   SELECT articles.*, users.name
   FROM articles LEFT JOIN users
        ON articles.author_id = users.id
   WHERE articles.category = "blog post"
   LIMIT 10;
SQL

# => "SELECT articles.*, users.name FROM articles LEFT JOIN users ON..."
```

[chomp]: http://ruby-doc.org/core-2.2.3/String.html#method-i-chomp
[chop]: http://ruby-doc.org/core-2.2.3/String.html#method-i-chop
[strip]: http://ruby-doc.org/core-2.2.3/String.html#method-i-strip
[strip_heredoc]: http://api.rubyonrails.org/classes/String.html#method-i-strip_heredoc
[squish]: http://api.rubyonrails.org/classes/String.html#method-i-squish
