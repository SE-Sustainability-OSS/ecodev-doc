# Cloud Transferors

We provide a helper method to transfer S3 data to an azure blob. 

!!! warning
    this method should only be used for < 1 Tb content, as it is quite slow. Look at <a href=https://learn.microsoft.com/en-us/azure/storage/common/storage-use-azcopy-v10" target="_blank">azcopy</a> for larger transfers.

```python
def transfer_s3_to_blob(folders: list[Path],
                        index_folder: Path,
                        bucket: str = BUCKET,
                        container: str = CONTAINER
                        ) -> None:
```

Attributes are:

- `folders`: the list pathlib <a href=https://docs.python.org/3/library/pathlib.html class="external-link" target="_blank">Path</a>
specifying where the data to transfer from the <a href="https://aws.amazon.com/s3/?nc1=h_ls" target="_blank">s3 bucket</a>
to the <a href="https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction#blob-storage-resources" target="_blank">blob container</a> are located.
Remember to read the [forge key](../generic/forge_key.md) page to learn how to properly... Well, forge a key 😊.
- `index_folder`: where to store index files (in order not to re-transfer already successfully transferred files, and to store files for which transfer failed)
- `bucket` (optional): the S3 bucket name from which to transfer. By default, the `s3_bucket_name` environment variable is used.
- `container` (optional): the AWS container name to  which to transfer. By default, the `container` environment variable is used.