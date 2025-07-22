# Forge key

## Prefix stripping

In order to access a resource stored on a S3 like/Blob like storage,you obviously need to specify its
location. 

using S3 object storage terminology, we call `key` the full path to access to your desired resource. 

Coming from the docker universe, the `ecodev` team had the habbits of <a href="https://docs.docker.com/storage/volumes/" class="external-link" target="_blank"> mounting a `data` volume</a>
in the root `/app` folder. 

In certain corner cases, even if you are familiar with S3/blob storage, you might stil want to access 
data as fast as can be (hence using something like an  <a href="https://aws.amazon.com/efs/?nc1=h_ls" class="external-link" target="_blank">Elastic File Storage</a>).

So to be coherent between disk accesses and S3/blob accesses, we took the habit of always prefixing 
our pathlib <a href=https://docs.python.org/3/library/pathlib.html class="external-link" target="_blank">Path</a> 
with `app`. Not wanting to have this `app` present on the S3/blob, we remove it with the `forge_key`
method

```python
def forge_key(file_path: Path) -> str:
    """
    Form a valid cloud key out of the passed file_path
     (basically trailing the leading ecodev_cloud/ parent)
    """
    return str(file_path.relative_to(*file_path.parts[:2]))
```

It proved to be very convenient for us to do so, but we understand that someone starting from scratch 
its S3/blob journey would rather prefer not to have this funky prefix stripping 😅. 

If you are in this case do not hesitate on creating an issue at <a href="https://github.com/SE-Sustainability-OSS/ecodev-cloud" target="_blank">https://github.com/SE-Sustainability-OSS/ecodev-cloud</a>,
and we will think about a way to deal with other scenarios (most presumably with a new env variable).

In the meantime, keep in mind that the highest folder in your `path` will be stripped when interacting with the distance storage.

## Example 

To try to vindicate our funky prefix stripping choice, here find a real life example of how we create
a `Folder` pydantic class with all storage (being it docker volumes, EFS like, S3/blob storage...) locations
aggregated in one place (very convenient, and can be instanciated differently for tests!)

```python
from pydantic import BaseModel
from pathlib import Path

"""
Root Directory of the docker
"""
ROOT_DIRECTORY = Path('/app')
ROOT_SHARED_DATA_DIR = Path('/app/shared_data')
"""
Directory where all climate_model_data should be put
"""
CLIMATE_MODEL_DATA = 'climate_model_data'
CLIMATE_MODEL_DATA_DIRECTORY = ROOT_DIRECTORY / 'climate_model_data'
"""
Directory where all indicators are stored
"""
INDICATOR_DIRECTORY = ROOT_DIRECTORY / 'indicators'
"""
Directory where all client data are to be found
"""
CLIENT_DIRECTORY = ROOT_SHARED_DATA_DIR / 'clients'
"""
Directory where all geographical data should computed/stored
"""
GEOGRAPHICAL_DIRECTORY = ROOT_SHARED_DATA_DIR / 'geographical_data'
"""
Directory where all logs are stored for subsequent analysis
"""
LOGS_DIRECTORY = ROOT_SHARED_DATA_DIR / 'logs'


class Folders(BaseModel):
    """
    Simple class storing all important folders. In production, these are the folders mounted on the
     container. In the end-to-end test test_generate_client_output, different values are used so
     as not to erase all production information :)
    """
    data: Path
    geo: Path
    indicator: Path
    client: Path
    logs: Path

"""
All important folders in production mode
"""
PROD_FOLDERS = Folders(data=DATA_DIRECTORY,
                       geo=GEOGRAPHICAL_DIRECTORY,
                       indicator=INDICATOR_DIRECTORY,
                       client=CLIENT_DIRECTORY,
                       logs=LOGS_DIRECTORY)
```

Here we see the interest of out `forge_key` prefix stripping and of `ecodev-cloud`: the disk and storage data are treated all the same.
