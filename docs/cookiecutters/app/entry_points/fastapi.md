# Fastapi Module

This is the main entry point for the backend. You should not interact that often with 
this technical <a href=https://github.com/SE-Sustainability-OSS/ecodev-app/blob/main/app/app.py class="external-link" target="_blank">app.py</a>,
but suffices to say that you find there 

- The instantiation of the FastAPI `FastAPI()` app
- The instantiation of the sqladmin `Admin` admin interface (with a custom authentication based on JWT, as for the rest of the stack authentication mechanisms)
- A `login` route to... Well, login 😅