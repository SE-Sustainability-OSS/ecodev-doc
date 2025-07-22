# Text components

Simple helpers around <a href=https://www.dash-mantine-components.com/components/text class="external-link" target="_blank">dmc Text</a>
component.

## Text Header

`dmc.Text` tailor made for representing headers. Eats a `str` header

```python
def text_header(text: str) -> dmc.Text:
    """
    Renders a stylized dmc.Text component to display headers
    """
    return dmc.Text(children=text, size='xl', fw=700, ta='center')
```

## Sub text

`dmc.Text` tailor made for representing sub-texts.  Eats a `str` sub-text

```python
def sub_text(text: str) -> dmc.Text:
    """
    Renders a stylized dmc.Text component to display sub-texts
    """
    return dmc.Text(children=text, ta='center')
```