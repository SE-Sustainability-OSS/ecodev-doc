# Dozzle

 <a href=https://dozzle.dev/ class="external-link" target="_blank">Dozzle</a> is a log aggregation tool. It
also allows to do basic search on these logs. The main advantage when compared to the numerous 
competitors (ElasticSearch stack with logstash, logspout...) is its ridiculously simple setup, plus 
 it's smooth UI/UX experience (do one thing but does it well).
 
## Setting up the dozzle stack 

Dozzle being directly plugged (like traefik) to the docker engine running on the host VM, there is not much to do to set it up.

All relevant information can be found in the `docker-compose.dozzle.yml` file (
<a href=https://github.com/SE-Sustainability-OSS/ecodev-infra/blob/main/docker-compose.dozzle.yml class="external-link" target="_blank">here</a>).

!!! note
    direct plugin comes in the mounted volume `/var/run/docker.sock:/var/run/docker.sock:ro`. In addition, there 
    are two optional and one mandatory environment variables that you can put inside a `.dozzle.env` file:

    - `DOZZLE_ENABLE_ACTIONS` (optional) controls wether the dozzle web interface is allowed to rstart/stop containers. It is `false` by default
    - `DOZZLE_NO_ANALYTICS` (optional) controls whether anonymous information are sent to google analytics or not. It is `true` by default.
       This helps dozzle creator to prioritize new feature, but disactivate it if full privacy is a concern.
    - `dozzle_url` (mandatory) corresponds to the DNS address you created in order to reach the dozzle interface 

To start the stack in production, you can make use of the `Makefile` provided in the repo (`make help`) to list available commands. Launch

```shell
make dozzle-launch
```

<div class="termy">
```console
$ make dozzle-launch
---> 100%
Dozzle successfully setup!
```
</div>

Then you can go to `dozzle_url`. The web interface is so simple to use that explanations are barely needed! 😲

Would you need more information, consult the  <a href=https://dozzle.dev/guide/what-is-dozzle class="external-link" target="_blank">official documentation</a>. 