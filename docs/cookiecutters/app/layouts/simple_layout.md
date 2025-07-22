# Simple layout

<p align="center">
  <a href="/img/ecodev_app/basic_page.png"><img src="/img/ecodev_app/basic_page.png" alt="Simple layout"></a>
</p>
<p align="center">
    <em>Simple page layout</em>
</p>
<p align="center">
</p>


The default way to create pages. Should normally be as simple as

```python
import dash_mantine_components as dmc
from dash import Input
from dash import Output
from dash import callback
from dash import html
from dash import register_page
from ecodev_front import TOKEN

from app.components.page_helpers import generic_page
from app.pages.layouts import SIMPLE_PAGE_ID
from app.pages.layouts import SIMPLE_PAGE_URL

register_page(__name__, path=SIMPLE_PAGE_URL)

layout = [html.Div(id=SIMPLE_PAGE_ID)]


@callback(Output(SIMPLE_PAGE_ID, 'children'),
          Input(TOKEN, 'data'))
def simple_page_layout_example(token: dict):
    """
    Example of a simple page
    """
    page = dmc.Container(['This is a simple page'])

    return generic_page(token, page)
```

See rendering above