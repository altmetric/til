# Passing blocks to Logger

When adding logger statements to Ruby or Rails apps, we're used to providing interpolated strings to whichever method matches the status level of our message:

```ruby

logger.info "Number of items matching our requirements: #{items.count(&:valid?)}"
```

Whether or not this string gets output to the log is dependent on the logger instance's `#level` property, which can vary (say, between development and production environments).

However, even if the `logger.level` determines that the text should not be output to the log, any ruby code called by the interpolated string will still be executed.

As an alternative, each of the `Logger` class's logging methods can take a block that returns a string. This block will only be called, and any Ruby code within will only be executed, if the severity is of a sufficient level to be output to the log:

```ruby

logger.level = Logger::WARN

# String parameter - Calculated, but not displayed
logger.debug "Retrieved items: #{items.inspect}"

# Block - Skipped completely
logger.debug { "Retrieved items: #{items.inspect}" }
```

This means that leaving debugging messages in a production codebase needn't be a concern in terms of production code performance.
