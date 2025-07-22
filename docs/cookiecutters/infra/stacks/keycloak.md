# Keycloak authentication 

At EcoAct, we prefer when our applications are responsible for both authentication and authorization (simpler given our small scale). 
But it happens from time to time when an app is not able to provide for these, especially when it has not been coded by us 😁.

A good example of this is  <a href=https://jupyter.org/ class="external-link" target="_blank">jupyter notebooks</a>, that in its basic form does not come (to the 
best of our knowledge) with a fine-grained authentication/authorization mechanism. 

To circumvent this limitation, we use <a href=https://www.keycloak.org/ class="external-link" target="_blank">keycloak</a>, alongside with the 
 <a href=https://doc.traefik.io/traefik/middlewares/http/forwardauth/ class="external-link" target="_blank">traefik forward-auth</a> middleware.

!!! warning 
    here we clearly see the limits of the homemade approach. A managed jupyter service would be a judicious choice to consider 😊.

## Setting up the keycloak stack 

There is no sense in setting up locally keycloak, we thus just present the production setup. 

All relevant information can be found in the `docker-compose.keycloak.yml` file (<a href=https://github.com/SE-Sustainability-OSS/ecodev-infra/blob/main/docker-compose.keycloak.yml class="external-link" target="_blank">here</a>).


You will need to **create a `.keycloak.env`** <a href=https://www.codementor.io/@parthibakumarmurugesan/what-is-env-how-to-set-up-and-run-a-env-file-in-node-1pnyxw9yxj  class="external-link" target="_blank">file</a> 
and setting for keycloak

- `DB_ADDR`: should be `db` (the [postgresql](db.md) database is used for keycloak)
- `DB_VENDOR`: should be `POSTGRES`
- `DB_DATABASE`: name of the database inside `db`
- `DB_USER`: the authorized user for `db` (see `POSTGRES_USER` in  [postgresql](db.md))
- `DB_PASSWORD`: password associated with `DB_USER` (see `POSTGRES_PASSWORD` in  [postgresql](db.md))
- `DB_SCHEMA`: should be `public`
- `KEYCLOAK_USER`: username to accesss (with admin right) to keycloak web interface
- `KEYCLOAK_PASSWORD`: password associated with `KEYCLOAK_USER`
- `PROXY_ADDRESS_FORWARDING`: put `true`. This is required to run keycloak behind traefik
- `keycloak_url`: entry to the DNS address you created in order to reach the keycloak web interface. 
- `KEYCLOAK_HOSTNAME`:  put the same value as in `keycloak_url`


You can then launch for a first time

```shell
make keycloak-launch
```

Once in the keycloak web interface, pick admin console, you should be in the (default) realm called master. Create a new client like so

<p align="center">
  <a href="/img/ecodev_infra/keycloak_client.png"><img src="/img/ecodev_infra/keycloak_client.png" alt="Create a new client"></a>
</p>
<p align="center">
    <em>Create a new client</em>
</p>
<p align="center">
</p>


<p align="center">
  <a href="/img/ecodev_infra/keycloak_client_credentials.png"><img src="/img/ecodev_infra/keycloak_client_credentials.png" alt="secret key used to generate JWT tokens"></a>
</p>
<p align="center">
    <em>Create a secret key used to generate JWT tokens</em>
</p>
<p align="center">
</p>

<p align="center">
  <a href="/img/ecodev_infra/keycloak_settings_1.png"><img src="/img/ecodev_infra/keycloak_settings_1.png" alt="Fill client settings (part 1)"></a>
</p>
<p align="center">
    <em>Fill client settings (part 1)</em>
</p>
<p align="center">
</p>


<p align="center">
  <a href="/img/ecodev_infra/keycloak_settings_2.png"><img src="/img/ecodev_infra/keycloak_settings_2.png" alt="Fill client settings (part 2)"></a>
</p>
<p align="center">
    <em>Fill client settings (part 2)</em>
</p>
<p align="center">
</p>

Then in the  `.keycloak.env`, fill the needed traefik forward auth environment variables

- `PROVIDERS_OIDC_CLIENT_ID`: see above screenshot (client/app to protect)
- `DEFAULT_PROVIDER`: put `oidc`
- `SECRET`: see above screenshot
- `PROVIDERS_OIDC_CLIENT_SECRET`: see above screenshot (same as `SECRET`)
- `OIDC_ISSUER`: see above screenshot and put `URL/auth/realms/master`. `URL` is the DNS address you created in order to reach the application you want to protect. 
- `PROVIDERS_OIDC_ISSUER_URL`: same as `OIDC_ISSUER`
- `AUTH_HOST`: see above screenshot and put `URL/_oauth`
- `COOKIE_DOMAIN`: the domain name of `URL` (like lcabox.com for this very website)

In the stack of the app you want to protect, you will need to add in the `labels` section 

```yml
- traefik.http.routers.jupyter-https.middlewares=traefik-forward-auth
```

Finally, you can stop and relaunch the app you want to protect and

```shell
make keycloak-launch
```

The app should be protected by keycloak! 😊
