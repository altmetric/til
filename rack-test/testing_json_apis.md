# Testing JSON APIs

When using [Rack::Test][0] to test a Rack application (e.g. through
[Capybara][1] in an [RSpec feature spec][2]), requests take three arguments:

1. The URI;
2. Any parameters;
3. The environment.

In order to test JSON APIs that require an explicit content type, you must do
two things:

1. Pass the JSON payload as a string rather than a hash ([otherwise it will be
   converted to a URL-encoded string][3]);
2. Pass the content type by setting `CONTENT_TYPE` (*not* `Content-Type`) to
   `application/json` in the environment.

```ruby
post('/comment', '{ "body": "Foo" }', 'CONTENT_TYPE' => 'application/json')
```

[0]: https://github.com/brynary/rack-test
[1]: https://github.com/jnicklas/capybara
[2]: https://www.relishapp.com/rspec/rspec-rails/v/3-2/docs/feature-specs/feature-spec
[3]: https://github.com/brynary/rack-test/blob/master/lib/rack/test.rb#L218-L228
