# Typer module

This is just a trivial example on how to use <a href= https://typer.tiangolo.com/ class="external-link" target="_blank">Typer</a>.

Check our code

```python
"""
Module with typer commands
"""
import typer
from ecodev_core import logger_get
from ecodev_core import safe_clt


typer_app = typer.Typer()
log = logger_get(__name__)


@safe_clt
@typer_app.command()
def example_typer_command() -> None:
    """
    Example typer command
    """
    log.info('example typer command')


if __name__ == '__main__':
    typer_app()
```

and then go read the official documentation!

!!! note
    you can also go read [this part of our documentation](../../../libraries/core/low_level_helpers/safe_utils.md#safe_clt) if curious about `safe_clt` (it's good to be curious 🥰)