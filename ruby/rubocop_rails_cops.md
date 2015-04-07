# Using Rubocop's Rails cops

The style guide checker [Rubocop][] has a collection of cops which are specific to [Rails][]. However, this collection is not automatically enabled by default.

They can be invoked on the command line using the `--rails` or `-R` options:

    rubocop -R

If using `guard-rubocop`, the option can be included in the `cli` parameter:

```ruby
guard :rubocop, cli: ['--rails', '--display-cop-names'] do
  watch(%r{.+\.rb$})
  watch(%r{(?:.+/)?\.rubocop\.yml$}) { |m| File.dirname(m[0]) }
end
```

But in most cases, you will want to have them enabled all the time, if you are using Rubocop on a Rails project. You can instead switch the Rails cops on in the project's `.rubocop.yml` file, which means that they will be enabled automatically when using the command line, `guard` or any IDE linter that correctly parses the configuration files.

To switch on Rails cops within the entire project, add the following to `.rubocop.yml`:

```yaml
AllCops:
  RunRailsCops: true
```


[Rubocop]: https://github.com/bbatsov/rubocop
[Rails]: http://rubyonrails.org
