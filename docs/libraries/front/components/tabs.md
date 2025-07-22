# Tabs components


Simple helpers around <a href=https://www.dash-mantine-components.com/components/tabs class="external-link" target="_blank">dmc Tab</a>
component. This is only valid for DMC versions prior to those using `TabsTab`

## Tabs

Helper method to deal with a list of tabs

```python
def tabs(tabs_list: list[dmc.Tab], tabs_id: str, value: str) -> dmc.Tabs:
    """
    Renders a dmc.Tabs components
    """
    return dmc.Tabs(children=[dmc.TabsList(tabs_list)], id=tabs_id, value=value)
```

## Tab

Helper method for creating a tab

```python
def tab(value: str):
    """
    Renders a dmc.Tab component
    """
    return dmc.Tab(' '.join(value.split('_')).capitalize(), value=value)
```