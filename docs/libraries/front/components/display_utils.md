# Display utils


Helper methods to help rendering numerical information


## Number formatting

Returns numbers in a more readable format (scaled with suffix).

```python
def number_formatting(number: float, decimals: int = 0) -> str:
    """
    Returns numbers in a more readable format (scaled with suffix).
    """
    units = ['', 'K', 'M', 'G', 'T', 'P']
    k = 1000.0
    magnitude = int(floor(log(number, k)))
    return f'%.{decimals}f %s' % (number / k ** magnitude, units[magnitude])
```

## Get magnitude

Returns the magnitude to divide by and the unit

```python
def get_magnitude(number: int | float) -> tuple[object, str]:
    """
    Returns the metric to divide by and the unit
    """
    units = ['', 'K', 'M', 'B', 'T', 'P']
    denominator = [1, THOUSAND, MILLION, BILLION, TRILLION, 'P']

    k = 1000.0
    magnitude = int(floor(log(number, k)))

    return denominator[magnitude], units[magnitude]
```