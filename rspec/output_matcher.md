# Output matcher

Testing the side-effects of code is always tricky, even something as simple as
checking that a subject under test prints to STDOUT or STDERR. The typical
approach is to temporarily redirect `$stdout` or `$stderr` to a
[`StringIO`](http://ruby-doc.org/stdlib-2.2.1/libdoc/stringio/rdoc/StringIO.html)
object and then inspect that.

Thankfully, RSpec wraps this (and more) up in its [`output`
matcher](https://www.relishapp.com/rspec/rspec-expectations/docs/built-in-matchers/output-matcher):

```ruby
RSpec.describe Foo do
  # ...
  it 'prints a greeting' do
    expect { foo.greet }.to output('Hello!').to_stdout
  end
end
```
