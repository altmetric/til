# Generating a list of dates from a date range

To generate a list of dates between two set dates:

1) First you will need `date` and `timedelta` from the `datetime` module
```python
  >>> from datetime import date, timedelta
```

2) Generate a list of all possible date units between two set dates, we have chosen the list to be in days. Note that `range` is not inclusive, so we have to increment the last value if we wish for it to be included in our list
```python
  >>> end_date = date(2017, 2, 10)
  >>> start_date = date(2017, 2, 1)
  >>> range((end_date - start_date).days + 1)
  [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

3) Generate a list of all possible dates using the `timedelta` object and the resulting date units list

`timedelta` returns an object representing the difference between two dates or times
```python
  >>> timedelta(days=1)
  datetime.timedelta(1)
```

This object supports multiple operations:

```python
  >>> delta = timedelta(days=1)
  datetime.timedelta(1)
  >>> date(2000, 1, 1) + delta
  datetime.date(2000, 1, 2)
  >>> date(2000, 1, 1) - delta
  datetime.date(1999, 12, 31)
```

To generate a list of all the dates between two dates ex. `01-02-2017` to `04-02-2017`:
```python
  >>> start_date = date(2017, 2, 1)
  >>> end_date = date(2017, 2, 4)
  >>> [start_date + timedelta(days=day) for day in range((end_date - start_date).days + 1)]
  [datetime.date(2017, 2, 1), datetime.date(2017, 2, 2), datetime.date(2017, 2, 3), datetime.date(2017, 2, 4)]
```


