# Ecodev-infra

## Purpose

This repo suggests an architecture based on

- <a href=https://doc.traefik.io/traefik/ class="external-link" target="_blank">Traefik</a>
-  <a href=https://www.postgresql.org/ class="external-link" target="_blank">PostgreSQL</a> db and <a href=https://www.pgadmin.org/ class="external-link" target="_blank">pgAdmin</a> interface
- <a href=https://www.trychroma.com/ class="external-link" target="_blank">ChromaDB</a> vector store 
- <a href=https://www.mongodb.com/ class="external-link" target="_blank">MongoDB</a> document database and its associated <a href=https://github.com/mongo-express/mongo-express class="external-link" target="_blank">mongoExpress</a> interface
- <a href=https://github.com/pypiserver/pypiserver class="external-link" target="_blank">Private pypi</a>
- <a href=https://www.bookstackapp.com/ class="external-link" target="_blank">Bookstack</a> and its associated db
- <a href=https://www.keycloak.org/ class="external-link" target="_blank">keycloak</a> and  <a href=https://doc.traefik.io/traefik/middlewares/http/forwardauth/ class="external-link" target="_blank">traefik forward-auth</a>   (to protect apps that cannot implement authentication without keycloak)
- <a href=https://github.com/mcuadros/ofelia class="external-link" target="_blank">Ofelia</a>
- <a href=https://min.io/ class="external-link" target="_blank">minio</a>  (back and front)
- <a href=https://dozzle.dev/ class="external-link" target="_blank">dozzle</a>
- <a href=https://github.com/louislam/uptime-kuma class="external-link" target="_blank">uptime-kuma</a>
- <a href=https://www.elastic.co/elasticsearch class="external-link" target="_blank">ElasticSearch</a>

<p align="center">
 <a href="/img/ecodev_infra/ecodev_infra.png"><img src="/img/ecodev_infra/ecodev_infra.png" alt="ecodev infra"></a>
</p>
<p align="center">
    <em>Ecodev infra in a nutshell</em>
</p>
<p align="center">
</p>

!!! warning
    The suggested infrastructure is **very** basic. It is 
    (in our opinion) only suitable for personal projects and small teams/startups
    in their infancy. 
    As soon as you have grown enough, you should switch to a managed service approach, being
    it via cloud giants like <a href=https://aws.amazon.com/ class="external-link" target="_blank">AWS</a>,
    <a href=https://www.youtube.com/@ArjanCodes class="external-link" target="_blank">Azure</a>,
    <a href=https://azure.microsoft.com/ class="external-link" target="_blank">GCP</a> or with open source managed services.
    We can for instance highly recommand <a href=https://elest.io/ class="external-link" target="_blank">elestio</a>, thanks to which we discovered 
    <a href=https://www.bookstackapp.com/ class="external-link" target="_blank">Bookstack</a> (that we use)
    and <a href=https://uptime.kuma.pet/ class="external-link" target="_blank">uptime-kuma</a> (that we enjoyed testing)   

!!! warning
    the `docker compose` files are **NOT** independant from each other. 
    You **MUST** `make trafik-launch` first, then `make db-launch`. The other `compose` stacks can 
    be launched (or not) when needed. 

    For this reason, we strongly advise reading the `traefik` documentation first, then the `db`. You can read on the remaining stacks when needed.

## Dependencies 

!!! warning
    The stacks are not independent. You **must** 

    - start the [traefik](stacks/traefik.md) stack first.
    - start the [db](stacks/db.md) stack second.
    - the other stacks can be launched upon need: 

      - [bookstack](stacks/bookstack.md)
      - [keycloak](stacks/keycloak.md)
      - [minio](stacks/minio.md)
      - [ofelia](stacks/ofelia.md)
      - [pypi](stacks/pypi.md)
      - [dozzle](stacks/dozzle.md)
      - [uptime-kuma](stacks/kuma.md)
      - [chroma-db](stacks/chroma-db.md)
      - [mongo-db](stacks/mongo-db.md)
      - [elasticsearch](stacks/elasticsearch-db.md)

<p align="center">
 <a href="/img/ecodev_infra/stack_dependencies.png"><img src="/img/ecodev_infra/stack_dependencies.png" alt="Stack dependencies"></a>
</p>
<p align="center">
    <em>Stack dependencies</em>
</p>
<p align="center">
</p>