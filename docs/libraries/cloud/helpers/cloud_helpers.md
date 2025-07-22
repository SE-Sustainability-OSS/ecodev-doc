# Cloud helpers

We provide several helper methods to interact with cloud storage (other than load/save)

## `cloud_copy_file`

Method to copy content from disk/cloud (depending on `dist_origin` value) to cloud.

```python
def cloud_copy_file(origin: Path,
                    dest: Path,
                    dist_origin: bool = False,
                    cloud: Cloud = CLOUD,
                    location: str | None = None
                    ) -> None:
```

Arguments are:

- `origin`: a pathlib <a href=https://docs.python.org/3/library/pathlib.html class="external-link" target="_blank">Path</a>
specifying where the data is located in the disk (if `dist_origin` is `False`) / <a href="https://aws.amazon.com/s3/?nc1=h_ls" target="_blank">s3 bucket</a>
/ <a href="https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction#blob-storage-resources" target="_blank">blob container</a>.
Remember to read the [forge key](../generic/forge_key.md) page to learn how to properly... Well, forge a key 😊.
- `dest`: the (pathlib path specified) cloud place where to copy the file
- `dist_origin` (optional): whether the file is already on the cloud storage (`True`) or on disk (`False`). 
Disk location by default
- `cloud` (optional): the cloud provider used to connect to a distance storage. Read the [installation guide](../installation.md) in order 
to learn how to connect to your S3 like/blob cloud provider, and also consult [cloud details](../generic/cloud.md).
By default, the environment variable `cloud_provider` is used.
- `location` (optional): the s3 bucket/blob container on which to connect. By default the `s3_bucket_name` environment variable is used if `cloud=Cloud.AWS`, and 
`container` is used if `cloud=Cloud.Azure`.

## `cloud_exists`


Method checking if a file exists on the cloud storage

```python
def cloud_exists(file_path: Path, cloud: Cloud = CLOUD) -> bool:
```

Arguments are:

- `file_path`: a pathlib <a href=https://docs.python.org/3/library/pathlib.html class="external-link" target="_blank">Path</a>
specifying where the data is located in the <a href="https://aws.amazon.com/s3/?nc1=h_ls" target="_blank">s3 bucket</a>
/ <a href="https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction#blob-storage-resources" target="_blank">blob container</a>.
Remember to read the [forge key](../generic/forge_key.md) page to learn how to properly... Well, forge a key 😊.
- `cloud` (optional): the cloud provider used to connect to a distance storage. Read the [installation guide](../installation.md) in order 
to learn how to connect to your S3 like/blob cloud provider, and also consult [cloud details](../generic/cloud.md).
By default, the environment variable `cloud_provider` is used.

## `cloud_is_dir`

Method checking if the passed `file_path` is a folder or not (look at file extension)

```python
def cloud_is_dir(file_path: Path) -> bool:
```


Arguments are:

- `file_path`: a pathlib <a href=https://docs.python.org/3/library/pathlib.html class="external-link" target="_blank">Path</a>
specifying where the data is located in the <a href="https://aws.amazon.com/s3/?nc1=h_ls" target="_blank">s3 bucket</a>
/ <a href="https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction#blob-storage-resources" target="_blank">blob container</a>.
Remember to read the [forge key](../generic/forge_key.md) page to learn how to properly... Well, forge a key 😊.


## `cloud_iterdir`

Method creating an `Iterator` of files  directly underneath (as the pathlib disk version) in the provided `file_path`

```python
def cloud_iterdir(file_path: Path, cloud: Cloud = CLOUD) -> Iterator[Path]:
```


Arguments are:

- `file_path`: a pathlib <a href=https://docs.python.org/3/library/pathlib.html class="external-link" target="_blank">Path</a>
specifying where the data is located in the <a href="https://aws.amazon.com/s3/?nc1=h_ls" target="_blank">s3 bucket</a>
/ <a href="https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction#blob-storage-resources" target="_blank">blob container</a>.
Remember to read the [forge key](../generic/forge_key.md) page to learn how to properly... Well, forge a key 😊.
- `cloud` (optional): the cloud provider used to connect to a distance storage. Read the [installation guide](../installation.md) in order 
to learn how to connect to your S3 like/blob cloud provider, and also consult [cloud details](../generic/cloud.md).
By default, the environment variable `cloud_provider` is used.

## `cloud_move_file`

Method to move content from disk/cloud (depending on `dist_origin` value) to cloud.

```python
def cloud_move_file(origin: Path,
                    dest: Path,
                    dist_origin: bool = False,
                    delete_file: bool = True,
                    cloud: Cloud = CLOUD
                    ) -> None:
```

Arguments are:

- `origin`: a pathlib <a href=https://docs.python.org/3/library/pathlib.html class="external-link" target="_blank">Path</a>
specifying where the data is located in the disk (if `dist_origin` is `False`) / <a href="https://aws.amazon.com/s3/?nc1=h_ls" target="_blank">s3 bucket</a>
/ <a href="https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction#blob-storage-resources" target="_blank">blob container</a>.
Remember to read the [forge key](../generic/forge_key.md) page to learn how to properly... Well, forge a key 😊.
- `dest`: the (pathlib path specified) cloud place where to move the file
- `dist_origin` (optional): whether the file is already on the cloud storage (`True`) or on disk (`False`). 
Disk location by default
- `cloud` (optional): the cloud provider used to connect to a distance storage. Read the [installation guide](../installation.md) in order 
to learn how to connect to your S3 like/blob cloud provider, and also consult [cloud details](../generic/cloud.md).
By default, the environment variable `cloud_provider` is used.
- `location` (optional): the s3 bucket/blob container on which to connect. By default the `s3_bucket_name` environment variable is used if `cloud=Cloud.AWS`, and 
`container` is used if `cloud=Cloud.Azure`.

## `cloud_move_folder`

Method to move/copy (depending on `delete_file` value) content from disk/cloud 
(depending on `dist_origin` value) to cloud.

```python
def cloud_move_folder(origin: Path,
                      dest: Path,
                      dist_origin: bool = False,
                      delete_file: bool = True,
                      cloud: Cloud = CLOUD
                      ) -> None:
```

Arguments are:

- `origin`: a pathlib <a href=https://docs.python.org/3/library/pathlib.html class="external-link" target="_blank">Path</a>
specifying where the folder is located in the disk (if `dist_origin` is `False`) / <a href="https://aws.amazon.com/s3/?nc1=h_ls" target="_blank">s3 bucket</a>
/ <a href="https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction#blob-storage-resources" target="_blank">blob container</a>.
Remember to read the [forge key](../generic/forge_key.md) page to learn how to properly... Well, forge a key 😊.
- `dest`: the (pathlib path specified) cloud place where to move the folder
- `dist_origin` (optional): whether the file is already on the cloud storage (`True`) or on disk (`False`). 
Disk location by default
- `cloud` (optional): the cloud provider used to connect to a distance storage. Read the [installation guide](../installation.md) in order 
to learn how to connect to your S3 like/blob cloud provider, and also consult [cloud details](../generic/cloud.md).
By default, the environment variable `cloud_provider` is used.
- `location` (optional): the s3 bucket/blob container on which to connect. By default the `s3_bucket_name` environment variable is used if `cloud=Cloud.AWS`, and 
`container` is used if `cloud=Cloud.Azure`.

## `cloud_rglob`

Method creating an `Iterator` of **all** (as would do the pathlib disk version) files underneath (even recursively) the provided `file_path`

```python
def cloud_rglob(file_path: Path,
                pattern: str | None = None,
                cloud: Cloud = CLOUD
                ) -> Iterator[Path]:
```

Arguments are:

- `file_path`: a pathlib <a href=https://docs.python.org/3/library/pathlib.html class="external-link" target="_blank">Path</a>
specifying where the data is located in the <a href="https://aws.amazon.com/s3/?nc1=h_ls" target="_blank">s3 bucket</a>
/ <a href="https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction#blob-storage-resources" target="_blank">blob container</a>.
Remember to read the [forge key](../generic/forge_key.md) page to learn how to properly... Well, forge a key 😊.
- `pattern`: a matching pattern (not equivalent to regex: rglob pathlib inspired. Read <a href=https://docs.python.org/3/library/fnmatch.html#module-fnmatch class="external-link" target="_blank">this</a> for more information).
Only keys matching this pattern will be returned.
- `cloud` (optional): the cloud provider used to connect to a distance storage. Read the [installation guide](../installation.md) in order 
to learn how to connect to your S3 like/blob cloud provider, and also consult [cloud details](../generic/cloud.md).
By default, the environment variable `cloud_provider` is used.


## `delete_cloud_content`

Method deleting the file `file_path` in the cloud storage


```python
def delete_cloud_content(file_path: Path,  cloud: Cloud = CLOUD) -> None:
```

- `file_path`: a pathlib <a href=https://docs.python.org/3/library/pathlib.html class="external-link" target="_blank">Path</a>
specifying where the data is located in the <a href="https://aws.amazon.com/s3/?nc1=h_ls" target="_blank">s3 bucket</a>
/ <a href="https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction#blob-storage-resources" target="_blank">blob container</a>.
Remember to read the [forge key](../generic/forge_key.md) page to learn how to properly... Well, forge a key 😊.
- `cloud` (optional): the cloud provider used to connect to a distance storage. Read the [installation guide](../installation.md) in order 
to learn how to connect to your S3 like/blob cloud provider, and also consult [cloud details](../generic/cloud.md).
By default, the environment variable `cloud_provider` is used.

## `download_cloud_object`

Method downloading the file `file_path` in the cloud storage

```python
def download_cloud_object(file_path: Path, local_path: Path, cloud: Cloud = CLOUD) -> None:
```

- `file_path`: a pathlib <a href=https://docs.python.org/3/library/pathlib.html class="external-link" target="_blank">Path</a>
specifying where the data is located in the <a href="https://aws.amazon.com/s3/?nc1=h_ls" target="_blank">s3 bucket</a>
/ <a href="https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction#blob-storage-resources" target="_blank">blob container</a>.
Remember to read the [forge key](../generic/forge_key.md) page to learn how to properly... Well, forge a key 😊.
- `local_path`: the (pathlib path specified) local disk place where to store the downloaded file.
- `cloud` (optional): the cloud provider used to connect to a distance storage. Read the [installation guide](../installation.md) in order 
to learn how to connect to your S3 like/blob cloud provider, and also consult [cloud details](../generic/cloud.md).
By default, the environment variable `cloud_provider` is used.

## `get_cloud_url`

In the case of:

- S3, one can generate <a href="https://boto3.amazonaws.com/v1/documentation/api/latest/guide/s3-presigned-urls.htmlX" target="_blank">pre-signed urls</a>
- blob, one can generate  <a href="https://learn.microsoft.com/en-us/azure/storage/blobs/sas-service-create-python" target="_blank">sas tokens</a>

In both cases, the aim is to give access to a person not having cloud storage credential to a specific file,
and for a limited duration. It proves handy to let a client download a given voluminous file for instance, where 
you obviously do not want to give the client access to the full cloud storage.

```python
def get_cloud_url(file_path: Path, timeout: int = 3600, cloud: Cloud = CLOUD) -> str | None:
```

- `file_path`: a pathlib <a href=https://docs.python.org/3/library/pathlib.html class="external-link" target="_blank">Path</a>
specifying where the data is located in the <a href="https://aws.amazon.com/s3/?nc1=h_ls" target="_blank">s3 bucket</a>
/ <a href="https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction#blob-storage-resources" target="_blank">blob container</a>.
Remember to read the [forge key](../generic/forge_key.md) page to learn how to properly... Well, forge a key 😊.
- `timeout` (optional): How long the url is valid for downloading data located at `file_path` (specified in seconds, `3600` by default)
- `cloud` (optional): the cloud provider used to connect to a distance storage. Read the [installation guide](../installation.md) in order 
to learn how to connect to your S3 like/blob cloud provider, and also consult [cloud details](../generic/cloud.md).
By default, the environment variable `cloud_provider` is used.