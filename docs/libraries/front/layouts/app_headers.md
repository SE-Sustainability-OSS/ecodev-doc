# App Headers

Helper components for creating an application header

## App logo

Helper methods for creating a logo. Use by default a png image named `logo` that should be put into the app `asset folder`

```python
def app_logo(logo_path: str = '/assets/logo.png',
             ratio: float = 570 / 128,
             width: str = '120px',
             style: dict[str, str] | None = None
             ) -> dmc.AspectRatio:
    """
    Application logo component.
    """
    style = style or {'margin-left': '10px', 'width': width}
    return dmc.AspectRatio(dmc.Image(src=logo_path),
                           ratio=ratio, style=style)
```

## App title

Helper methods for creating a title. Go search by default for an environment variable named `app_name`

```python
def app_title(app_name: str | None = None, color='white') -> html.Div:
    """
    Application title component.
    """
    app_name = app_name or os.getenv('app_name')
    return html.Div(dmc.Title(app_name, c=color, fz=32))
```

## App header

Simple application header, composed of an app logo and title components.

```python
def app_header(logo: html.Div, title: html.Div) -> dmc.GridCol:
    """
    Application header, composed of an app logo and title components.
    """
    return dmc.GridCol(span='auto',
                       visibleFrom='md',
                       children=[
                           dmc.Anchor(href='/', children=[
                               dmc.Group([logo, title],
                                         justify='space-around',
                                         style={'margin-top': '5px'}),
                           ])
                       ])
```

Rendering:

<p align="center">
  <a href="/img/ecodev_front/app_header.png"><img src="/img/ecodev_front/app_header.png" alt="App header"></a>
</p>
<p align="center">
    <em>An example of such a component in a dash application</em>
</p>
<p align="center">
</p>