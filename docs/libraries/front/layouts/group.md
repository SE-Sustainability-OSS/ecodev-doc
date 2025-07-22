# Group layouts


Simple helpers around <a href=https://www.dash-mantine-components.com/components/group class="external-link" target="_blank">dmc Group</a>
component.

## Stack

Helper method for creating a group

```python
def group(children: list[Any],
          grow: bool = True,
          id: str = '',
          justify: str = 'space-around'
          ) -> dmc.Group:
    """
    Renders a dmc.Group component with the correct formatting
    """
    return dmc.Group(children=children, grow=grow, justify=justify, id=id)
```