# Read write helpers

Simple helpers to read/write files/folders.

## `load_json_file`

Reads a `json` file sitting at the passed pathlib <a href=https://docs.python.org/3/library/pathlib.html class="external-link" target="_blank">Path</a> (by the way, we advise to get rid of all `os` code when you can. 
For dealing with files/folders, pathlib is just way simpler and user friendly to use.).

## `write_json_file`

write the passed `data` (either a `dict` or a `list`) onto disk at the passed pathlib path.

## `make_dir`

safe (recursive) directory creation given a pathlib path (you can just create in one shot `b` `c` and `d` in `a` by passing `a/b/c/d`)


Simple example 
```python 
# low-level read-write methods
data = {'toto': 23}
make_dir(TEST_DIR)
write_json_file(data, TEST_DIR / 'test.json')
loaded_data = load_json_file(TEST_DIR / 'test.json')
self.assertEqual(data, loaded_data)`
```