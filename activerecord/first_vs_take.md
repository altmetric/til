# Using `.take` can be faster than `.first`

When selecting records from an ActiveRecord collection, we often use `.first` to grab a single item, or `.first!` if we want the failure to find an item to raise an error rather than return `nil`.

There are occasions where `.first` is used for situations where there will only ever be zero or one records in the collection, though – for example, when querying a field that has a unique index.

```ruby
Widget.where(serial_number: '123ABCD9').first
```

In such instances, `take` will generally be faster than `first`. That's because `first` will always ensure that the ActiveRecord query has an explicit order – if none has been defined, an order of the table's primary key will be added automatically. In contrast, `take` will look at the first records available, and if no order has been specified it will select records in whatever order the underlying database decides.

If we have a unique index, though, the order is irrelevant, so can be ignored, so the additional `order` clause added by `first` is redundant (and may even prevent the underlying DB from selecting the best index for the job).

## Using `find_by` instead

But where we have a defining set of criteria to which we only ever expect a maximum of one matching result, and where we are applying `first` or `take` at the same time as a `where` clause, `find_by` will probably be faster than either.

Under the hood, `find_by` is implemented as `where(...).take`, so for a single query either implementation should be as performant as the other. However, Rails 4.2 and above implements additional caching within `find_by` to speed up repeated calls to the same table (see [Aaron Patterson's blog post on the `AdequateRecord Pro` improvements to ActiveRecord][adequate_record]). Thus, a comparison of `where.first`, `where.take` and `find_by` reveals some large speed improvements when repeated calls to `find_by` come into play:

```ruby
Benchmark.ips do |x|
  x.report('find_by! :name') { Person.find_by!(name: name) }
  x.report('find_by :name') { Person.find_by(name: name) }
  x.report('find_by_name') { Person.find_by_name(name) }
  x.report('where.take!') { Person.where(name: name).take! }
  x.report('where.take') { Person.where(name: name).take }
  x.report('where.first!') { Person.where(name: name).first! }
  x.report('where.first') { Person.where(name: name).first }
  x.compare!
end
```

```text
Calculating -------------------------------------
      find_by! :name   811.000  i/100ms
       find_by :name     1.322k i/100ms
        find_by_name     1.303k i/100ms
         where.take!   497.000  i/100ms
          where.take   493.000  i/100ms
        where.first!   433.000  i/100ms
         where.first   449.000  i/100ms
-------------------------------------------------
      find_by! :name     13.233k (±12.2%) i/s -     65.691k
       find_by :name     13.749k (±11.2%) i/s -     68.744k
        find_by_name     13.525k (±12.3%) i/s -     66.453k
         where.take!      5.032k (±12.1%) i/s -     24.850k
          where.take      5.080k (±15.7%) i/s -     24.157k
        where.first!      4.665k (± 6.2%) i/s -     23.382k
         where.first      4.623k (± 7.5%) i/s -     23.348k

Comparison:
       find_by :name:    13749.3 i/s
        find_by_name:    13525.5 i/s - 1.02x slower
      find_by! :name:    13233.4 i/s - 1.04x slower
          where.take:     5080.5 i/s - 2.71x slower
         where.take!:     5032.2 i/s - 2.73x slower
        where.first!:     4665.0 i/s - 2.95x slower
         where.first:     4623.5 i/s - 2.97x slower
```

[Full gist at https://gist.github.com/scottmatthewman/0e7b73c298fd4bc556ba]



[adequate_record]: http://tenderlovemaking.com/2014/02/19/adequaterecord-pro-like-activerecord.html
