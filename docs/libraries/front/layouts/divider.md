# Divider layouts


Simple helpers around <a href=https://www.dash-mantine-components.com/components/divider class="external-link" target="_blank">dmc Divider</a>
component.

## Divider

Helper method for creating a divider

```python
def divider(orientation: str = 'horizontal',
            label: str | None = None,
            position: str = 'center',
            margin: int = 10,
            w: str | int = ''
            ) -> dmc.Divider:
    """
    Renders a divider
    """
    return dmc.Divider(orientation=orientation,
                       label=label, labelPosition=position,
                       m=margin, w=w)
```

## Header Divider


Helper method for creating a header divider

```python
def header_divider() -> dmc.Divider:
    """
    Generates the vertical navbar dividers between app header sections
    """
    return dmc.Divider(orientation='vertical',
                       style={'color': '#f2f2f2',
                              'marginTop': '10px',
                              'marginBottom': '10px'})
```


Rendering:

<p align="center">
  <a href="/img/ecodev_front/header_divider.png"><img src="/img/ecodev_front/header_divider.png" alt="Header divider"></a>
</p>
<p align="center">
    <em>An example of such a component in a dash application</em>
</p>
<p align="center">
</p>