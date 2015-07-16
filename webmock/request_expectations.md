# Setting request expectations

We often use [WebMock](https://github.com/bblimke/webmock) to stub external
web services such as customer APIs during tests, using a [classicist testing
style](http://martinfowler.com/articles/mocksArentStubs.html#ClassicalAndMockistTesting)
to assert the behaviour of a system given a certain HTTP response.

However, there are some cases where setting expectations on specific web
requests happening at all makes it easier to test: e.g. when checking that
OAuth tokens get renewed after a timeout. In these situations, you can use
[WebMock's built-in
expectations](https://github.com/bblimke/webmock#setting-expectations-in-rspec-with-a_request)
on your stubs:

```ruby
RSpec.describe FooClient do
  describe '#get' do
    it 'renews tokens when necessary' do
      token = stub_request(:post, 'https://example.com/oauth/token')
        .to_return(body: some_json)

      client = described_class.new
      client.get('foo')
      allow(Time).to receive(:now).and_return(Time.now + 60)
      client.get('bar')

      expect(token).to have_been_requested.twice
    end
  end
end
```
