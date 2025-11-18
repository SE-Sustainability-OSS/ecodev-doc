## Rest API client
We provide a template for creating REST API endpoints [here](/docs/cookiecutters/app/entry_points/fastapi.md) using [FastAPI](https://fastapi.tiangolo.com/).

Below, is an example of how the template can be extended with endpoints for business logic operations.

```py
#app.py
from ecodev_core import AppUser
from ecodev_core import get_user

def _some_computation(inputs: list[float]) -> float:...

@app.get('/compute')
def do_computation(
    inputs: Annotated[list[float], Query(description="Business computation 1")],
    user: AppUser = Depends(get_user)) -> float:
    """
    Do computation
    """
    return _some_computation(inputs)
```

The endpoint defined above will be accessible only by app users. The client calling the endpoint will have to provide the token retrieved from the login_route defined in the [template](/docs/cookiecutters/app/entry_points/fastapi.md) (the hashing algorithm and expiration period for the token management can be managed [here](/docs/libraries/core/authentication/auth_configuration.md)).

We provide a high level REST API client for calling endpoints defined using the template. This client has the following functionalities :
- 5 main HTTP request methods
- No silent fails, will raise an error if the status code of the response is erroneous
- Works with current stack with limited extra setup
- Auto-refreshes token if expired or close to expiration

Before using it, make sure that you are using the same app setup as the application you are calling (with specific attention to the token management environment variables [here](docs/libraries/core/authentication/auth_configuration.md) which should be identical).

Additionally, the host of the apis as well as the authentication credentials (username and password) needed to access the API need to be stored as environment variables as defined [here](/docs/libraries/core/rest_api_client/rest_api_configuration.md).

The client can be used to make calls to the endpoints as follows :

```py
from ecodev_core import get_rest_api_client

rest_api_client = get_rest_api_client()

url = 'http://example/compute/'
params = {'inputs':[1, 2, 3],}
response = rest_api_client.get(url=url, params=params) #Returns a float

bad_url = 'http://example/does-not-exist'
params = {'param_a':['A','B','C'], 'param_b':'1',}
response = rest_api_client.get(url=bad_url) #raises error with message "Error 404 : {"detail":"Not Found"}"
```

The [client](https://github.com/SE-Sustainability-OSS/ecodev-core/blob/main/ecodev_core/rest_api_client.py) can be trivially extended to make calls to different applications with different credentials.
