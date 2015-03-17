# Benchmarking

If you want to compare two or blocks of code in ruby, there's a handy gem for it called benchkmark-ips (https://github.com/evanphx/benchmark-ips).

Here's a simple example comparing the performance of `tap` against using local variables.

```ruby
require 'benchmark/ips'

class Monkey
  attr_accessor :name

  def speak
    name
  end
end

Benchmark.ips do |x|
  x.report('tap') do
    Monkey.new.tap do |monkey|
      monkey.name = "Bob"
      monkey.speak
    end
  end

  x.report('local variable') do
    monkey = Monkey.new
    monkey.name = "Bob"
    monkey.speak

    monkey
  end

  x.compare!
end
```

And here is the sample result

```console
Calculating -------------------------------------
                 tap    92.386k i/100ms
      local variable    99.640k i/100ms
-------------------------------------------------
                 tap      2.250M (± 4.9%) i/s -     11.271M
      local variable      2.751M (± 5.1%) i/s -     13.750M

Comparison:
      local variable:  2751220.7 i/s
                 tap:  2249893.3 i/s - 1.22x slower
```

Thanks to [@mudge](http://github.com/mudge).
