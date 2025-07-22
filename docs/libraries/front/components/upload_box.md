# Upload box components

Simple wrapper around <a href=https://dash.plotly.com/dash-core-components/upload class="external-link" target="_blank">the dcc Upload</a>
component. Freezes as shown below some of the parameters:

```python
def upload_box(id: str,
               label: str,
               multiple: bool,
               style: dict[str, str] | None = None
               ) -> dcc.Upload:
    """
    Returns an upload box
    """
    return dcc.Upload(id=id, multiple=multiple, style=style or DEFAULT_STYLE,
                      children=html.H5(label, style={'display': 'flex',
                                                     'justifyContent': 'center',
                                                     'alignItems': 'center',
                                                     'textAlign': 'center'}))
```

`DEFAULT_STYLE` is as follow:

```python
DEFAULT_STYLE = {'height': '30vh',
                 'lineHeight': '60px',
                 'borderWidth': '1px',
                 'borderStyle': 'dashed',
                 'borderRadius': '10px',
                 'textAlign': 'center',
                 'margin': 'auto',
                 'color': '#A0AEC0',
                 'verticalAlign': 'middle'}
```

Rendering:

<p align="center">
  <a href="/img/ecodev_front/upload_box.png"><img src="/img/ecodev_front/upload_box.png" alt="Upload box"></a>
</p>
<p align="center">
    <em>An example of such a component in a dash application</em>
</p>
<p align="center">
</p>
