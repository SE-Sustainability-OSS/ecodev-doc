# Minio

<a href=https://min.io/ class="external-link" target="_blank">minio</a> is a self-hosted solution that allows one to 

- mimic the <a href=https://aws.amazon.com/s3/?nc1=h_ls class="external-link" target="_blank">AWS S3</a> API standard 
- renders all applications that store data thanks to it <a href=https://12factor.net/processes class="external-link" target="_blank">stateless</a> (no need to mount <a href=https://docs.docker.com/storage/volumes/ class="external-link" target="_blank">volumes</a>  anymore! 😊)

Very useful to test S3 like interaction in a local dev environment!

## Setting up the minio stack 

All relevant information can be found in the `docker-compose.minio.yml` file (<a href=https://github.com/SE-Sustainability-OSS/ecodev-infra/blob/main/docker-compose.minio.yml class="external-link" target="_blank">here</a>).

### Production mode

Remember that you need to setup traefik when in production before launching this stack. Go read [the traefik page](traefik.md) if not already done.


All relevant information can be found in the `docker-compose.minio.yml` file (<a href=https://github.com/SE-Sustainability-OSS/ecodev-infra/blob/main/docker-compose.minio.yml class="external-link" target="_blank">here</a> ).


To start the stack in production, you can make use of the `Makefile` provided in the repo (`make help`) to list available commands.

To use it you will need to **create one folder** in the folder containing `docker-compose.minio.yml`

- `s3_data`

you will need to create a `.minio.env` <a href=https://www.codementor.io/@parthibakumarmurugesan/what-is-env-how-to-set-up-and-run-a-env-file-in-node-1pnyxw9yxj  class="external-link" target="_blank">file</a> and setting

- `minio_url` entry to the DNS address you created in order to reach the minio interface (`minio_frontend`). 
- `admin_minio_url`: entry to the DNS address you will call to programatically access minio (`minio_backend`)
- `MINIO_ROOT_USER`: the username to connect to either `minio_frontend` of `minio_backend`.
- `MINIO_ROOT_PASSWORD`: password associated to `MINIO_ROOT_PASSWORD`


You can then safely launch 

```shell
make minio-launch
```

minio will be accessible at `minio_url`. You can create a <a href= https://min.io/docs/minio/container/administration/concepts.html class="external-link" target="_blank">`bucket`</a>
in 2/3 (very user-friendly 😊) clicks.

Example to programmatically access a `csv_file` csv from the minio interface (in `bucket`)

```python
import boto3
import pandas as pd
ECLR_S3 = boto3.session.Session().resource(
    service_name='cloud',
    aws_access_key_id='MINIO_ROOT_USER',
    aws_secret_access_key='MINIO_ROOT_PASSWORD',
    endpoint_url='admin_minio_url',
    region_name='eu-de',
    use_ssl=True,
    verify=True,
)
df = pd.read_csv(ECLR_S3.Object(bucket_name='bucket',
                                key='csv_file').get()['Body'])
```

!!! note 
    Ideally we would have preferred to have just one `minio` service. While it worked for very old minio images
    (like... Very old. More than 2.5 years, when `MINIO_ROOT_USER` and `MINIO_ROOT_PASSWORD` was not the way to set up credentials), we never managed to make it work 
    for more recent minio image😭. If you think you know how to do it, do not hesitate to contact us! 🥰

!!! note 
    we plan to release a library related to S3 read/write helper methods for exotic file formats. 
    Stay tune! 😊

### Local dev mode

In this case you will make use of the additional  `docker-compose.minio.override.yml` file. Here you will set in your `.env` **MINIO_ROOT_USER and MINIO_ROOT_PASSWORD** 
plus 

- `minio_port`: the local port on which you want to access to the minio web interface. If not set, 9000 is taken by default. 

Then launch
```shell
make minio-dev-launch
```

 minio web interface will be accessible at `http://127.0.0.1:minio_port`