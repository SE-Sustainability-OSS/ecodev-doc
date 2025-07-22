# Ecodev App 

This is the cornerstone of **Ecoact CDA team** Open source contribution: <a href=https://github.com/SE-Sustainability-OSS/ecodev-app class="external-link" target="_blank">ecodev-app</a>. 


<p align="center">
  <a href="/img/ecodev_app/ecodev_app_nutshell.png"><img src="/img/ecodev_app/ecodev_app_nutshell.png" alt="Ecodev app"></a>
</p>
<p align="center">
    <em>What you can get out of the box 😊</em>
</p>
<p align="center">
</p>

Heavily inspired by  <a href=https://github.com/fastapi/full-stack-fastapi-template class="external-link" target="_blank">Tiangolo full-stack FastAPI template</a>,
we wanted to provide something on the same vein but purely based on python. Plus when we started working on it, Tiangolo's work was not yet based on
<a href=https://sqlmodel.tiangolo.com/ class="external-link" target="_blank">SQLModel</a>, only on 
<a href=https://www.sqlalchemy.org/ class="external-link" target="_blank">SQL Alchemy</a>. 

That is why we use <a href=https://dash.plotly.com/ class="external-link" target="_blank">Dash</a> as our frontend tool, in order to only use python as programming 
language, a thing quite convenient <a href=https://www.tiobe.com/tiobe-index/ class="external-link" target="_blank">these days</a>.

In this part of the documentation, you will find details on what is inside ecodev-app.

We assume here that you have already read and/or have been redirected from [the setup from scratch part of the doc](../../setup-from-scratch/index.md), meaning you 
have an operational db.


## Usage

### 1. New repository

First create a `<new-repository>` in your GitHub, then follow these commands, replacing any mention of `<new-repository>`, with your repo name:

```bash
# Create new folder where you would like your repo to be located on your machine
mkdir <new-repository>; cd <new-repository>

# Make a bare clone of this repository
git clone https://github.com/SE-Sustainability-OSS/ecodev-app

# Move files to parent directory
cd .. ; mv <new-repository>/ecodev-app/* <new-repository>/

# Delete remaining ecodev-app files
cd <new-repository> ; rm -rf ecodev-app/

# Remove git file
rm -rf .git

# Create new .git file (and optionally rename master to main)
git init ; git branch -M main

# Add new remote
git remote add origin https://github.com/<user>/<new-repository>
```


### 2. Replace name

Search for and replace any references to `ecodev-app` and `ecodev_app` within the repo, with your own app name.

- `docker-compose.yml`
- `docker-compose.override.yml`
- `Makefile`

!!! warning
    Do keep the respective `-` and `_` used throughout


### 3. Update Readme

Update this `README.md` with your own. A template is provided in `README-template.md`

Feel free to remove any other markdown file (e.g. `SECURITY.md`, `CODE_OF_CONDUCT.md`, etc.) that are not applicable._


### 4. First commit

 Once done, create your first commit and push to your new repo, as you would normally do.
```bash
git add . ; git commit -m 'Initial commit'
git push origin main
```

<div class="termy">
```console
$ git add . 
$ git commit -m 'Initial commit'
$ git push origin main
---> 100%
Successfully initialized ecodev-app!
```
</div>

## Run

To run your app, follow the steps below (also available in the `README-template.md` file)

### 1. Env file

Create an `.env` file and save it under the application's root folder. It must contain the following (you have an `env_template` to help you):

```.env
# App name
app_name= template # Name of the app. Will appear on the app header.

# API app
app_port=8000 # API app port number. Remove this entry in production
fastapi_url= # API app web url. Fill only in production.

# Dash app
dash_app_port=8071 # Dash app port number. Remove this entry in production
dash_url= # Dash app web url.  Fill only in production.

# Jupyter app
jupyter_port=5000 # Jupyter notebook port number. Remove this entry in production

# Database access
db_port=5432 # Database port number
db_host=db # Database server name
db_name=db # Database name
db_username=user # Database login username. Change to a real username.
db_password=password # Database login password. Change to a secured password.

# Hashing secrets
secret_key=azertyuiopqdfghjklmwxcvbn134567890azertyuiopqsdfghjklmwxcvbn1234567890 # App secret key (for hashing purposed)
algorithm=HS256 # Password hashing algorithm

# Other
access_token_expire_minutes=1440 # App JWT access token expiry duration

# App mailbox
email_sender= # Mailbox email address. Put to the real email sender
email_password= # Mailbox password. Put to the real email sender password
email_smtp= # Mailbox SMTP (like smtp.gmail.com or mail.outlook.com). Put your smtp provider here

# gunicorn/uvicorn setup
gunicorn_setup=false # Should be True in production. Simpler to only with uvicorn for local dev
debug=false # Put to true if you want to be in dash debug mode
```

### 2. Setup pre-commits 

Run `make setup` to install the pre-commits

<div class="termy">
```console
$ make setup
---> 100%
Pre-commit installed!
```
</div>


### 3. Build Docker image

Build the docker image

<div class="termy">

```console
$ make dev-build
---> 100%
ecodev-app image built!
```

</div>

### 4. Setup pgadmin

Open your [local PGAdmin](../infra/stacks/db.md) and login using the admin credentials.

    ⚠️ You may encounter login issues if using Firefox browser.

### 5. db connect 
 
Connect to the database server using the admin credentials

### 6. Create new db 


Create a new database `<YOUR_DB_NAME>_db`, as per your .env file (`db`) in the example.

### 7. Create user

Add a user.json file under data, if you'd like to initialise a few users. It should contain:
    ```json
    [
        {
            "id": null,
            "user": "user_name",
            "password": "hashed_password",
            "permission": "permission_level",
            "client": "project_name",
            "application": "application_name"
        }
    ]
    ```


### 8. Actually launch the container! 😊

Run the docker container

<div class="termy">

```console
$ docker compose up -d
---> 100%
Successfully launched ecodev-app!
```
</div>

The app should be running under http://localhost: <YOUR_ENV_DASH_APP_PORT>:


To check for any issues, you can check the logs with:

```bash
docker logs <YOUR_APP_CONTAINER_NAME> --tail=100 -f
```

<div class="termy">
```console
$ docker logs ecodev-app --tail=100 -f
some logs
```
</div>

Use `Ctrl+C` to exit the logs