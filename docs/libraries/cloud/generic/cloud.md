# Cloud configuration


## Cloud providers

`ecodev-cloud` (as of today) can handle `AWS` and `Azure` cloud providers. This is reflected by the 
`Cloud` enum

```python
@unique
class Cloud(str, Enum):
    """
    All cloud object storage sources
    """
    AZURE = 'Azure'
    AWS = 'Aws'
```

To be more precise, the S3 protocol and the blob protocol are covered by `ecodev-cloud`, so that you
can for instance use `AWS` for  <a href="https://www.open-telekom-cloud.com/en/products-services/core-services/object-storage-service" class="external-link" target="_blank">`T-system` object storage </a> for instance,
which is a S3 compatible storage. Other <a href="https://european-alternatives.eu/category/object-storage-providers" class="external-link"  target="_blank">European S3 compatible storage providers</a>.

## `CloudConfiguration`

To switch between the two protocols, just populate your `.env` <a href=https://www.codementor.io/@parthibakumarmurugesan/what-is-env-how-to-set-up-and-run-a-env-file-in-node-1pnyxw9yxj  class="external-link" target="_blank">file</a>
with the `cloud_provider` field (set to `Azure` or `AWS`).

If you do not specify anything, `AWS` will be taken as default.

