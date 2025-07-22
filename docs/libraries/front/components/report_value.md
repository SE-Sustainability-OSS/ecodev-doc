# Report value components


Simple helpers around <a href=https://www.dash-mantine-components.com/components/text class="external-link" target="_blank">dmc Text</a>
component.

## Report value

Helper method to create a report value: a title, a value and an (optional) associated unit

```python
def report_value(title: str,
                 value: str | int,
                 unit: str | None = None
                 ) -> html.Div:
    """
    Renders a human readable (reportable) value
    """
    if isinstance(value, (int, float)) or value.isnumeric():
        value = number_formatting(float(value))

    return html.Div(
        dmc.Group(
            [
                dmc.Text(title, fw=700, fz='16px', c='grey'),
                dmc.Text(value, fw=700, fz='16px'),
                dmc.Text(unit, fz='14px', fs='italic')
            ]
        )
    )
```

Rendering:

<p align="center">
  <a href="/img/ecodev_front/report_value.png"><img src="/img/ecodev_front/report_value.png" alt="Report value"></a>
</p>
<p align="center">
    <em>An example of such a component in a dash application</em>
</p>
<p align="center">
</p>
