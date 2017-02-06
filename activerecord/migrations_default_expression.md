# Using an expression as default value for a column with Active Record Migrations

In relational databases, it's a quite common task to put some value in a column that is the result of an expression at the moment of the insertion. It's very useful to get the next sequential number (IDs) or get the current timestamp among others. Using raw SQL we can achieve the latter doing something like:

```sql
CREATE TABLE timestamps (
    a_timestamp timestamp without time zone DEFAULT now() NOT NULL
);
```

But how do we do it using ActiveRecord's migrations?
If we try with:

```ruby
create_table :timestamps do |t|
  t.timestamp :a_timestamp, null: false, default: 'NOW()'
end
```

The `NOW()` will be evaluated at creation time, and will be translated into:

```sql
CREATE TABLE timestamps (
    a_timestamp timestamp without time zone DEFAULT '2017-02-03 13:48:43.796235' NOT NULL
);
```

Or whatever current timestamp was at that point.

## The solution

For Rails < 5, we have to manually `ALTER` the table in the migration:

```ruby
create_table :timestamps do |t|
  t.timestamp :a_timestamp, null: false
end
execute 'ALTER TABLE ONLY timestamps ALTER COLUMN a_timestamp SET DEFAULT NOW()'
```

But starting with Rails 5, we can achieve this within the migration itself, using a proc in the `default` attribute:

```ruby
create_table :timestamps do |t|
  t.timestamp :a_timestamp, null: false, default: -> { 'NOW()' }
end
```

Details of this change can be found in [its PR](https://github.com/rails/rails/pull/20005/files) in the Rails repository.

Note: I'm using PostgreSQL syntax in this examples, some SQL might differ for other databases.
