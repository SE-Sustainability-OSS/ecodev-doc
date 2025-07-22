# Graph layouts


Simple helpers around <a href=https://www.dash-mantine-components.com/components/group class="external-link" target="_blank">dcc Graph</a>
component.

## Graph box

Helper method for creating a graph box.  It allows to format a plotly figure with correct sizing & padding

```python
def graph_box(fig: go.Figure, id: str = '') -> dcc.Graph:
    """
    Helper component to format a plotly figure with correct sizing & padding.
    """
    fig.update_layout(
        margin={'t': 10, 'l': 10, 'b': 0, 'r': 10},
    )
    return dcc.Graph(figure=fig, responsive=True, style={'height': '100%', 'width': '100%'},
                     id=id)
```