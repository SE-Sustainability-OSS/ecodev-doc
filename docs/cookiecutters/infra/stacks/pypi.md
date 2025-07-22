# Private pypi 
 
As this documentation and the associated code repositories can assess, <a href=https://eco-act.com/ class="external-link" target="_blank">EcoAct</a> fully embraces Open Source.

Nevertheless, for critical private domain model knowledge, it turns useful to have a private library hub in order to share the domain model between your different applications.

!!! note 
    There are numerous cloud services that will offer (well not offer, but you see what we mean 😁) you this library hosting functionality. 
    You can (and should once mature enough) stick with them. But some of the <a href=https://python-poetry.org/ class="external-link" target="_blank">poetry</a> 
    knowledge presented here might still prove useful to you! 😊

We will use <a href=https://github.com/pypiserver/pypiserver class="external-link" target="_blank">pypiserver</a> to do this hosting.

## Setting up the private pypi stack 

There is no sense in setting up locally pypiserver, we thus just present the production setup. 

All relevant information can be found in the `docker-compose.pypi.yml` file (<a href=https://github.com/SE-Sustainability-OSS/ecodev-infra/blob/main/docker-compose.pypi.yml class="external-link" target="_blank">here</a>).

To start the stack in production, you can make use of the `Makefile` provided in the repo (`make help`) to list available commands.

To use it you will need to **create two folder** in the folder containing `docker-compose.pypi.yml`

- `pypi_data`: this will host the `.tar.gz` and the `.whl` files containing your library code. 
- `pypi_auth`: this will contain the `.htpasswd` file(s) used for authentication 

you will need to create a `.pypi.env` <a href=https://www.codementor.io/@parthibakumarmurugesan/what-is-env-how-to-set-up-and-run-a-env-file-in-node-1pnyxw9yxj  class="external-link" target="_blank">file</a> and setting

- `pypi_url` entry to the DNS address you created in order to reach the pypi web interface.  

To use htpasswd needed for pypi authentication, install 
```shell
sudo apt-get install apache2-utils
```
then run **in the `pypy_auth` folder**

```properties
 htpasswd -sc .htpasswd <USER>
```

You will be prompted for a `<PASSWORD>`. Note that **poetry upload will not work if you use any
<a href=https://github.com/python-poetry/poetry/issues/1999 class="external-link" target="_blank">non bash interpretable characters in your password</a>**.

You can then safely launch 
```properties
make pypi-launch
```

your pypi will be accessible at `pypi_url`.

<a href=https://testdriven.io/blog/private-pypi/ class="external-link" target="_blank">Useful resource on the topic</a> (we already mentioned that we like this website, didn't we? 😊).

## Uploading a package with poetry 

We assume that you have basic knowledge of poetry. If you do not, check out
-  <a href=https://www.youtube.com/watch?v=0f3moPe_bhk&t=104s&ab_channel=ArjanCodes class="external-link" target="_blank">This</a> Arjan video 
- The <a href=https://python-poetry.org/ class="external-link" target="_blank">official documentation</a> (for dependancies <a href=https://python-poetry.org/docs/dependency-specification/ class="external-link" target="_blank">this reading</a>  should not be superfluous)

In the code repository containing your `pyproject.toml` file, run 
```properties
poetry config repositories.<your_package_name> `pypi_url`
poetry config http-basic.<your_package_name> '<USER>' `<PASSWORD>`
```
This will point your package upload mechanism towards `pypi_url`. Then once you are satisfied with your code modifications, run 
```properties
# automatically bump minor version
poetry version patch
# build new package version
poetry build
# upload <your_package_name> to `pypi_url`
poetry publish --repository <your_package_name>
```
## Installing a package in a docker image. 

It turns out to be not so trivial to pass an environment variable to a dockerfile (in order not to commit our '<USER>' and `<PASSWORD>`...). 

The best way we found to do this relies on <a href=https://docs.docker.com/engine/reference/commandline/compose_build/ class="external-link" target="_blank">docker-compose build</a>.

in your application docker compose service stack, add the following section 

```yaml
build:
  context: .
  dockerfile: ./<DOCKERFILE>
  args:
    PIP_EXTRA_INDEX_URL: ${PIP_EXTRA_INDEX_URL}
env_file:
  - ./.pypi.env
```

You will have to add to your `.pypi.env` the following variable: 

- `PIP_EXTRA_INDEX_URL`: takes the form `https://<USER>:<PASSWORD>@<pypi_dns>`. 
   For example, if you host your private pypi at `https://myawesomepypi.com`, and you chose 
  `user` `password` as user/password (very bad choice 😁), then put `PIP_EXTRA_INDEX_URL=https://user:password@myawesomepypi.com`

Then in your `<DOCKERFILE>`:

- add after your `FROM` instruction : `ARG PIP_EXTRA_INDEX_URL`. To learn more on `ARG` and `ENV`, go <a href=https://vsupalov.com/docker-arg-env-variable-guide/ class="external-link" target="_blank">here</a>  
- add the following option to the line doing the `pip install`: 

  ```yaml
  --extra-index-url PIP_EXTRA_INDEX_URL
  ```



Finally, you can update your `requirements.txt` file, adding to it `<your_package_name>` (and do not forget to allow for automatic minor updates, putting `==0.*` for instance
if your package is in major `0`). 

You should then build the docker image in the following way

```property
docker compose build
```