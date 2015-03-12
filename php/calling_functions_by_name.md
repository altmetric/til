# Calling functions by name

Before [anonymous functions](http://php.net/manual/en/functions.anonymous.php)
were added to PHP, the way to specify callback functions was by string name:

```php
function even($x)
{
    return $x % 2 === 0;
}

array_filter([1, 2, 3], 'even');
//=> [2]
```

However, it is also possible to use the string name of a function to call it
directly:

```php
$foo = 'even';
$foo(1);
//=> false
```

The callback syntax for instance methods is to use an array:

```php
class Monkey
{
    public function speak()
    {
        return 'ook!';
    }
}

$monkey = new Monkey;

array_map([$monkey, 'speak'], [1, 2, 3]);
//=> ['ook!', 'ook!', 'ook!']
```

More surprisingly, these arrays can also be used to call the function
directly:

```php
$bar = [$monkey, 'speak'];
$bar();
//=> 'ook!'
```

