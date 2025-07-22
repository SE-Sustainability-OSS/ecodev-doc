# Disk Transferors

We provide a helper method to transfer disk data to an azure blob. 

```python
def transfer_disk_to_blob(folders: list[Path],
                          index_folder: Path,
                          container: str = CONTAINER
                          ) -> None:
```

- `folders`: the list pathlib <a href=https://docs.python.org/3/library/pathlib.html class="external-link" target="_blank">Path</a>
specifying where the data to transfer from disk
to the <a href="https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction#blob-storage-resources" target="_blank">blob container</a> are located.
Remember to read the [forge key](../generic/forge_key.md) page to learn how to properly... Well, forge a key 😊.
- `index_folder`: where to store index files (in order not to re-transfer already successfully transferred files, and to store files for which transfer failed)
- `container` (optional): the AWS container name to  which to transfer. By default, the `container` environment variable is used.