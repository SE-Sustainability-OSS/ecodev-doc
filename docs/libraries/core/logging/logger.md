# Logger

Logging is always a heated subject. You should definitely have it, but how? Some prefere relying on existing libraries.

Among those we are aware of, we can vouch for

- <a href=https://github.com/Delgan/loguru class="external-link" target="_blank">loguru</a>, user friendly logging, period 😊.
- <a href=https://github.com/gruns/icecream class="external-link" target="_blank">icecream</a>, more for debugging purposes as they advocate themselves

Our solution is more similar to loguru, but without the additional dependency.


## How to use logging?

### Vanilla `log`

Simply put on top of all of your modules

```python
from ecodev_core import logger_get

log = logger_get(__name__)
```

Afterwards, you use log as you would use it normally: `log.info('info')`, `log.critical('critical')`...
The benefits of using the `ecodev_core` `logger` can hopefully be illustrated in the next image

<p align="center">
  <a href="/img/ecodev_core/logging.png"><img src="/img/ecodev_core/logging.png" alt="logger example"></a>
</p>
<p align="center">
    <em>Benefits of using ecodev_core logger</em>
</p>
<p align="center">
</p>

As you can see, you get out of the box

- colors
- datetime
-  <a href=https://en.wikipedia.org/wiki/Process_identifier class="external-link" target="_blank">ppid</a>
- the name of the method and module where the log sits, alongside with the exact code line
- the actual message

In production, the colors prove invaluable when scanning big chunk of logs (yes, we can improve on the automatic log treatment side  😊). Same goes for where the log was generated.

### `log_critical`

sometimes you want to log an error inside a try except. This you can accomplish in the following manner

```python
from ecodev_core import logger_get

log = logger_get(__name__)

log_critical("something terrible happened", log)
```

<p align="center">
  <a href="/img/ecodev_core/log_critical.png"><img src="/img/ecodev_core/log_critical.png" alt="log critical"></a>
</p>
<p align="center">
    <em>Benefits of using ecodev_core log_critical</em>
</p>
<p align="center">
</p>

This proves particularly useful in <a href=https://dash.plotly.com/ class="external-link" target="_blank">Dash</a> applications, where by default if do not use this logger you
get very uninformative messages

<p align="center">
  <a href="/img/ecodev_core/log_dash.png"><img src="/img/ecodev_core/log_dash.png" alt="log critical"></a>
</p>
<p align="center">
    <em>Sometimes dash is not really nice</em>
</p>
<p align="center">
</p>