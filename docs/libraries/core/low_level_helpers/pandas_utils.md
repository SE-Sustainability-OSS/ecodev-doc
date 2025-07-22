# Pandas helpers

Simple helper methods related to pandas

## `pd_equals`

If you ever pull your hair out with `None` values converted to `np.nan` when stored on disk by pandas `to_csv`, causing issues when comparing two dataframes, 
then `pd_equals` is for you. It is meant to compare a stored csv from a one in memory, and does so by writing and reading the latter to have the same funky conversion for both.

Of course the real solution would be to use the `converters` option from `read_csv` (look the <a href=https://pandas.pydata.org/docs/reference/api/pandas.read_csv.html class="external-link" target="_blank">official documentation</a>. ), but it can be quite tedious and frankly overkill for tests.


```python 
# test pd_equals method (subtlety of None np.Nan that imposes to write on/read from disk)
df = pd.DataFrame.from_dict({'a': [1], 'b': None})
self.assertTrue(pd_equals(df, TEST_FILE) is None)
```


## `jsonify_series`

As for [`pd_equals`](#`pd_equals`), converting a pandas serie to something that is json acceptable can be useful. 

Simple example 

```python 
# test jsonify_series (subtlety of None/np.Nan)
df = pd.DataFrame.from_dict({'a': [1, 2], 'b': [np.nan, 2]})
self.assertDictEqual(jsonify_series(df['b']), {0: None, 1: 2.0})
```
## `get_excelfile`

Takes an input file, inserted through a front interface such as `FastApi` or a `dcc.Upload`
component in **Dash** and returns a `pd.ExcelFile` object

Simple example 

```python 
# test get_excelfile 
xl = get_excelfile(load_json_file(TEST_FILE.parent / 'str_parsing.json')['data'])
```

## `safe_drop_columns`

Checks if a `list` of columns are in a `pd.DataFrame` and drops only the subset which has been 
found, preventing any errors

Simple example 

```python 
# test safe_drop_columns 
df = pd.DataFrame({'a': [1, 2, 3], 'b': [None, None, None]}
pd.testing.assert_frame_equal(safe_drop_columns(df, ['b']), df.drop(columns='b')
pd.testing.assert_frame_equal(safe_drop_columns(df, ['c']), df)
```

## `is_null`

If you ever struggled with null values, that can take multiple forms `None`, `np.nan`, `NaN`,... 
this function is made for you as it will try different representation of a null value and return 
a `bool`

Simple example 

```python 
# test is_null 
self.assertTrue(is_null(None))
self.assertTrue(is_null(np.nan))
self.assertFalse(is_null(15))
self.assertFalse(is_null('test'))
```

## `get_value`

If you want to get a value of a `pd.Series` based on a column name when iterating over a 
`pd.DataFrame`, but you don't know if the column is actually in the `pd.DataFrame` beforehand, 
you can use `get_value` to get the value if the column exist (and apply a transformation function or 
transform it into an enum) else `None`. An equivalent to `dict.get()` for `pandas`.

Simple example 

```python 
# test get_value 
df = pd.DataFrame({'a': [1, 2, 3], 'b': [None, None, None]})

self.assertEqual(get_value('a', intify, df.iloc[0]), 1)
self.assertEqual(get_value('c', intify, df.iloc[0]), None)
```