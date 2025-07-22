# Segmented control components


Simple helpers around <a href=https://www.dash-mantine-components.com/components/segmentedcontrol class="external-link" target="_blank">dmc SegmentedControl</a>
component.

## Segmented control

Helper method to create a simple segmented control, with sensible default parameters

```python
def segmented_control(id: str,
                      label: str,
                      value: str | None = None,
                      data: list[str] | None = None
                      ) -> dmc.SegmentedControl:
    """
    Renders a dmc.SegmentedControl that allows to toggle between different values
    """
    return dmc.SegmentedControl(
        id=id,
        label=label,
        value=value,
        data=data or []
    )
```

## Segmented control with label

Helper method for creating a segmented control with a stylized label, with sensible default parameters for the select

```python
def segmented_control_label(id: str,
                            label: str,
                            value: str | None = None,
                            data: list[str] | None = None,
                            size: str = 'md',
                            c: str = '#A0AEC0') -> dmc.Stack:
    """
    Renders a segmented control with a stylized label
    """

    return dmc.Stack([
        dmc.Text(label, c=c),
        dmc.SegmentedControl(
            id=id,
            value=value,
            data=data or [],
            size=size,
        )
    ], gap='xs', w='100%')
```