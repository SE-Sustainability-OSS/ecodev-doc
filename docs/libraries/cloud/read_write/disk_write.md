# Disk write



Inspired by the `save_cloud_data` [method](cloud_write.md), we provide a high level method to 
load data from disk.

It's interface reads

```python
def disk_save(file_path: Path, data: Any) -> None:
```


where: 

- `file_path`: a pathlib <a href=https://docs.python.org/3/library/pathlib.html class="external-link" target="_blank">Path</a>
specifying where the data is located on disk. Remember to read the [forge key](../generic/forge_key.md) page to learn how to properly... 
Well, forge a key 😊.
- `data`: the data to store on cloud. Read on to find the different types of data that can be saved.

As of 2024/05, this method can write the following file types (it relies on the file extension given in `file_type` to use the appropriate loader):

- csv
- xlsx
- npy
- npy.npz (<a href="https://numpy.org/doc/stable/reference/generated/numpy.savez_compressed.html" target="_blank">compressed numpy</a>)
- json
- <a href="https://en.wikipedia.org/wiki/LaTeX" target="_blank">tex</a>
- txt
- png
- zip
- shp

