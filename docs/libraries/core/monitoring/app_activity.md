# Monitoring your applications

!!! warning
    This page describes an *ad hoc* solution to a problem to which better solutions certainly exist,
    like the famous <a href=https://prometheus.io/ class="external-link" target="_blank">Prometheus</a>/<a href=https://grafana.com/ class="external-link" target="_blank">Grafana</a> stack.

    It presumably reflects our limited knowledge (and if you do know of a better way to do it, do not hesitate to reach to us!! 😊).

    TL DR: If your app sees a huge traffic, go with existing tools. If nevertheless you want to finely track a small number of users that connect from time to time, continue reading.

    In our experience (we setup the Prometheus/Grafana stack for both dash and fastapi applications), tracking users proves cumbersome (Prometheus metrics were not meant to do that).


We provide a very crude user monitoring system in `ecodev_core`.

## App Activity 

A code snippet is hopefully worth a thousand words here 

```python 
from typing import Optional
from datetime import datetime
from sqlmodel import Field
from sqlmodel import SQLModel

class AppActivityBase(SQLModel):
    """
    Simple monitoring class

    Attributes are:
        - user: the name of the user that triggered the monitoring log
        - application: the application in which the user triggered the monitoring log
        - method: the method called by the user  that triggered the monitoring log
        - relevant_option: if filled, complementary information on method (num of treated lines...)
    """
    user: str = Field(index=True)
    application: str = Field(index=True)
    method: str = Field(index=True)
    relevant_option: Optional[str] = Field(index=True, default=None)


class AppActivity(AppActivityBase, table=True):  # type: ignore
    """
    The table version of the AppActivityBase monitoring class
    """
    __tablename__ = 'app_activity'
    id: Optional[int] = Field(default=None, primary_key=True)
    created_at: datetime = Field(default_factory=datetime.utcnow)
```

To store in this monitoring table, we have two methods depending whether we are monitoring a dash or a fastapi endpoint. 

## Dash monitoring 

```python 
dash_monitor(method: str, token: Dict, application: str,  relevant_option: Optional[str] = None)
```

Here we use the token that we usually have in a <a href=https://dash.plotly.com/dash-core-components/store class="external-link" target="_blank">Dash store</a> 

You can see a use example in the <a href=https://github.com/SE-Sustainability-OSS/ecodev-core/blob/main/tests/functional/test_authentication.py#L94 class="external-link" target="_blank">test suite</a>

## FastAPI monitoring 

```python 
fastapi_monitor(method: str, user: AppUser, application: str, session: Session, relevant_option: Optional[str] = None)
```

Here we usually directly have access to the user thanks to <a href=https://fastapi.tiangolo.com/tutorial/dependencies/ class="external-link" target="_blank">fastapi Depends</a> class that 
acts as a middleware to perform certain operations (fetch a user and check if its valid, fetch a db session...)

You can see a use example in the <a href=https://github.com/SE-Sustainability-OSS/ecodev-core/blob/main/tests/functional/test_authentication.py#L94 class="external-link" target="_blank">test suite</a> 

## How to use it in real life?

In the application we want to monitor, we usually put calls to these methods in Dash callbacks/FastAPI routes. 

Then the application to monitor usually has a route of the sort

```python
from typing import List

from ecodev_core import AppActivity
from ecodev_core import AppUser
from ecodev_core import get_recent_activities
from ecodev_core import get_session
from ecodev_core import is_monitoring_user

from fastapi import Depends
from fastapi import FastAPI
from fastapi import status
from sqlmodel import Session

app = FastAPI()


@app.get('/get-activities', status_code=status.HTTP_201_CREATED,
         response_model=List[AppActivity])
async def get_activities(*, last_date: str,
                         user: AppUser = Depends(is_monitoring_user),
                         session: Session = Depends(get_session),
                         ) -> List[AppActivity]:
    """
    Retrieve recent activities from the app
    """
    return get_recent_activities(last_date, session)
```

Where `get_recent_activities` is a `ecodev_core` helper method fetching all `AppActivity` more recent than a passed `datetime`.

Then another central monitoring web app is responsible for calling all `get_activities` from all registered applicaions to monitor. 

With dash you can then constructs a simple dashboard that looks like so in our case

<p align="center">
  <a href="/img/ecodev_core/consolidated_monito.png"><img src="/img/ecodev_core/consolidated_monito.png" alt="Consolidated view"></a>
</p>
<p align="center">
    <em>Our EcoAct monitoring web application in all its splendor 😆</em>
</p>
<p align="center">
</p>

Where we can then easily track a user (hello me! 😆)

<p align="center">
  <a href="/img/ecodev_core/one_user_monito.png"><img src="/img/ecodev_core/one_user_monito.png" alt="One user one app view"></a>
</p>
<p align="center">
    <em>How one user uses a specific application</em>
</p>
<p align="center">
</p>

!!!tip
    If enough interest is shown for the details of this monitoring app, we could consider releasing it as open source as well 😎