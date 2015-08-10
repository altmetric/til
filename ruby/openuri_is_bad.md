# OpenURI is bad

It's a common pattern for simple, one-off fetching of data, to use `OpenURI` -
which is a simple wrapper around `net/http`/`net/https`/`net/ftp`:

```ruby
require 'open-uri'

data = open('http://example.com').read
```

However, `OpenURI` works by patching `Kernel#open`, which is a dangerous method!

If the URI is user controllable (for example, passed from a front-end app) then
we've opened a remote code execution vulnerability – Ruby will execute any
string passed to `Kernel#open` that starts with the `|` pipe character.

```ruby
require 'open-uri'

params = { uri: '|cat /etc/passwd' }
open(params[:uri]).read
=> "Super-secret information!"
```

You should avoid using `OpenURI` where there is any possibility of user content
making its way into the call to `#open`.

See [Egor Homakov's blog post](http://sakurity.com/blog/2015/02/28/openuri.html) for more information. 
