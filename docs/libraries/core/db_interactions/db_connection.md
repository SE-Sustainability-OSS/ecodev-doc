# DB connection

Low level helper methods to setup the database connection and the orm interaction with the database.

## `engine`

Simple adaptation of <a href=https://sqlmodel.tiangolo.com/tutorial/create-db-and-table/#create-the-engine class="external-link" target="_blank">SQLModel official documentation</a>
to connect to a SQL database. At EcoAct we use <a href=https://www.postgresql.org/ class="external-link" target="_blank">postgresql</a> and are happy with it

## `create_db_and_tables`

Simple wrapper around  <a href=https://sqlmodel.tiangolo.com/tutorial/create-db-and-table/#calling-create_all class="external-link" target="_blank">SQLModel create_all method</a>.
Make sure to read this documentation, as the order matters between importing Models and this `create_db_and_tables`.

What you usally find in our applications (being Dash or FastAPI):
```python
# Go read https://sqlmodel.tiangolo.com/tutorial/create-db-and-table/?h=create_all#sqlmodel-metadata-order-matters to learn more
from ecodev_core import create_db_and_tables
import app.db_model as db_model # BELOW for a reason!
```

## `delete_table`

Simple helper to drop all passed `SQLModel` model associated table entries.

```python
def delete_table(model: Callable) -> None:
    """
    Delete all rows of the passed model from db
    """
    with Session(engine) as session:
        result = session.execute(delete(model))
        session.commit()
        log.info(f'Deleted {result.rowcount} rows')
```

## `get_session`

Retrieves the session, used in Depends() attributes of fastapi routes. Completely inspired from the
official <a href=https://sqlmodel.tiangolo.com/tutorial/fastapi/session-with-dependency/?h=#create-a-fastapi-dependencyclass="external-link" target="_blank">SQLModel documentation</a>,
that you should read in its entirety (best return on a 1/2 day dev investment ever 😊) 
