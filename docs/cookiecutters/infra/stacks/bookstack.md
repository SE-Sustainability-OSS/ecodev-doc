# Bookstack

 <a href=https://www.bookstackapp.com/ class="external-link" target="_blank">Bookstack</a>  is a self-hosted open source solution to create documentation. 
 
As [Devs](../../../contact_us.md), we do prefer  <a href=https://squidfunk.github.io/mkdocs-material/ class="external-link" target="_blank">the tool</a> that we 
use to write this very documentation 😅. But it turns out that non technical profiles do not share our enthousiasm on writing markdown and deploying via github/docker compose 😋.

Bookstack thus comes handy as once deployed anybody (with the right authorization) can write and update content.

## Setting up the bookstack stack 

There is no sense in setting up locally Bookstack, we thus just present the production setup. 

All relevant information can be found in the `docker-compose.bookstack.yml` file (<a href=https://github.com/SE-Sustainability-OSS/ecodev-infra/blob/main/docker-compose.bookstack.yml class="external-link" target="_blank">here</a> ).

You will need to **create two folders** in the folder containing `docker-compose.bookstack.yml`

- `bookstack_app_data`
- `bookstack_storage_data`

!!! warning
    Be sure to give write access to the external world (<a href=https://doc.ubuntu-fr.org/permissions  class="external-link" target="_blank">chmod</a> 755 presumably) for these folders.
    Otherwise you won't be able to upload documents. We lost some time on that 😅

you will need to **create a `.bookstack.env`** <a href=https://www.codementor.io/@parthibakumarmurugesan/what-is-env-how-to-set-up-and-run-a-env-file-in-node-1pnyxw9yxj  class="external-link" target="_blank">file</a> and setting

- `MYSQL_USER`: username for the mySQL `bookstack_db` container
- `MYSQL_PASSWORD`: password associated to `MYSQL_USER`
- `MYSQL_ROOT_PASSWORD`: password for the mySQL `bookstack_db` container
- `MYSQL_DATABASE`: the db name to which your `bookstack` container will connect to
- `APP_URL`: entry to the DNS address you created in order to reach the bookstack interface. 
- `DB_DATABASE`: put the same value as in `MYSQL_DATABASE`
- `DB_HOST`: put here `bookstack_db:3306`
- `DB_USERNAME`: put the same value as in `MYSQL_USER`
- `DB_PASSWORD`: put the same value as in `MYSQL_PASSWORD`
- `APP_KEY`: As explained  <a href=https://github.com/solidnerd/docker-bookstack class="external-link" target="_blank">here</a>
- `bookstack_url`:  put the same value as in `APP_URL`

!!! warning 
    `APP_KEY` must be some random string with **exactly** 32 characters (we lost some time on that 😅).

To start the stack in production, you can make use of the `Makefile` provided in the repo (`make help` to list available commands). Launch

```shell
make bookstack-launch
```

pgAdmin will be accessible at `bookstack_url`. Please refer to the  <a href=https://www.bookstackapp.com/ class="external-link" target="_blank">official documentation</a> to create new users, contents...