# Search bar components


Simple helpers around <a href=https://www.dash-mantine-components.com/components/segmentedcontrol class="external-link" target="_blank">dmc TextInput and Text</a>
components.

## Search bar

Helper method to create a simple search bar

```python
def search_bar(id: str,
               label: str,
               helper: str,
               helper_text_color: str = 'gray',
               style: dict[str, str] | None = None,
               icon: str = 'quill:search',
               debounce: int = 750
               ) -> html.Div:
    """
    Component to create a centered big / main search bar.

    Note: Default style has a width of 50vh.
    """
    style = style or {'width': '50vh'}
    return html.Div([
        dmc.Stack([
            dmc.TextInput(id=id,
                          placeholder=label,
                          leftSection=DashIconify(icon=icon, width=20),
                          radius='xl', size='lg',
                          debounce=debounce,
                          style=style),
            dmc.Text(helper, size='sm', fs='italic', c=helper_text_color)
        ])
    ], style={'display': 'flex',
              'justifyContent': 'center',
              'alignItems': 'center',
              'textAlign': 'center'})
```

Rendering:

<p align="center">
  <a href="/img/ecodev_front/search_bar.png"><img src="/img/ecodev_front/search_bar.png" alt="Search Bar"></a>
</p>
<p align="center">
    <em>An example of such a component in a dash application</em>
</p>
<p align="center">
</p>
