# Cloud read



One of the two central methods of `ecodev-cloud` is `load_cloud_data` allows one to load data from 
the cloud provider specified in the `cloud_provider` [environment variable](../generic/cloud.md#cloudconfiguration).


It's interface reads

```python
def load_cloud_data(file_path: Path,
                    cloud: Cloud = CLOUD,
                    location: str | None = None
                    ) -> Any:
```

where: 

- `file_path`: a pathlib <a href=https://docs.python.org/3/library/pathlib.html class="external-link" target="_blank">Path</a> (by the way, we advise to get rid of all `os` code when you can. 
For dealing with files/folders, pathlib is just way simpler and user friendly to use.)
specifying where the data is located in the  <a href="https://aws.amazon.com/s3/?nc1=h_ls" target="_blank">s3 bucket</a>
/ <a href="https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction#blob-storage-resources" target="_blank">blob container</a>.
Remember to read the [forge key](../generic/forge_key.md) page to learn how to properly... Well, forge a key 😊.
- `cloud` (optional): the cloud provider used to connect to a distance storage. Read the [installation guide](../installation.md) in order 
to learn how to connect to your S3 like/blob cloud provider, and also consult [cloud details](../generic/cloud.md).
By default, the environment variable `cloud_provider` is used.
- `location` (optional): the s3 bucket/blob container on which to connect. By default the `s3_bucket_name` environment variable is used if `cloud=Cloud.AWS`, and 
`container` is used if `cloud=Cloud.Azure`.

As of 2024/05, this method can read the following file types (it relies on the file extension given in `file_type` to use the appropriate loader):

- csv
- xlsx
- npy
- npy.npz (<a href="https://numpy.org/doc/stable/reference/generated/numpy.savez_compressed.html" target="_blank">compressed numpy</a>)
- json
- <a href="https://en.wikipedia.org/wiki/NetCDF" target="_blank">netcdf</a> (a very useful format when dealing with climate data)
- <a href="https://en.wikipedia.org/wiki/LaTeX" target="_blank">tex</a>
- txt
- tif
- <a href="https://en.wikipedia.org/wiki/GeoPackage" target="_blank">gpkg</a>
- shp (subtlety: you have to store the shapefile zipped in order to easily retrieve it. You can go inspect the  <a href="https://github.com/SE-Sustainability-OSS/ecodev-cloud/blob/main/ecodev_cloud/file_processing/shapely_processing.py" target="_blank">source code</a>, 
and the `load_zipped_shp` method to learn more)

 



