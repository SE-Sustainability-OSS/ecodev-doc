# DB Upsertion

Helper CRUD methods to interact with db

## Version

In order to keep previous versions of a particular (table, column, row) triplet, we use a `Version` table.

```python
class Version(SQLModel, table=True):  # type: ignore
    """
    Table handling versioning.

    Attributes are:
        - table: the table to version
        - column: the column of table to version
        - row_id: the the row id of the column of table to version
        - col_type: the column type
        - value: the value to store (previous row/column/table version)
    """
    __tablename__ = 'version'
    id: Optional[int] = Field(default=None, primary_key=True)
    created_at: datetime = Field(default_factory=datetime.utcnow)
    table: str = Field(index=True)
    column: str = Field(index=True)
    row_id: int = Field(index=True)
    col_type: ColType
    value: str | None = Field(index=True)

    __table_args__ = (
        Index(
            'version_filter_id',
            'table',
            'row_id',
            'column'
        ),
    )
```

To be noted ,the `col_type` attribute encodes what column type we are able to handle (6 as of 2025/03/18)

```python
@unique
class ColType(str, Enum):
    """
    Enum listing all col types allowed to be stored in version class
    """
    STR = 'str'
    BOOL = 'bool'
    INT = 'int'
    FLOAT = 'float'
    DATE = 'datetime'
    ENUM = 'enum'


COL_TYPES = str | int | bool | float | datetime | Enum | None
```

To retrieve previous `Version`, use the following two methods

## `get_versions`

Retrieve all previous versions of a given `(table: str, column: str, row_id: int)` triple

## `get_row_versions`

Retrieve all previous versions all of versioned columns of a given `(table: str, row_id: int)` couple

## `upsert_data` and `upsert_df_data`

**Core methods of these CRUD helpers**. `upsert_df_data` works on a pandas dataframe, and `upsert_data` 
on a `list` of `dict | SQLModel`. `upsert_df_data` converts its input and calls `upsert_data` under the hood.

Both do the upsertion of the passed data and the (optional in the case of `upsert_data`) passed `db_schema: SQLModelMetaclass`. 

NB: `db_schema` is optional for `upsert_data`, as it can be retrieved by 
<a href=https://en.wikipedia.org/wiki/Reflective_programming class="external-link" target="_blank">reflection</a> in the case of a `list` of `SQLModel` objects. 

Upsertion means the following:

- all new `dict` data are inserted into the db. New data are identified by their `sfield` and the `upsert_selector` method (more on that below)
- all `dict` already present in db are upserted, only editing `field` (more on that below), and only storing in `Version` the previous version if the value
  is indeed modified.
  
Example:

```python
class UpFoo(SQLModel, table=True):  # type: ignore
    """
    Test class to test db upsertion
    """
    __tablename__ = 'up_foo'
    id: Optional[int] = Field(default=None, primary_key=True)
    bar1: str = sfield()
    bar2: bool = sfield()
    bar3: str = field()
    bar4: bool = field()
    bar5: float = field()
    bar6: int = field()
    bar7: datetime = field()
    bar8: Permission = field()
    bar9: Optional[str] = field(default=None)

foo = UpFoo(bar1='bar', bar2=True, bar3='bar', bar4=False, bar5=42.42, bar6=42,
            bar7=datetime(2025, 3, 17), bar8=Permission.ADMIN)
foo2 = UpFoo(bar1='bar', bar2=True, bar3='babar', bar4=False, bar5=42.42, bar6=42,
             bar7=datetime(2025, 3, 17), bar8=Permission.ADMIN)
```
Calling `upsert_data` on first `[foo]` and then `[foo2]` will only create one row in `up_foo`, 
and one in `version` (storing the old `bar` value of column `bar4`). You can go on the <a href=https://github.com/SE-Sustainability-OSS/ecodev-core/blob/main/tests/functional/test_db_upsertion.py class="external-link" target="_blank">test suite</a> to learn more.

## `upsert_deletor`

Delete one row of the db corresponding to the passed `values: SQLModel`. 

`db_schema` is retrieved by <a href=https://en.wikipedia.org/wiki/Reflective_programming class="external-link" target="_blank">reflection</a>.

## `field` and `sfield`

simple wrapper methods around <a href=https://sqlmodel.tiangolo.com/ class="external-link" target="_blank">SQLModel</a> `Field`. 
Used to specify whether an attribute is required for uniquely selecting a row (this is `sfield`) or need to be versioned (this is `field`).
The interface is in both cases identical to `Field`, with this additional behaviour. A column no used to select and not to be versioned can use the
regular `Field` (as most presumably every `id` primary key column).

## `upsert_selector`

Select one row of the db corresponding to the passed `values: SQLModel`. 

`db_schema` is retrieved by <a href=https://en.wikipedia.org/wiki/Reflective_programming class="external-link" target="_blank">reflection</a>.

## `db_to_value`

`Version` store all types as str (or `None`) type. To convert back into the original column `col_type`, call this method. 

```python
def db_to_value(db_value: str | None, col_type: type | EnumType) -> COL_TYPES:
    """
    Convert back a str version value stored to a real value (types hancled listed in ColType)
    NB: assumption that if the type is not known, this is an enum type.
    """
    if db_value is None:
        return None
    if col_type in [int, str, float]:
        return col_type(db_value)
    if col_type == bool:
        return True if db_value == 'True' else False
    if col_type == datetime:
        return datetime.strptime(db_value, '%Y-%m-%d %H:%M:%S.%f')
    return col_type[db_value]   # type: ignore[index]
```