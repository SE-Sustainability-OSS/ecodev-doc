# PostgreSQL and pgAdmin

**Ecoact CDA team**  uses <a href=https://www.postgresql.org/ class="external-link" target="_blank">PostgreSQL</a> as its SQL database, alongside with 
<a href=https://www.pgadmin.org/ class="external-link" target="_blank">pgAdmin</a> to have admin access and handling of data. 

If you know nothing about <a href=https://fr.wikipedia.org/wiki/Structured_Query_Language class="external-link" target="_blank">SQL</a> 
(unlikely but hey, everybody has the right to start somewhere! 😊), you can learn by playing  <a href=https://sql-island.informatik.uni-kl.de/ class="external-link" target="_blank">here</a> 😅.

If you want to have a bird eye view of common SQL use cases when working with python, the SQLModel 
<a href=https://sqlmodel.tiangolo.com/databases/ class="external-link" target="_blank">documentation</a> is as often an awesome resource.

## Setting up the db stack

All relevant information can be found in the `docker-compose.yml` file (<a href=https://github.com/SE-Sustainability-OSS/ecodev-infra/blob/main/docker-compose.yml class="external-link" target="_blank">here</a>).

### Production mode

Remember that you need to setup traefik when in production before launching this stack. Go read [the traefik page](traefik.md) if not already done.

To start the stack in production, you can make use of the `Makefile` provided in the repo (`make help`) to list available commands.

To use it you will need to **create one folder** in the folder containing `docker-compose.yml`

- `db_data`

!!! note
    The `db` container will itself handle user/group profile on this folder. You might see a funky `70` user, not listed in `/etc/group`. This is normal, it corresponds to a user 
    inside the `db` container, unknown to the host VM (technical parenthesis that gave us some headache at some point closed 😂)

you will need to create a `.env` <a href=https://www.codementor.io/@parthibakumarmurugesan/what-is-env-how-to-set-up-and-run-a-env-file-in-node-1pnyxw9yxj  class="external-link" target="_blank">file</a> and setting

- `pgadmin_url` entry to the DNS address you created in order to reach the pgadmin interface. 
- `POSTGRES_DB`: the db name to which your application will connect to 
- `POSTGRES_USER`: the user authorized to connect to `POSTGRES_DB`
- `POSTGRES_PASSWORD`:  the password associated to `POSTGRES_USER`
- `PGADMIN_DEFAULT_EMAIL`: the username to connect to the pgAdmin web interface (accessible at `pgadmin_url`)
- `PGADMIN_DEFAULT_PASSWORD`:  the password to connect to the pgAdmin web interface (accessible at `pgadmin_url`)

You can then safely launch 
```shell
make db-launch
```

<div class="termy">
```console
$ make db-launch
---> 100%
Production DB successfully setup!
```
</div>

pgAdmin will be accessible at `pgadmin_url`

### Local dev mode

In this case you will make use of the additional  `docker-compose.override.yml` file. Here you will set in your `.env` **all parameters of the previous section** (bare 
`pgadmin_url`, since traefik is not used locally) plus 
- `pgadmin_port`: the local port on which you want to access to pgAdmin. If not set, 5050 is taken by default. 

Then launch
```shell
make db-dev-launch
```

<div class="termy">
```console
$ make db-dev-launch
---> 100%
Local dev DB successfully setup!
```
</div>

pgAdmin will be accessible at `http://127.0.0.1:pgadmin_port`