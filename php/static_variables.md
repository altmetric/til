# Static variables

```php
<?php
function test()
{
    static $a = 0;
    echo $a;
    $a++;
}
```

It may appear that the code above will *always* echo `0` (as it is set to 0 in
the first line of the function) but it will actually echo a new number each
time it is called:

```php
<?php
test();
// 0

test();
// 1

test();
// 2
```

This is because the `static` keyword defines a [static variable][0] which will
only ever be initialised once. Note that static variables can only be assigned
to raw values and not the result of an expression.

[0]: http://php.net/manual/en/language.variables.scope.php#language.variables.scope.static
