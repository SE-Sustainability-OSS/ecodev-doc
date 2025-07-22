# Footer layouts


Helper method to create a footer.

```python
def app_footer(children: html.Div,
               color: str = '#0066A1',
               height: str = '4vh',
               display: str = 'flex',
               ) -> html.Div:
    """
    Main app footer
    """
    return html.Div(children, id=FOOTER_ID, style=footer_style(color, height, display))
```

It's default style is 

```python
def footer_style(color: str = '#0066A1',
                 height: str = '5vh',
                 display: str = 'flex'
                 ) -> dict[str, str | int]:
    """
    Main app footer style
    """
    return {
        'position': 'fixed',
        'bottom': 0,
        'height': height,
        'width': '100vw',
        'paddingBottom': '10px',
        'backgroundColor': color,
        'color': 'white',
        'display': display,
        'textAlign': 'center',
        'alignContent': 'center',
        'justifyContent': 'center',
    }
```


Rendering: 

<p align="center">
  <a href="/img/ecodev_front/footer.png"><img src="/img/ecodev_front/footer.png" alt="Footer components"></a>
</p>
<p align="center">
    <em>An example of such a component in a dash application</em>
</p>
<p align="center">
</p>
