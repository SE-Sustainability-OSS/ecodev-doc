# Custom equal helpers

Should be enriched with a generic equality in the future for pure domain model classes formed of elementary types (`str`, `int`, `float`...).

As of today it only contains one helper method. 

## `custom_equal`

Compare whether two elements are both None or both not None and equals (same type/same value)

Attributes are:

- `element_1`: the first element of the comparison
- `element_2`:  the second element of the comparison
- `element_type`:  the expected element type for both elements

Some examples

```python 
# test custom equal float equality
self.assertTrue(custom_equal(3.0, 2.99999999999999, float))
# test custom equal str equality
self.assertTrue(custom_equal('3.0', '3.0', str))
# test custom equal none equality
self.assertTrue(custom_equal(None, None, str))
# test custom equal different type inequality
self.assertFalse(custom_equal('3.0', 3.0, str))
```