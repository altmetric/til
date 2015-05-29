Support for [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing) is a bit tricky to test with `RSpec`, because we don't have a real server (like [Webrick](https://github.com/nahi/webrick), [Unicorn](http://unicorn.bogomips.org/) or [Puma](https://github.com/puma/puma)) between the code and the client. This usually is not a problem but it does affect in this particular case, and the [rfc3875](https://tools.ietf.org/html/rfc3875#section-4.1.18) explains why. The server translate the custom `HTTP` request headers in this way:

* Convert to upper case.
* Replace `-` with `_`
* Prepend `HTTP_`.

Because we don't have the server to do it for us, we must do it manually. For example, the `Origin` becomes `HTTP_ORIGIN` and `Http-Access-Control-Request-Method` becomes `HTTP_ACCESS_CONTROL_REQUEST_METHOD`. Let's see some example tests:

####Sending the Origin header:

```ruby
RSpec.feature "the requests support CORS headers", type: :feature do
  scenario 'Returns the response CORS headers' do
    get '/people/1234', nil, 'HTTP_ORIGIN' => '*'

    expect(last_response.headers['Access-Control-Allow-Origin']).to eq('*')
  end
end
```
####Sending the preflight options method:
```ruby
RSpec.feature "the requests support CORS headers", type: :feature do
  scenario 'Send the CORS preflight OPTIONS request' do
    options '/', nil, 'HTTP_ORIGIN' => '*', 'HTTP_ACCESS_CONTROL_REQUEST_METHOD' => 'GET', 'HTTP_ACCESS_CONTROL_REQUEST_HEADERS' => 'test'

    expect(last_response.headers['Access-Control-Allow-Origin']).to eq('*')
    expect(last_response.headers['Access-Control-Allow-Methods']).to eq('GET, POST, PATCH, OPTIONS')
    expect(last_response.headers['Access-Control-Allow-Headers']).to eq('test')
    expect(last_response.headers).to have_key('Access-Control-Max-Age')
  end
end
```
Note: You must use integration tests to be able the test through `rack`. A controller test doesn't trigger `rack`, it's like an unit test for the controller.

[More info](http://jbilbo.com/blog/2015/05/19/testing-cors-with-rspec/)
