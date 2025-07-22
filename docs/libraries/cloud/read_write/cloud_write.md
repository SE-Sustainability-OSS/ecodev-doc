# Cloud write



The second central method of `ecodev-cloud` is `save_cloud_data`, that allows one to save data to 
the cloud provider specified in the `cloud_provider` [environment variable](../generic/cloud.md#cloudconfiguration).

It's interface reads


```python
def save_cloud_data(file_path: Path,
                    data: DATA_TYPE,
                    cloud: Cloud = CLOUD,
                    location: str | None = None
                    ) -> None:
```

where: 

- `file_path`: a pathlib <a href=https://docs.python.org/3/library/pathlib.html class="external-link" target="_blank">Path</a> (by the way, we advise to get rid of all `os` code when you can. 
For dealing with files/folders, pathlib is just way simpler and user friendly to use.)
specifying where the data will be located in the  <a href="https://aws.amazon.com/s3/?nc1=h_ls" target="_blank">s3 bucket</a>
/ <a href="https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction#blob-storage-resources" target="_blank">blob container</a>.
Remember to read the [forge key](../generic/forge_key.md) page to learn how to properly... Well, forge a key 😊.
- `data`: the data to store on cloud. Read on to find the different types of data that can be saved.
- `cloud` (optional): the cloud provider used to connect to a distance storage. Read the [installation guide](../installation.md) in order 
to learn how to connect to your S3 like/blob cloud provider, and also consult [cloud details](../generic/cloud.md).
By default, the environment variable `cloud_provider` is used.
- `location` (optional): the s3 bucket/blob container on which to connect. By default the `s3_bucket_name` environment variable is used if `cloud=Cloud.AWS`, and 
`container` is used if `cloud=Cloud.Azure`.

As of 2024/05, this method can write the following file types (it relies on the file extension given in `file_type` to use the appropriate saver):

- csv
- xlsx
- npy
- npy.npz (<a href="https://numpy.org/doc/stable/reference/generated/numpy.savez_compressed.html" target="_blank">compressed numpy</a>)
- json
- <a href="https://en.wikipedia.org/wiki/LaTeX" target="_blank">tex</a>
- txt
- png
- zip
- shp (subtlety: the shapefile will be stored zipped in order to easily retrieve it. You can go inspect the  <a href="https://github.com/SE-Sustainability-OSS/ecodev-cloud/blob/main/ecodev_cloud/file_processing/shapely_processing.py" target="_blank">source code</a>, 
and the `save_shp` method to learn more)
