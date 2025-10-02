# DB Schema Upsertion

!!! warning
    As with many of the modules presented here, better solutions exist elsewhere if you have the time to devote
    to learn and use them. Here <a href=https://alembic.sqlalchemy.org/en/latest/ class="external-link" target="_blank">Alembic</a>
    is the way to go to properly handle rigorous (and version controlled) database schema migration. 
    The alternative that we offer here has only for merit its simplicity and ease of use.

<a href=https://en.wikipedia.org/wiki/Schema_migration class="external-link" target="_blank">Database migration</a> 
is always a touchy and tricky topic. 

The solution we present here only do two things: 

- Update existing `EnumType` types 
- Add new columns to existing tables

But it does it automatically, so that one never has to worry about writing any migration script. The obvious downside
being that the migration are not version controlled, but for product in their infancy it is a perfectly fine tradeoff in our 
experience

## Update existing `EnumType` types

the method `add_missing_enum_values` allows one to update  `EnumType` **already present in database** with new values 
added in the code, and does so automatically by introspection.

What this means if that if one starts with the `Permission` enum

```python
@unique
class Permission(str, Enum):
    """
    Enum listing all permission levels an application user can have
    """
    ADMIN = 'Admin'
    Consultant = 'Consultant'
```

like so, associated to a column of an existing table, and then decide to add a new value to the enum

```python
@unique
class Permission(str, Enum):
    """
    Enum listing all permission levels an application user can have
    """
    ADMIN = 'Admin'
    Consultant = 'Consultant'
    Client = 'Client'
```

Then calling

```python
add_missing_enum_values(Permission, session)
```

will automatically add the new `Client` value on the database side. We recommend on our side to create 
a list of enums to update
```python
# List of all enums for which new values in the associated enum class are automatically added
# on the db side
ENUMS_TO_MIGRATE: list = [
]
```

and to call at FastAPI/Dash app startup 
```python
log.info('inserting new enum data')
for enum_name in ENUMS_TO_MIGRATE:
    add_missing_enum_values(enum_name, session)
```

## Add new columns to existing tables


the method `add_missing_columns` allows one to update an existing `SQLModel` class (with `table=True`): adding a new
column on the code side automatically adds it on the database side at the method call.

What this means if that if one starts with the `UpFoo` table

```python
class UpFoo(SQLModel, table=True):  # type: ignore
    """
    Test class to tes db upsertion
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
```

and then add new columns

```python
class UpFoo(SQLModel, table=True):  # type: ignore
    """
    Test class to tes db upsertion
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
    bar10: Optional[int] = Field(default=None)
    bar100: int = Field(default=0)
    bar11: float | None = Field(default=None)
    bar110: float = Field(default=0.0)
    bar12: str = Field(default='toto')
    bar120: Optional[str] = Field(default=None)
    bar13: Permission = Field(default=Permission.Client)
    bar130: Optional[Permission] = Field(default=None)
    bar14: Optional[int] = Field(default=None, index=True, foreign_key='app_user.id')
    bar15: bool = Field(default=True)
    bar150: Optional[bool] = Field(default=None)
    bar16: bytes = Field(default=b'toto')
    bar160: Optional[bytes] = Field(default=None)
    bar17: dict = Field(sa_type=JSONB, default={'key': 'toto'})
    bar170: Optional[dict] = Field(sa_type=JSONB)
```

Then calling

```python
add_missing_enum_values(UpFoo, session)
```

will automatically add the new `bar` fields database side. We recommend on our side to create 
a list of classes to update
```python
# List of all table for which new columns in the associated sqlmodel class are automatically added
# on the db side
TABLES_TO_UPDATE: list = [
]
```

and to call at FastAPI/Dash app startup 
```python
log.info('inserting new columns in existing tables')
for table_name in TABLES_TO_UPDATE:
    add_missing_columns(table_name, session)
```

!!! note
    as of 2025/10/01 we cover the following python types (both allowing and forbidding `NULL` on the database side):
    
    - `str`
    - `int`
    - `float`
    - `EnumType`
    - `bytes`
    - `bool`
    - `JSONB`

    To be noted, the code also allows to 
    - add an index on the new column
    - associate the new column to another table via a foreign key

    The code snippet above illustrates all of these features.