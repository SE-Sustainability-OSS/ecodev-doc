# Notifications

A few notification templates to use in the app.

## Error notification

Notification when an error is encountered. 
If the error message is long, only the first 50 characters are displayed.
The full error message is available in a Popover.

Requires both:
* a title;
* an error message.

```python
def get_error_notif(title_name: str, raw_msg: str) -> dmc.Notification:
    """
    Generate an 'error' notification
    """
    msg = raw_msg
    if len(raw_msg) > 50:
        top_msg = raw_msg.lstrip()[:50]
        msg = dmc.Stack([
            top_msg,
            dmc.Popover(
                children=[
                    dmc.PopoverTarget(dmc.Button('More details...')),
                    dmc.PopoverDropdown(
                        dmc.Textarea(
                            raw_msg,
                            autosize=True,
                            w=500,
                            disabled=True
                        )
                    ),
                ]
            )
        ])
    return dmc.Notification(
        id=f'notif_{title_name}_error_id',
        title=f"Error in '{title_name}'",
        message=msg,
        loading=False,
        color='red',
        action='show',
        autoClose=False,
        icon=DashIconify(icon='akar-icons:circle-check'),
    )

```


## Information notification

A notification to display information.

Requires both:
* a title;
* an information message.

```python
def get_info_notif(title_name: str, msg: str) -> dmc.Notification:
    """
    Generate an 'info' notification
    """
    return dmc.Notification(
        id=f'notif_{title_name}_info_id',
        title=f'{title_name} - info',
        message=msg,
        loading=False,
        color='blue',
        action='show',
        autoClose=2000,
    )
```


## Launch notification

To be used when a (long) process is launched.
The notification can then be updated to mark the end of the process.
See also `get_complete_notif` below.

Simply requires a title.

```python
def get_launch_notif(title_name: str) -> dmc.Notification:
    """
    Generate a 'launch' notification
    """
    return dmc.Notification(
        id=f'notif_{title_name}_id',
        title=f'{title_name} - initiated',
        message='The process has started',
        loading=True,
        color='orange',
        action='show',
    )
```

## Complete notification

Notification to signal that a process is complete.

Requires a title, but a detailed message can also be provided.


```python
def get_complete_notif(
        title_name: str, notif_id: str | None = None,
        message: str | None = None, autoclose: int | None = None
) -> dmc.Notification:
    """
    Generate a 'process complete' notification
    """
    notif_id = notif_id or f'notif_{title_name}_id'
    message = message or 'The process is now complete'
    autoclose = autoclose or 2000

    return dmc.Notification(
        id=notif_id,
        title=f'{title_name} - complete',
        message=message,
        loading=False,
        color='green',
        action='show',
        autoClose=autoclose,
        icon=DashIconify(icon='akar-icons:circle-check'),
    )
```
