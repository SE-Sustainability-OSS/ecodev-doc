# Ofelia

 <a href=https://github.com/mcuadros/ofelia class="external-link" target="_blank">Ofelia</a> is a docker
 <a href=https://en.wikipedia.org/wiki/Cronjob class="external-link" target="_blank">cron</a> scheduler.
The main advantage when compared to the regular good old cron is that it is embbedded in the `docker-compose` stacks (thus version controlled), easing the deployment of a new stack
 (less manual operation to do on the host VM). Ofelia allows for  scheduling job 

- on the host VM (`job-local`)
- inside a running container (`job-exec`)

and much more 😊. We use it in conjunction with <a href=https://typer.tiangolo.com/ class="external-link" target="_blank">typer</a> (another Tiangolo marvel? No way! 😍) to periodically
launch python commands.

!!! note 
    if you are new to cron, you won't like its way of specifying time periods (or you would be just the one!).
    A good ressouce to help you  <a href=https://crontab.guru/ class="external-link" target="_blank">here</a>.
    To be noted, Ofelia uses a CRON expression with 6 component (the 6th for seconds!).
 
## Setting up the ofelia stack 

Ofelia being directly plugged (like traefik) to the docker engine running on the host VM, there is not much to do to set it up.

!!! note
    direct plugin comes in the mounted volume `/var/run/docker.sock:/var/run/docker.sock:ro`

All relevant information can be found in the `docker-compose.ofelia.yml` file (<a href=https://github.com/SE-Sustainability-OSS/ecodev-infra/blob/main/docker-compose.ofelia.yml class="external-link" target="_blank">here</a>).

To start the stack in production, you can make use of the `Makefile` provided in the repo (`make help`) to list available commands. Launch

```shell
make ofelia-launch
```

In the docker compose application stack where you want to launch a job, add these kind of lines 

```yml
- ofelia.enabled=true
# all 8th of each month at 00:00:01 
- ofelia.job-exec.datecron.schedule=0 0 8 * * 1 
# The actual command to launch 
- ofelia.job-exec.datecron.command=python -m TYPER_LAUNCH
```
