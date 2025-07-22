# App Right

For advanced application, a user can have specified access rights richer than just what [permission](app_rights.md) would presume.

For instance in a web app, some clients could be allowed to see certain pages, but not others. 

This is handled in `ecodev_core` via the `AppRight` class, in <a href=https://en.wikipedia.org/wiki/One-to-many_(data_model) class="external-link" target="_blank">one to many</a>
(not to be confused with one too many, usually late in a bar 😁) relationship with `AppUser` (meaning a user can have several rights. )

## AppRight code 

`AppRight` is a simple `SQLModel` descendant

```python
from typing import Optional
from typing import TYPE_CHECKING

from sqlmodel import Field
from sqlmodel import Relationship
from sqlmodel import SQLModel


if TYPE_CHECKING:  # pragma: no cover
    from ecodev_core.app_user import AppUser


class AppRight(SQLModel, table=True):  # type: ignore
    """
    Simple right class: listing all app_services that a particular user can access to
    """
    __tablename__ = 'app_right'
    id: Optional[int] = Field(default=None, primary_key=True)
    app_service: str
    user_id: Optional[int] = Field(default=None, foreign_key='app_user.id')
    user: Optional['AppUser'] = Relationship(back_populates='rights')
```

Here:

- `id` is a self explanatory field, the primary key of the `app_right` table. 
- `app_service` is the service granted to the associated user. It could be a web page, an action in a page, accessing a specific data source... 
- `user_id` defines the foreign key (which is the `AppUser` primary key)
- `user` is a convenience offered by `SQLModel`, only available inside a `session`. More on the relationship fields  <a href=https://en.wikipedia.org/wiki/One-to-many_(data_model) class="external-link" target="_blank">here</a> 

## `AppRight` creations

As of 2024/01, we mainly setup app rights manually in a <a href=https://www.pgadmin.org/ class="external-link" target="_blank">pgadmin</a> interface, but we hope
to add a generic page in `ecodev-app` to allow for a simple process to add rights.

## `AppRight` uses

We usually use the `get_app_services` method 

### get_app_services

Feeding `get_app_services` with a `AppUser` and a `session` returns the list of `app_services` the user has access to

```python
from sqlmodel import Session
from ecodev_core import engine
from ecodev_core import select_user
from ecodev_core import get_app_services

with Session(engine) as session:
    admin_services = get_app_services(select_user('admin', session), session)
```

More on `select_user` [here](app_user.md#select_user).

We then use these services to grant or deny access to a specific web page, or to filter on a column of a table. We are confident you can put `AppRight` to some use!