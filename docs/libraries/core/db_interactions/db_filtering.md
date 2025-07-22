# DB filtering

Low level helpers to filter data. If you're in a rush, just look at `ServerSideFilter` and `SERVER_SIDE_FILTERS`

## `ServerSideFilter`

An enum characterizing all the (current) filtering possibilities. One value for each filter, that we define below.

We advise you to go check the <a href=https://github.com/SE-Sustainability-OSS/ecodev-core/blob/main/tests/functional/test_db_filters.py class="external-link" target="_blank">test suite</a> as well

### `filter_start_str_field`

Filtering is done by checking if the passed value starts the field.

!!!warning 
    case-insensitive!


### `_filter_str_ilike_field`

Filtering is done by checking if the passed value is contained in db values

!!!warning 
    case-insensitive!

### `_filter_str_like_field`

Filtering is done by checking if the passed value is contained in db values

!!!note 
    case-sensitive 😁

### `__filter_strict_str_field`

Filtering is done by checking if the passed value is equal to one of the db values

!!!note 
    case-sensitive 😁
   
### `__filter_bool_like_field`

Filtering is done by checking if the passed value is equal to one of the db values

!!!note 
    case-sensitive 😁

### `_filter_num_like_field`

Filtering is done by comparing the passed value to db values  with the passed operator.

In the case an example is in order, as we also allow filtering on the field year if a datetime

```python
# test _filter_num_like_field
filtered_date_1 = _filter_num_like_field(
    AppActivity.created_at, select(AppActivity),
    operator='>=', value=datetime.now().year, is_date=True)
filtered_date_2 = _filter_num_like_field(
    AppActivity.created_at, select(AppActivity),
    operator='>', value=datetime.now().year, is_date=True)
filtered_date_3 = _filter_num_like_field(
    AppActivity.created_at, select(AppActivity),
    operator='<=', value=datetime.now().year+1, is_date=True)
filtered_date_4 = _filter_num_like_field(
    AppActivity.created_at, select(AppActivity),
    operator='<', value=datetime.now().year+1, is_date=True)
filtered_date_5 = _filter_num_like_field(
    AppActivity.created_at, select(AppActivity),
    operator='=', value=datetime.now().year, is_date=True)

with Session(engine) as session:
    admin = select_user('admin', session)
    filtered_query_equal = _filter_num_like_field(
        AppUser.id, self.query_user, operator='=', value=admin.id)
    
    result = session.exec(filtered_query_equal).all()
    result_date_1 = session.exec(filtered_date_1).all()
    result_date_2 = session.exec(filtered_date_2).all()
    result_date_3 = session.exec(filtered_date_3).all()
    result_date_4 = session.exec(filtered_date_4).all()
    result_date_5 = session.exec(filtered_date_5).all()
    
self.assertEqual(len(result), 1)
self.assertEqual(len(result_date_1), 1)
self.assertEqual(len(result_date_2), 1)
self.assertEqual(len(result_date_3), 1)
self.assertEqual(len(result_date_4), 1)
self.assertEqual(len(result_date_5), 0)
```