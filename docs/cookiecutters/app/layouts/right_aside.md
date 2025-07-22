# Right aside page

<p align="center">
  <a href="/img/ecodev_app/right_aside.png"><img src="/img/ecodev_app/right_aside.png" alt="Right aside layout"></a>
</p>
<p align="center">
    <em>Right aside page layout</em>
</p>
<p align="center">
</p>


A page with a right aside

```python
import dash_mantine_components as dmc
from dash import callback
from dash import html
from dash import Input
from dash import Output
from dash import register_page
from dash import State
from ecodev_core import safe_get_user
from ecodev_front import ASIDE
from ecodev_front import TOKEN
from ecodev_front import URL

from app.components.page_helpers import generic_page
from app.pages.layouts import RIGHT_ASIDE_PAGE_ID
from app.pages.layouts import RIGHT_ASIDE_PAGE_URL


register_page(__name__, path=RIGHT_ASIDE_PAGE_URL)

layout = [html.Div(id=RIGHT_ASIDE_PAGE_ID)]


@callback(Output(ASIDE, 'children'),
          Input(URL, 'pathname'),
          State(TOKEN, 'data'))
def enable_right_aside(pathname: str, token: dict):
    """
    Callback rendering the left aside (setting width and content)
    """
    right_aside_content = dmc.Text('Example of right aside content',
                                   fz=24, style={'padding': '20px'})
    if safe_get_user(token) and pathname == RIGHT_ASIDE_PAGE_URL:
        return right_aside_content


@callback(Output(RIGHT_ASIDE_PAGE_ID, 'children'),
          Input(TOKEN, 'data'))
def render_page(token: dict):
    """
    Callback rendering the page's main content.
    """
    page = dmc.Container(['This content is offset by the right aside'])

    return generic_page(token, page)
```


See rendering above.

The only difference with the [default template](simple_layout.md) is the `enable_right_aside` callback, controlling the right aside.

Note that the id of the right aside is `ASIDE` in our paradigm.