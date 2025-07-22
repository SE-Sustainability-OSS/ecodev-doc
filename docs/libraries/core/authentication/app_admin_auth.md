# App and admin authentication 

This work has not particular value on its own, except aggregating different sources of very skilled
people in a hopefully user friendly manner.

Sources that can be cited:

- <a href=https://fastapi.tiangolo.com/tutorial/security/oauth2-jwt/ class="external-link" target="_blank">FastAPI documentation</a> on OAuth2 with Password (and hashing), Bearer with JWT tokens
- <a href=https://www.youtube.com/watch?v=0sOvCWFmrtA&t=38141s&ab_channel=freeCodeCamp.org class="external-link" target="_blank">Python API Development documentation</a>: an invaluable
free ressource provided as a 19 hours (you read right) video!
- <a href=https://aminalaee.dev/sqladmin/authentication/ class="external-link" target="_blank">Sqladmin documentation</a> on authentication

## app authentication

Here we detail the main methods that should prove useful to people. At the end the section we list all the other available (not explained here) methods.

### attempt_to_log

The method `attempt_to_log` is used to retrieve a  <a href=https://jwt.io/ class="external-link" target="_blank">Json Web Token</a> corresponding to a user,
characterized by its user and password.

```python
from sqlmodel import Session
from ecodev_core import attempt_to_log
from ecodev_core import engine

with Session(engine) as session:
    token = attempt_to_log('monitoring', 'monitoring', session)
```

!!! warning
    The method raises

    - a `HTTPException(status_code=status.HTTP_403_FORBIDDEN, detail=INVALID_USER)` error when the user is not found in db (more on `AppUser` [here](app_user.md)).
    `INVALID_USER` is a hopefully clear error message
    - a `HTTPException(status_code=status.HTTP_403_FORBIDDEN, detail=INVALID_CREDENTIALS)` error when the user is found in db but the password is incorrect(more on `AppUser` [here](app_user.md)).
    `INVALID_CREDENTIALS` is a hopefully clear error message

If the request is valid, the token is returned like so:

```
{'access_token': 'FIRST_HASH.SECOND_HASH.THIRD_HASH', 'token_type': 'bearer'}
```

#### Two Factor Authentication (>=0.0.18)

ecodev-core now supports two factor authentication. `attempt_to_log` can now take an (optional)
argument `tfa_value` to be added encoded to the token returned.  

A good library to generate TFA codes is <a href=https://github.com/pyauth/pyotp class="external-link" target="_blank">pyotp</a>.

### get_current_user

The method `get_current_user` returns a `AppUser` (more on `AppUser` [here](app_user.md)). given a valid <a href=https://jwt.io/ class="external-link" target="_blank">JWT</a>.
It expects a `str` `token` as its input

```python
from ecodev_core import get_current_user

client = get_current_user(CLIENT_TOKEN)
```

!!! warning
    The method raises an `HTTPException(status_code=status.HTTP_401_UNAUTHORIZED, detail=INVALID_USER)`  error when the token does not correspond to any valid user. `INVALID_USER` is a hopefully clear error message

#### Two Factor Authentication (>=0.0.18)

`get_current_user` now takes two additional optional arguments:

- `tfa_check`: whether or not to perform TFA check. `False` by default
- `tfa_value`: the plain TFA value to be compared with the one encoded in the passed `token`. Not used 
   if `tfa_check` is `False`. `None` by default.


### Other available methods/classes

- `safe_get_user`: expects a dictionary
  ```python
    {'token': {'access_token': <HASH>, 'token_type': 'bearer'}}
  ```
   and returns the corresponding `AppUser` if the token `HASH` is valid, `None` otherwise
- `is_admin_user` checks if the `AppUser` correspondding to the passed `str` token argument has Admin permissions (more on permissions `AppUser` [here](app_user.md)), and if so returns the `AppUser`
- `is_monitoring_user` checks if the `AppUser` correspondding to the passed `str` token is the monitoring user (more on monitoring [here](../monitoring/app_activity.md)), and if so returns the `AppUser`
-  `is_authorized_user` checks if the `AppUser` correspondding to the passed `str` token is a valid one (returns a `bool`)
- `get_user` morally the same has `get_current_user`, can be used interchangeably (one of them should be deprecated in the future)
- `get_app_services` more on this in [here](app_rights.md#get_app_services), related to `AppRight`
- `Token` is a simple class for storing an `access_token` and its `token_type`

#### Two Factor Authentication (>=0.0.18)

All the above methods now support TFA when it makes sense, thanks to the two additional optional arguments:

- `tfa_check`: whether or not to perform TFA check. `False` by default
- `tfa_value`: the plain TFA value to be compared with the one encoded in the passed `token`. Not used 
   if `tfa_check` is `False`. `None` by default.

## admin authentication

`ecodev-core` uses  <a href=https://github.com/aminalaee/sqladmin class="external-link" target="_blank">Sqladmin</a> as its admin interface. It is a very simple to use
library, user friendly and the library owner is very nice and responsive (thanks <a href=https://github.com/aminalaee class="external-link" target="_blank">Amin</a> !! 😊).

sqladmin does not come out of the box with an authentication mechanism, but it is pretty straightforward to implement with the official <a href=https://aminalaee.dev/sqladmin/authentication/ class="external-link" target="_blank"> documentation</a>.

We did just so at <a href=https://eco-act.com/ class="external-link" target="_blank">EcoAct</a>.

## JwtAuth

To use sqladmin, one just has to instanciate it:

```python
from ecodev_core import AUTH
from ecodev_core import JwtAuth
from ecodev_core import engine
from fastapi import FastAPI
from sqladmin import Admin

app = FastAPI()
admin = Admin(app, engine,
              authentication_backend=JwtAuth(secret_key=AUTH.secret_key))
```

Here `secret_key` should be set via environment variables. More on `AUTH` [here](auth_configuration.md).

Afterwards, adding an admin view on a table is as simple as

```python
from ecodev_core import AppUser
from sqladmin import ModelView

class AppUserAdmin(ModelView, model=AppUser):  # type: ignore
    """
    AppUser admin view
    """
    column_list = [AppUser.id,
                   AppUser.user,
                   AppUser.permission,
                   AppUser.client,]
    column_searchable_list = [AppUser.user]
admin.add_view(AppUserAdmin)
```

More on `column_searchable_list` and other sqladmin awesome configurations <a href=https://aminalaee.dev/sqladmin/configurations/ class="external-link" target="_blank">here</a>. 
<p align="center">
  <a href="/img/ecodev_core/sqladmin_example.png"><img src="/img/ecodev_core/sqladmin_example.png" alt="Sqladmin example"></a>
</p>
<p align="center">
    <em>Example of sqladmin rendering. You should go read the official documentation now 😊</em>
</p>
<p align="center">
</p>
