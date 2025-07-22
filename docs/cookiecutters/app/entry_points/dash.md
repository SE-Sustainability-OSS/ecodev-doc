# Dash module

This is the main entry point for the frontend web interface. You should not interact that often with 
this technical <a href=https://github.com/SE-Sustainability-OSS/ecodev-app/blob/main/app/dash.py class="external-link" target="_blank">dash.py</a>,
but suffices to say that you find there 

- The logic to initially create the tables inside the [postgresql](../../infra/stacks/db.md) instance
- The logic to connect to the app.
- The logic to create initial user(s). Go re-read the ecodev-app intro documentation [introduction](../index.md) to see one way to create the initial user(s).
- The logic to define the app colors. Edit `APP_COLORS` as you see fit!
- The logic to be in debug mode or nor for the dash app. Controlled by the `DEBUG` flag setup via env variables. Go re-read the ecodev-app intro
- documentation [introduction](../index.md) if needs be.
- The logic to use either <a href=https://www.uvicorn.org/ class="external-link" target="_blank">uvicorn</a> or 
<a href=https://gunicorn.org/ class="external-link" target="_blank">gunicorn</a>. Controlled by the `DASH_APP_SETTINGS.gunicorn_setup` flag setup via env variables.

!!! warning
    We found scarcely few documentation on how to use gunicorn with dash. For this to work implied for us
    
    - Using `from flask import Flask` and then create the `Dash` app  with the `Flask(__name__)` instance
    - Instanciate the  `Dash` app like so 
        ```python
        if __name__ == '__main__':
            dash_app.run_server(host='0.0.0.0', port=80)
        ```
    - in the `docker-compose.yml` file, call
        ```yaml
        command: gunicorn app.dash_app:server --workers 4 --worker-class gevent  --bind 0.0.0.0:80
        ```
    <a href=http://www.gevent.org/ class="external-link" target="_blank">gevent</a> is the best thing we found to work with workers for Dash. 
    You can obviously change the number of `workers` as you see fit. 

!!! warning
    With gunicorn long (>30s dash callbacks with timeout. You should handle long callbacks in another fashion (which presumably will improve the UX of your application
    anyway 😋). You can call your backend and use a Fastapi 
<a href=https://fastapi.tiangolo.com/tutorial/background-tasks/ class="external-link" target="_blank">`BackgroundTasks`</a> there,
go nuts with <a href=https://docs.celeryq.dev/en/stable/  class="external-link" target="_blank">Celery</a>... Sky is the limit 😋.