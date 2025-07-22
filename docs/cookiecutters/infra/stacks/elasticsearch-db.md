# ElasticSearch

**Ecoact CDA team**  uses <a href=https://www.elastic.co/elasticsearch class="external-link" target="_blank">ElasticSearch</a> as its search engine.

This tool allows to index documents and apply near real-time full-text search on those indexed documents. We only use this tool but a full elastic stack could also includes its associated analytics platform, <a href=https://www.elastic.co/kibana class="external-link" target="_blank">Kibana</a>



## Setting up the es-db stack

All relevant information can be found in the `docker-compose.elasticsearch.yml` file (<a href=https://github.com/SE-Sustainability-OSS/ecodev-infra/blob/main/docker-compose.elasticsearch.yml class="external-link" target="_blank">here</a>).

### Production mode

To start the stack in production, you can make use of the `Makefile` provided in the repo (`make help`) to list available commands.


you will need to create a `.env` <a href=https://www.codementor.io/@parthibakumarmurugesan/what-is-env-how-to-set-up-and-run-a-env-file-in-node-1pnyxw9yxj  class="external-link" target="_blank">file</a> and setting


- `ELASTIC_PASSWORD`: the password associated with the `elastic` user
- `ES_MEM_LIMIT`:  the total memory assigned to the ElasticSearch container. 
- `ES_PORT`:  the port on which the container can be reached
- `ES_DATA`: the path where elastic search data will be stored

You can then safely launch 
```shell
make es-launch
```


