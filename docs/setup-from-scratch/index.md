# Setup from scratch

Here we will got through each step of the setup of the ecodev ecosystem, so that you may install it from scratch, 
either on your machine or "in production" (e.g. a remote Virtual Machine (VM)).

## Dev setup 

### Requirements

To work locally, you need  <a href=https://www.docker.com/ class="external-link" target="_blank">docker</a> installed and

- A database. We typically work with <a href=https://www.postgresql.org/ class="external-link" target="_blank">PostgreSQL</a>, but any SQL database (e.g. SQLite) will suffice.
- an admin interface to handle the db (you could work without it, but it is such a time saviour in our experience. 
We adopt here <a href=https://www.pgadmin.org/ class="external-link" target="_blank">pgAdmin</a>)
- An application 😋: this is the raison d'être of [ecodev-app](../cookiecutters/app/index.md)! 
So instead of creating your own one from scratch, start from `ecodev-app` as explained below.


## How to

- 1) To setup the db and the admin interface, follow [this part of the documentation](../cookiecutters/infra/stacks/db.md#local-dev-mode). Take the environment variables
from the production setup and then skip to the dev part.
- 2) follow [this section](../cookiecutters/app/index.md#usage) and then [this section](../cookiecutters/app/index.md#run) of the ecodev-app documentation.

You should then have a locally working ecodev-app plugged into it's sql db! Congrats! 🥰

## Prod setup

### Requirements

To work on a remote VM, you need

- a VM (duh 😅)
- A reverse-proxy: our choice goes for  <a href=https://doc.traefik.io/traefik/ class="external-link" target="_blank">Traefik</a>.
- a db: our choice goes for <a href=https://www.postgresql.org/ class="external-link" target="_blank">PostgreSQL</a>
- an admin interface to handle the db (you could work without it, but it is such a time saviour in our experience. 
We adopt here <a href=https://www.pgadmin.org/ class="external-link" target="_blank">pgAdmin</a>)
- monitoring tools. As discussed elsewhere in this documentation, we favored simplicity over the popular prometheus/grafana stack: else we advocate 

     - <a href=https://github.com/louislam/uptime-kuma class="external-link" target="_blank">uptime-kuma</a> for alerting and
     - <a href=https://dozzle.dev/ class="external-link" target="_blank">Dozzle</a> for monitoring.
  
- An application 😋: this is the raison d'être of [ecodev-app](../cookiecutters/app/index.md)!

### How to

- 1) Setup your VM with Ubuntu (rent it from your favourite cloud provider and follow their setup tutorials), then install `ecodev-infra` required OS packages 
(mostly <a href=https://www.docker.com/ class="external-link" target="_blank">docker</a>) using 
[this ecodev-infra documentation page](../cookiecutters/infra/setup/setup_ubuntu_vm.md).
- 2) We strongly encourage you to read this [ecodev-infra documentation on security](../cookiecutters/infra/security.md).
- 3) To setup traefik, go read [this](../cookiecutters/infra/stacks/traefik.md)
- 4) To setup the db and the admin interface, follow [this part of the documentation](../cookiecutters/infra/stacks/db.md). Skip the last section (relate to local deployment)
- 5) To setup the monitoring/alerting tools, go read [this](../cookiecutters/infra/stacks/kuma.md) and [this](../cookiecutters/infra/stacks/dozzle.md).
- 6) follow [this section](../cookiecutters/app/index.md#usage) and then [this section](../cookiecutters/app/index.md#run) of the ecodev-app documentation.

You should have a working, secured application on the cloud! 😍. To update it, simply run `git pull` when needs be (and do not forget to rebuild the image first if you updated the 
dependencies since last build! 😊)