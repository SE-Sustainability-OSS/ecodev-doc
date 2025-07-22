# Admin interface

You can go read on [ecodev-core](../../../libraries/core/authentication/app_admin_auth.md) for more information on
<a href=https://github.com/aminalaee/sqladmin class="external-link" target="_blank">Sqladmin</a>. It is a very simple to use library that does the equivalent of the
<a href=https://docs.djangoproject.com/en/5.1/ref/contrib/admin/ class="external-link" target="_blank">Django Admin interface</a>.

Set up thanks to the [app entry point](../entry_points/fastapi.md), you get ouf of the box a login screen for this admin interface

<p align="center">
  <a href="/img/ecodev_app/admin.png"><img src="/img/ecodev_app/admin.png" alt="Admin login screen"></a>
</p>
<p align="center">
    <em>Admin login screen</em>
</p>
<p align="center">
</p>

When loggged in, you can modify all table entries corresponding to views that you defined in 
<a href=https://github.com/SE-Sustainability-OSS/ecodev-app/blob/main/app/admin.py class="external-link" target="_blank">admin.py</a> (as you can see,
it is an easy thing to do 😊)


<p align="center">
  <a href="/img/ecodev_app/sqladmin.png"><img src="/img/ecodev_app/sqladmin.png" alt="Admin table view"></a>
</p>
<p align="center">
    <em>Admin table view</em>
</p>
<p align="center">
</p>

And to edit an entry, simply click on thre corresponding icon to gtet the following screen

<p align="center">
  <a href="/img/ecodev_app/sqladmin_view.png"><img src="/img/ecodev_app/sqladmin_view.png" alt="Admin entry view"></a>
</p>
<p align="center">
    <em>Admin entry view</em>
</p>
<p align="center">
</p>

The different fields to edit correspond to the ones defined in the <a href=https://sqlmodel.tiangolo.com/ class="external-link" target="_blank">SQLModel</a> class

```python
"""
Module implementing an example sql table/python class (alchemy handled by sql...model :))
"""
from datetime import datetime

from sqlmodel import Field
from sqlmodel import SQLModel

from app.domain_model import ProductType


class ProductBase(SQLModel):  # type: ignore
    """
    Example base class. See
    https://sqlmodel.tiangolo.com/tutorial/fastapi/multiple-models/
    for good practice examples.
    """

    id: int | None = Field(default=None, primary_key=True)
    type: ProductType
    name: str
    value: float


class Product(ProductBase, table=True):  # type: ignore
    """
    Example sql table/python class
    """
    __tablename__ = 'product'
    id: int | None = Field(default=None, primary_key=True)
    created_at: datetime = Field(default_factory=datetime.utcnow)
```

Here `ProductType` is an Enum

```python
"""
Module implementing an example enum
"""
from enum import Enum
from enum import unique


@unique
class ProductType(str, Enum):
    """
    Example enum
    """
    TECHNOLOGY = 'Technology'
    FOOD = 'Food'
    HEALTH = 'Health'
```

and interpreted like so by sqladmin as you can see on the sqladmin entry view 😊! 