# Stack layouts


Simple helpers around <a href=https://www.dash-mantine-components.com/components/stack class="external-link" target="_blank">dmc Stack</a>
component.

## Group

Helper method for creating a stack

```python
def stack(children: Any,
          id: str = '',
          gap: str = 'md',
          align: str = 'stretch'
          ) -> dmc.Stack:
    """
    Renders a dmc.Stack component that encapsulates a page
    """
    return dmc.Stack(children=children, align=align, justify='flex-end', gap=gap,
                     id=id)
```