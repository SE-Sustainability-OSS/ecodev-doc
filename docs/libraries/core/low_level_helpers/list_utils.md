# List helpers

We found ourselves at <a href=https://eco-act.com/ class="external-link" target="_blank">EcoAct</a> reusing again and again the following helpers. 
They are heavily inspired from C# <a href=https://learn.microsoft.com/fr-fr/dotnet/csharp/linq/ class="external-link" target="_blank">LINQ</a> spirit. 

## `first_or_default`

Take the first element of a list that satisfies a specific condition.

Returns None if no element matches the specified condition.

The <a href=https://docs.python.org/3/library/unittest.html class="external-link" target="_blank">Unit test</a> should be self explanatory 

```python 
 data = [1, 2, 3, 1, 2, 3]
self.assertEqual(first_or_default(data, lambda x: x > 2), 3)
self.assertEqual(first_or_default(data), 1)
self.assertEqual(first_or_default(None), None)
```

## `first_transformed_or_default`

Take the first element of a list that satisfies a specific condition and apply a transformation to this element. 

Returns None if no element matches the specified condition.

The <a href=https://docs.python.org/3/library/unittest.html class="external-link" target="_blank">Unit test</a> should be self explanatory 

```python 
data = [1, 2, 3, 1, 1, 2, 3]
self.assertEqual(first_transformed_or_default(data, lambda x: x * 3 if x > 2 else None), 9)
```

## `group_by_value`

Sometimes a code snippet is worth a thousands words

```python 
data = [1, 2, 3, 1, 1, 2, 3]
self.assertEqual(group_by_value(data), {1: [0, 3, 4], 2: [1, 5], 3: [2, 6]})
```

`group_by_value` thus eats a sequence and spits a dict where 

- keys are all unique sequence element values
- values are indexes where the corresponding key sits in the sequence

## `lselect`

The builtin python <a href=https://docs.python.org/3/library/functions.html#filter class="external-link" target="_blank">filter</a> method returns an 
<a href=https://en.wikipedia.org/wiki/Iterator class="external-link" target="_blank">Iterator</a> and not a list.

This is convenient if you plan to directly `foreach` on the `filter` result only once, but not so convenient if you need to do so several times, or access a particular element. 
For that we use `lselect`.

```python 
data = [1, 2, 3, 1, 1, 2, 3]
self.assertEqual(lselect(data, lambda x: x > 2), [3, 3])
self.assertEqual(lselectfirst(data, lambda x: x > 2), 3)
```

## `lselectfirst` (deprecated)

Should be deprecated, use [`first_or_default`](#first_or_default) in its stead


## `list_tuple_to_dict`

 Transforms the result of a sqlmodel query into a list of Dict