# Installation

## Install the lib

The easiest way to install `ecodev-cloud` is with docker. We provide a 
<a href=https://github.com/SE-Sustainability-OSS/ecodev-cloud/blob/main/Dockerfile class="external-link" target="_blank">sample image</a>
to this mean. Just adds `ècodev-cloud` to your `requirements.txt`. 


Docker eases the pain of installing `ecodev-cloud`, most of the pain being caused by `Gdal`,
which is <a href=https://gdal.org/api/python_bindings.html#installation class="external-link" target="_blank">quite a pain to install</a>.

This part of the docker image 
```yaml 
ENV CPLUS_INCLUDE_PATH="/usr/include/gdal"
ENV C_INCLUDE_PATH="/usr/include/gdal"

# Install dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends python3.11 python3-pip python3-wheel libpython3.11-dev g++ libgdal-dev cdo python3-dev&& \
    apt-get -y autoremove && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
```
Is indeed there solely for `gdal`.

If you want to install `ecodev-cloud` without docker, we suggest to install on the linux (you're using linux, right? 😊) OS your using
```shell
sudo apt-get install -y --no-install-recommends python3-pip python3-wheel libpython3.11-dev g++ libgdal-dev python3-dev
```

## Setup the required environment variables

### AWS

In order to interact with AWS, you need to setup the following environment variables:

- `s3_access_key_id`: your... S3 access key 😊
- `s3_secret_access_key`: your... S3 secret access key 😊
- `s3_endpoint_url`: the dns of your S3
- `s3_region_name`: the name of the region in which your S3 sits.
- `s3_bucket_name`: the main bucket in your S3 with which you want to interact. Almost all the `ecodev-cloud` methods allow you to change that.
- `aws_use`: if you're using a S3 bucket and your code is (`aws_use=True`) in the AWS environment (EC2, Fargate...), then presumably the previous environment variables do not even need to be specified!

### Azure

In order to interact with Azure, you need to setup the following environment variables:

- `connection_string`: your azure blob connection string.
- `container`: the main container in your blob storage with which you want to interact. Almost all the `ecodev-cloud` methods allow you to change that.

### Testing locally

We use <a href=https://min.io/ class="external-link" target="_blank">minio</a> and <a href= https://github.com/Azure/Azurite class="external-link" target="_blank">azurite</a> in order to test locally (and in the CI! Go look at the <a href=https://github.com/SE-Sustainability-OSS/ecodev-cloud/blob/main/.github/workflows/code-quality.yml class="external-link" target="_blank">github workflow</a> if interested).

#### Minio

For Minio to work properly, we need to specify in addition:

- `MINIO_ROOT_USER`: minio root username
- `MINIO_ROOT_PASSWORD`: minio root password.

Change `s3_access_key_id` to match `MINIO_ROOT_USER` and `s3_secret_access_key` 
to match `MINIO_ROOT_USER`. `s3_endpoint_url` is the local address at which minio is available (go look at the provided <a href=https://github.com/SE-Sustainability-OSS/ecodev-cloud/blob/main/docker-compose.override.yml class="external-link" target="_blank"> docker compose override</a>)

#### Azurite

For Azurite, you need to override `connection_string` to what is specified <a href=https://github.com/Azure/Azurite?tab=readme-ov-file#connection-strings class="external-link" target="_blank">there</a>.


