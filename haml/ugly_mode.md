# Use Haml's "ugly" mode in development

Recently, we hit an issue where an HTML page constructed from Haml templates exhibited different whitespace behaviour on development machines than on staging or in production.

Needless to say, this is annoying.

The culprit is Haml's rendering mode. In production and testing, it uses a higher-performance rendering method, which it calls "ugly mode", but the option is off by default in development.

In 'non-ugly' mode, Haml outputs HTML with full indentation. In ugly mode, Haml doesn't bother calculating the indentation level for each element, which contributes to the increase in performance. In the real world, however, that source code indentation is not used all that much -- we tend to use the HTML inspectors in Chrome or Safari, which render the DOM tree themselves and apply their own indentation, throwing away all that expensively-calculated whitespace that Haml has created.

To better increase compatibility between development, test and production modes, it's therefore best to ensure that the high-performance ugly mode is used across all environments.

## To switch ugly mode on

In a Rails app, in either `config/application.rb` or an initializer file (e.g., `config/initializers/Haml.rb`:

```ruby
Haml::Template.options[:ugly] = true
```

## References
* [Why you should start using Haml's ugly mode in development today](https://coderwall.com/p/tashig/why-you-should-start-using-haml-s-ugly-mode-in-development-today)
* [Some of Haml's tools for controlling whitespace](http://haml.info/docs/yardoc/file.REFERENCE.html#whitespace_removal__and_) (once you have whitespace management consistent across environments!)
