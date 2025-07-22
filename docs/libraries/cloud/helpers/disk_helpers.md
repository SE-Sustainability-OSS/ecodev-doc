# Disk helpers

Inspired by the [cloud helpers method](cloud_helpers.md), we provide their disk counterparts
where it make sense.

## `disk_copy`


Method to copy a disk file from one location to another.

```python
def disk_copy(origin: Path, dest: Path) -> None:
```

Attributes are:

- `origin`: a pathlib <a href=https://docs.python.org/3/library/pathlib.html class="external-link" target="_blank">Path</a>
specifying where the data is located on disk. Remember to read the [forge key](../generic/forge_key.md) page to learn how to properly... 
Well, forge a key 😊.
- `dest`: the (pathlib path specified) disk place where to copy the file

## `disk_is_dir`

Method checking if the passed `file_path` is a folder or not

```python
def disk_is_dir(file_path: Path) -> bool:
```

Attributes are:

- `file_path`: a pathlib <a href=https://docs.python.org/3/library/pathlib.html class="external-link" target="_blank">Path</a>
specifying where the data is located on disk. Remember to read the [forge key](../generic/forge_key.md) page to learn how to properly... 
Well, forge a key 😊.

## `disk_rglob`

Method creating an `Iterator` of **all** files underneath (even recursively) the provided `file_path`


```python
def disk_rglob(file_path: Path, pattern: str | None = None) -> Iterator[Path]:
```

Attributes are:

- `file_path`: a pathlib <a href=https://docs.python.org/3/library/pathlib.html class="external-link" target="_blank">Path</a>
specifying where the data is located on disk. Remember to read the [forge key](../generic/forge_key.md) page to learn how to properly... 
Well, forge a key 😊.
- `pattern`: a matching pattern (not equivalent to regex: rglob pathlib one. Read <a href=https://docs.python.org/3/library/fnmatch.html#module-fnmatch class="external-link" target="_blank">this</a> for more information).
Only keys matching this pattern will be returned.

## `disk_iterdir`

Method creating an `Iterator` of files directly underneath (as the pathlib disk version) in the provided `file_path`
`
`
```python
def disk_iterdir(file_path: Path) -> Iterator[Path]:
```

Attributes are:

- `file_path`: a pathlib <a href=https://docs.python.org/3/library/pathlib.html class="external-link" target="_blank">Path</a>
specifying where the data is located on disk. Remember to read the [forge key](../generic/forge_key.md) page to learn how to properly... 
Well, forge a key 😊.

## `disk_exists`

Method to check if a disk location exists.

```python
def disk_exists(file_path: Path) -> bool:
```

Attributes are:

- `file_path`: a pathlib <a href=https://docs.python.org/3/library/pathlib.html class="external-link" target="_blank">Path</a>
specifying where the data is located on disk. Remember to read the [forge key](../generic/forge_key.md) page to learn how to properly... 
Well, forge a key 😊.

## `disk_move`

Method to move a disk file from one location to another.

```python
def disk_move(origin: Path, dest: Path) -> None:
```

Attributes are:

- `origin`: a pathlib <a href=https://docs.python.org/3/library/pathlib.html class="external-link" target="_blank">Path</a>
specifying where the data is located on disk. Remember to read the [forge key](../generic/forge_key.md) page to learn how to properly... 
Well, forge a key 😊.
- `dest`: the (pathlib path specified) disk place where to move the file
