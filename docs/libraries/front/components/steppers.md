# Stepper components


Simple helpers around <a href=https://www.dash-mantine-components.com/components/stepper class="external-link" target="_blank">dmc Stepper and StepperStep</a>
components.

## Vertical stepper

Helper methods to create a vertical stepper with several stepper steps

```python
def vertical_stepper(id: str,
                     steps: list[dmc.StepperStep],
                     color: str = 'default-color'
                     ) -> dmc.Stepper:
    """
    Returns a vertical stepper.
    """
    return dmc.Stepper(
        id=id,
        active=0,
        orientation='vertical',
        color=color,
        children=steps
    )
```

## Stepper step

Helper method for creating a stepper step

```python
def stepper_step(label: str,
                 icon: str,
                 description: str | None = None,
                 href: str | None = None
                 ) -> dmc.StepperStep:
    """
    Returns a stepper step with redirecting icons, if provided with an href.
    """
    icon = DashIconify(icon=icon, width=22)
    active_step = dmc.Anchor(icon, href=href, c='#495057', inline=True) if href else icon
    completed_step = dmc.Anchor(icon, href=href, c='white', inline=True) if href else icon

    return dmc.StepperStep(
        label=label,
        description=description,
        icon=active_step,
        completedIcon=completed_step
    )
```