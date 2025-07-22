# Ecodev-cloud

## Aim

Ecodev-cloud aims to ease the interaction with 

- <a href="https://aws.amazon.com/s3/" class="external-link"  target="_blank">AWS S3 buckets </a>
- <a href="https://azure.microsoft.com/en-us/products/storage/blobs" class="external-link"  target="_blank">Azure blob storage</a>

It does so by providing to high level helper methods: 

- `load_cloud_data`: load data from a cloud storage
- `save_cloud_data`: save data to a cloud storage

Other helper methods are also present to list the content of a cloud storage, copy content from disk to cloud...

This is explained in details in this part of the ecodev documentation.

- Start by reading the [installation](installation.md) section to properly setup ecodev-cloud. 
- Then we advise you to read the [cloud](generic/cloud.md) section to learn on how we cover different storage protocols
- Finally, read [forge key](generic/forge_key.md) to learn how to interact with all methods of the libraries. 

You can then navigate freely between the other parts of the documentation.


## Why it exists

We desperately tried to find a correct library to do just this: read/write on cloud storages in a more or less 
cloud-agnostic manner, but found nothing of the sort. We then developed it, and hope it can be useful for others! 😊

If you do know a python library that can do what is presented here, please share the tips with us! 