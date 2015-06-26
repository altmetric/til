# `has_and_belongs_to_many` dirty tracking

When using [Mongoid's
`has_and_belongs_to_many`](http://mongoid.org/en/mongoid/docs/relations.html#has_and_belongs_to_many) relation macro with `inverse_of: nil` (where the primary keys of other documents are stored as an array on the parent) like so:

```ruby
class Customer
  include Mongoid::Document
  has_and_belongs_to_many :products, inverse_of: nil
end

class Product
  include Mongoid::Document
end
```

Resulting in records in the database like so:

```javascript
# The customer document
{
    "_id": ObjectId("4d3ed089fb60ab534684b7e9"),
    "product_ids": [ ObjectId("4d3ed089fb60ab534684b7f2") ]
}

# The product document
{
    "_id": ObjectId("4d3ed089fb60ab534684b7f2")
}
```

Be warned that setting the `product_ids` via the generated `products`
association method will *not* result in [tracked
changes](http://mongoid.org/en/mongoid/docs/documents.html#dirty) (even
though it does alter the value of the array):

```ruby
customer = Customer.new
customer.products << Product.first
customer.product_ids_changed?
#=> false
```

You can work around this by using the `product_ids` field directly:

```ruby
customer = Customer.new
customer.product_ids << Product.first.id
customer.product_ids_changed?
#=> true
```
