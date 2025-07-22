# DB Retrieval

Helper methods to retrieve/count data from DB 

## `ServerSideField`

Simple class used for server side data retrieval.

Attributes are:

- `col_name`: the name as it will appear on a frontend interface showing the field
- `field_name`: the SQLModel attribute name associated with this field (**TODO**: explain the use in the context of ecodev-front. Should be removed in the future, fetch it via `field`)
- `field`: the SQLModel attribute associated with this field
- `filter`: the filtering mechanism to use for this field (more on filtering in [the filtering page](db_filtering.md))

To see more on its use, you can go inspect the  <a href=https://github.com/SE-Sustainability-OSS/ecodev-core/blob/main/tests/functional/test_db_retrieval.py class="external-link" target="_blank">test suite</a>

## `get_rows`

Quite often when you store data in DB, you want to retrieve them 😆.

`get_rows` helps you to do so 

```python
def get_rows(fields: List[ServerSideField],
             model: Any,
             limit: Union[int, None] = None,
             offset: Union[int, None] = None,
             filter_str: str = '',
             search_str: str = '',
             search_cols: Optional[List] = None,
             fields_order: Optional[Callable] = None
             ) -> pd.DataFrame:
```

Its attributes are: 

- `fields`: a list of `ServerSideField` used to filter each column of the passed `model`
- `model`: the SQLModel on which to do the DB request 
- `limit`: if filled, limit the number of elements fetched 
- `offset`: if filled, put an offset before fetching elements. More in the official SQLModel <a href=https://sqlmodel.tiangolo.com/tutorial/fastapi/limit-and-offset/ class="external-link" target="_blank">documentation</a>
- `filter_str`: if filled, filter the `model` based on it. The syntax is taken from <a href=https://dash.plotly.com/datatable/filtering class="external-link" target="_blank">Dash datatable filtering</a>,
  to ease plug with it. Go see the <a href=https://github.com/SE-Sustainability-OSS/ecodev-core/blob/main/tests/functional/test_db_retrieval.py class="external-link" target="_blank">test suite</a> to larn more.
- `search_str`: if filled, corresponds to a textual (regex like) seach on `search_cols`
- `search_cols`: the DB columns of `model` on which to search for `search_str` 
- `fields_order`: The order on which to return the fetched data (sort on this column, then this one...)
 
The data are returned as a dataframe (simple to directly plug on Dash table components, being it data table or <a href=https://dash.plotly.com/dash-ag-grid class="external-link" target="_blank">Dash AG Grid</a>)

## `count_rows`

In the same spirit as [`get_rows`](#`get_rows`), allows to easily count element from SQLModel associated table.


Its attributes are: 

- `fields`: a list of `ServerSideField` used to filter each column of the passed `model`
- `model`: the SQLModel on which to do the DB request 
- `limit`: if filled, limit the number of elements fetched 
- `filter_str`: if filled, filter the `model` based on it. The syntax is taken from <a href=https://dash.plotly.com/datatable/filtering class="external-link" target="_blank">Dash datatable filtering</a>,
  to ease plug with it. Go see the <a href=https://github.com/SE-Sustainability-OSS/ecodev-core/blob/main/tests/functional/test_db_retrieval.py class="external-link" target="_blank">test suite</a> to larn more.
- `search_str`: if filled, corresponds to a textual (regex like) seach on `search_cols`
- `search_cols`: the DB columns of `model` on which to search for `search_str`


Example (go read  [the filtering page](db_filtering.md) as well):

```python
# test that the count_rows method works as intended, assuming we inseted 3 users admin, client and monitoring) 
APP_FILTER = ServerSideField(col_name='user', field_name='user', field=AppUser.user,
                             filter=ServerSideFilter.ILIKESTR)
self.assertTrue(count_rows([APP_FILTER], AppUser) == 3)
self.assertTrue(count_rows([APP_FILTER], AppUser, filter_str='{user} scontains o') == 1)
```
       