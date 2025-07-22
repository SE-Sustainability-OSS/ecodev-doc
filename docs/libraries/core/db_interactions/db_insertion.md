# DB Insertion

Helpers for upserting data.

## `generic_insertion`

Usually called in a post route eating a fastapi <a href=https://fastapi.tiangolo.com/reference/uploadfile/ class="external-link" target="_blank">`UploadFile`</a>,
either synchronously or asynchronously (with fastapi <a href=https://fastapi.tiangolo.com/reference/background/ class="external-link" target="_blank">`BackgroundTask`</a> ) a 
`pd.DataFrame` of `pd.ExcelFile`. 

Arguments are: 

- `df_or_xl`: the data to insert, converted from `UploadFile` to a pandas object with [`get_raw_df`](#`get_raw_df`)
- `session` the db session. See [here](db_connection.md) for more information on session.
- `insertor`: a Callable, eating the two previous arguments and inserting the data into db.
- `background_tasks`: if provided, does the insertion in the background in an asynchronous manner.

## `Insertor`

A class helping to perform insertions. Attributes are:

- `reductor`: how to create or update a row in db

  example for [`AppUser`](../authentication/app_user.md)

  ```python
   def user_reductor(in_db_row: AppUser,  db_row: AppUser) -> AppUser:
    """
    Update an existing in_db_row with new information coming from db_row
    """
    in_db_row.permission = db_row.permission
    in_db_row.password = db_row.password
    return in_db_row
  ```

- `db_schema`: the default constructor of the sqlmodel based class defining the db table. Example for [`AppUser`](../authentication/app_user.md): `AppUser` 😊

- `selector`: the criteria on which to decide whether to create or update (example: only add
  a user if a user with the same name is not already present in the db)  Example for [`AppUser`](../authentication/app_user.md)
  ```python
  def user_selector(db_row: AppUser) -> SelectOfScalar:
    """
    Criteria on which to decide whether creating a new row or updating an existing one in db
    """
    return select(AppUser).where(col(AppUser.user) == db_row.user)
  ```

- `convertor`: how to convert the raw csv/excel passed by the user to json like db rows. Example for [`AppUser`](../authentication/app_user.md)
  ```python
  def user_convertor(df: pd.DataFrame) -> List[Dict]:
    """
    Dummy user convertor
    """
    return [x for _, x in df.iterrows()]
  ```

- `read_excel_file`: whether to insert data based on an xlsx (if true) or a csv (if false)

Example to insert `AppUser` can be found in the tests  <a href=https://github.com/SE-Sustainability-OSS/ecodev-core/blob/main/tests/functional/test_db_insertion.py class="external-link" target="_blank">test suite</a>

But basically, all one needs to do is to define

```python
from fastapi import FastAPI
from fastapi import status
from fastapi import UploadFile
from sqlmodel import Session
from fastapi import Depends
from ecodev_core import is_admin_user
from ecodev_core import get_session
from ecodev_core import AppUser
from fastapi import File
from ecodev_core.db_insertion import insert_file

app = FastAPI()
USER_INSERTOR = Insertor(convertor=user_convertor, 
                         selector=user_selector,
                         reductor=user_reductor, 
                         db_schema=AppUser, 
                         read_excel_file=False)

@app.post('/user-insert',
          status_code=status.HTTP_201_CREATED, response_model=None)
async def user_insert(*, file: UploadFile = File(...),
                      user: AppUser = Depends(is_admin_user),
                      session: Session = Depends(get_session),
                      ) -> None:
    """
    Insert seed ClimateFactor, System and Sector data
    """
    await insert_file(file, USER_INSERTOR, session)
```

More on `insert_file` in the [next sections](#`insert_file`)


## `insert_data`

Inserts a csv/df into a database

Attributes are: 

- `df_or_xl`: the data to insert, converted from `UploadFile` to a pandas object with [`get_raw_df`](#`get_raw_df`)
- `session` the db session. See [here](db_connection.md) for more information on session.
- `insertor`: an `Insertor`, as defined  [here](#insertor)

## `insert_file`

Simple wrapper around [`insert_data`](#`insert_data`) and [`get_raw_df`](#get_raw_df)

```python
async def insert_file(file: UploadFile, insertor: Insertor, session: Session) -> None:
    """
    Inserts an uploaded file into a database
    """
    df_raw = await get_raw_df(file, insertor.read_excel_file)
    insert_data(df_raw, insertor, session)
```

## `get_raw_df`

Convert a passed `file: UploadFile` to a pandas object, either excel or csv given the passed `read_excel_file: bool`.
If `read_excel_file` is `False`, ` sep: str = ','` can be specified to correctly read the csv embbeded in the `UploadFile`