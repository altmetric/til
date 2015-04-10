# Exception cause chains

When handling specific exceptions, we sometimes wish to re-raise the exception, or to raise a different exception to the one we've just caught. A real world example of this is within ActiveRecord's database adapters: exceptions thrown by a specific database (e.g., Postgres) are re-raised as their database-agnostic ActiveRecord equivalents. So a `PG::UniqueViolation`, which occurs when saving a record would violate a table's unique index, is captured by ActiveRecord's Postgres adapter and re-raised as `ActiveRecord::RecordNotUnique`.

When this happens, Ruby 2.1+ automatically wraps the original exception as a property of the new one. The new exception's `#cause` property will retain a record of the original.

From the [Ruby docs](http://ruby-doc.org/core-2.2.1/Exception.html#method-i-cause):

> ### cause → an_exception or nil
> Returns the previous exception (`$!`) at the time this exception was raised. This is useful for wrapping exceptions and retaining the original exception information.

Bugsnag takes advantage of this exception trail to detail a full backtrace of each exception in the chain. Errors are then listed according to the very first exception in the chain.

It's worth noting, then, that **the error in Bugsnag's headline may not be the exception that your own code is experiencing**. In the example above, the `PG::UniqueViolation` will appear in Bugsnag's headline, but the application code will only ever experience the `ActiveRecord::RecordNotUnique` one.

## Further reading

* [Bugsnag blog: Exception#cause in Ruby 2.1](https://bugsnag.com/blog/ruby-2-1-exception-causes)
* [Avdi Grimm: Exception causes in Ruby 2.1](http://devblog.avdi.org/2013/12/25/exception-causes-in-ruby-2-1/)
