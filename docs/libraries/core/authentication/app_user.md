# App User

`AppUser` is the central class used for registering and identifying users. While the best practice would recommand to 
split the identification and the authentication procedures, for the products that we develop at <a href=https://eco-act.com/ class="external-link" target="_blank">EcoAct</a>
it suffices our need to have both concepts centralized in one place.

## AppUser code 

`AppUser` is a simple `SQLModel` descendant

```python

from typing import List
from typing import Optional
from typing import TYPE_CHECKING

from sqlmodel import Field
from sqlmodel import Relationship
from sqlmodel import SQLModel

from ecodev_core.permissions import Permission


if TYPE_CHECKING:  # pragma: no cover
    from ecodev_core.app_rights import AppRight
    
    
class AppUser(SQLModel, table=True):  # type: ignore
    """
    Simple user class: an id associate to a user with a password
    """
    __tablename__ = 'app_user'
    id: Optional[int] = Field(default=None, primary_key=True)
    user: str = Field(index=True)
    password: str
    permission: Permission = Field(default=Permission.ADMIN)
    client: Optional[str] = Field(default=None)
    rights: List['AppRight'] = Relationship(back_populates='user')
```

Here:

- `id` is a self explanatory field, the primary key of the `app_user` table. 
- `user`: a `str` identifying the users corresponding to each instances.
- `password`: a hash password serving for authentication.
- `permission`: a simple enum characterizing the permissions granted to the associated user.
  ```python
  from enum import Enum
  from enum import unique
  
  @unique
  class Permission(str, Enum):
      """
      Enum listing all permission levels an application user can have
      """
      ADMIN = 'Admin'
      Consultant = 'Consultant'
      Client = 'Client'
  ```
- client: a user can be associated to a particular client (think about a multi client application)
- rights: for advanced application, a user can have specified access rights richer than just what permission would presume. More on this [here](app_rights.md).

If in wonder about the funky `TYPE_CHECKING` import, you should definitely go read  <a href=https://sqlmodel.tiangolo.com/tutorial/code-structure/?h=type_checking#import-only-while-editing-with-type_checking class="external-link" target="_blank">this</a> sqlmodel documentation page.

If in wonder about the `Relationship` field, you should definitely <a href=https://www.projectaon.org/en/Main/Home class="external-link" target="_blank">g̶o̶ ̶t̶o̶ ̶p̶a̶g̶e̶ ̶3̶</a>  read  <a href=https://sqlmodel.tiangolo.com/tutorial/relationship-attributes/define-relationships-attributes/ class="external-link" target="_blank">this</a> sqlmodel documentation page, alongside with the following ones.

## `AppUser` creation

We usually create user in three different ways:

- at FastAPI application <a href=https://fastapi.tiangolo.com/advanced/events/#startup-event class="external-link" target="_blank">startup</a>. For that, we have a code of the sort
  ```python
  from core_devoops import engine
  from core_devoops import upsert_app_users
  from sqlmodel import Session

  if (file_path := JSON_PATH).exists():
      with Session(engine) as session:
          upsert_app_users(file_path, session)
  ```
  where `JSON_PATH` is a path towards a json file where `AppUser` instances have been previoulsy serialized (the file this contains a `List` of `Dict`, one `Dict` per `AppUser` to insert.
- Manually in a <a href=https://www.pgadmin.org/ class="external-link" target="_blank">pgadmin</a> interface (more on pgadmin here [here](../../../cookiecutters/infra/stacks/db.md))
- Via a <a href=https://dash.plotly.com/ class="external-link" target="_blank">Dash</a> page. More on that here**TODO**

## `AppUser` uses

We request user via several means. Most of them are described [here](app_admin_auth.md), but there is also the `select_user` method 

### select_user 

`select_user` only asks for a `user` (`str`) and a `session`

```python
from sqlmodel import Session
from ecodev_core import engine
from ecodev_core import select_user

with Session(engine) as session:
    admin_user = select_user('admin', session)
```