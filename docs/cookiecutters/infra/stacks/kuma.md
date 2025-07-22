# Uptime-kuma

 <a href=https://github.com/louislam/uptime-kuma class="external-link" target="_blank">uptime-kuma</a> is a very basic though marvelously simple monitoring and alerting tool.
This is the reason we picked it over numerous over competitors (on top of which we surely find <a href=https://prometheus.io/ class="external-link" target="_blank">Prometheus</a>/<a href=https://grafana.com/ class="external-link" target="_blank">Grafana</a> stack).


It is also ridicuslously simple to setup, as we're going to see 😊.
 
## Setting up the uptime-kuma stack 

All relevant information can be found in the `docker-compose.kuma.yml` file (<a href=https://github.com/SE-Sustainability-OSS/ecodev-infra/blob/main/docker-compose.kuma.yml class="external-link" target="_blank">here</a>).

Only one `uptime_kuma_data` volume is needed for `uptime-kuma` to store it's information. 

Optional environment variables (documentation <a href=https://github.com/louislam/uptime-kuma/wiki/Environment-Variables class="external-link" target="_blank">here</a>) should be put in an `.kuma.env` file. There also put one mandatory variable:

- `kuma_url` corresponds to the DNS address you created in order to reach the kuma interface 

To start the stack in production, you can make use of the `Makefile` provided in the repo (`make help`) to list available commands. Launch

```shell
make kuma-launch
```

<div class="termy">
```console
$ make kuma-launch
---> 100%
Uptime-kuma successfully setup!
```
</div>

Then just go to `kuma_url` and enjoy the smooth experience. Would you need some additional information, you can
 
- watch this 5 minutes <a href=https://www.youtube.com/watch?v=muZiPdH2JZ8 class="external-link" target="_blank">video</a>
- read the official (short) <a href=https://github.com/louislam/uptime-kuma/wiki class="external-link" target="_blank">documentation</a>