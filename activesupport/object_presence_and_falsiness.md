# `Object#presence` and falsiness

ActiveSupport adds a [`presence` method to
`Object`](http://api.rubyonrails.org/classes/Object.html#method-i-presence)
which allows you to treat empty objects such as strings and hashes as if they
were false-y, e.g.

```ruby
region = params[:state].presence || params[:country].presence || 'US'
```

In the above example, if `params[:state]` is an empty string, it will fall
through to `params[:country]` and so on. Typically in Ruby this would not work
as the only things that are false-y are `false` and `nil`.

We can take advantage of this to make our own objects false-y as well:

```ruby
class Response
  # ...
  def empty?
    status == 404
  end
end

response = a_response.presence || b_response.presence
# where a_response and b_response are instances of Response
```

This works because `Object#presence` uses
[`Object#present?`](http://api.rubyonrails.org/classes/Object.html#method-i-present-3F)
which uses
[`Object#blank?`](http://api.rubyonrails.org/classes/Object.html#method-i-blank-3F)
which will use the method `empty?` on an object if it exists.
