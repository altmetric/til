# Converting a list into pairs of elements

The `zip` function is used to return a list of tuples from each of the iterables passed to it

```python
  >>> list_one = ['a', 'b']
  >>> list_two = ['c', 'd']
  >>> zip(list_one, list_two)
  [('a', 'c'), ('b', 'd')]
```

If we were to use the `zip` function on the same list with itself we would get the following:

```python
  >>> list = ['a', 'b', 'c']
  >>> zip(list, list)
  [(a, a), (b, b), (c, c)]
```

But in order to return a list of pairs instead of a list of the same pairs we can use the `iter` function.
The `iter` returns an Iterator from an object

```python
  >>> iter([1, 2, 3])
  <listiterator object at 0x10f581a10>
```

When used with `zip` we can iterate for every 2 elements, taking advantage of the mutating iterator object which returns the next value, allowing us to interleave a list with the following result:

```python
  >>> list = ['a', 'b', 'c', 'd', 'e', 'f']
  >>> iterator = iter(list)
  >>> zip(iterator, iterator)
  [('a', 'b'), ('c', 'd'), ('e', 'f')]
```


