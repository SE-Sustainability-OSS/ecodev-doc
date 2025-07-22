# Navlink components


Simple helpers around <a href=https://www.dash-mantine-components.com/components/navlink class="external-link" target="_blank">dmc NavLink</a>
component.

## Navlink

Helper method for creating a navlink

```python
def navlink(label: str,
            href: str,
            icon: str | None = None
            ) -> dmc.NavLink:
    """
    A simple navigation link component.
    """
    return dmc.NavLink(label=label, leftSection=DashIconify(icon=icon), href=href)
```