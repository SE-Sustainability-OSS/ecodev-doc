# MongoDB and MongoExpress

**Ecoact CDA team**  uses <a href=https://www.mongodb.com/ class="external-link" target="_blank">MongoDB</a> as its document oriented database, alongside with 
<a href=https://github.com/mongo-express/mongo-express class="external-link" target="_blank">mongo-express</a> to have admin access and handling of data. 

Contrary to relational database, NoSQL systems can handle non tabular data. For a brief overview of the differences, see <a href=https://www.mongodb.com/nosql-explained/nosql-vs-sql>this</a>.  
One kind of NoSQL databases are ,<a href=https://www.mongodb.com/document-databases>document databases</a>, that store structured document in format such as XML or JSON.  
MongoDB is one such document database.  

MongoExpress is a web-based MongoDB admin interface allowing you to access your documents collections through your browser. It is the MongoDB equivalent of pgAdmin. 
## Setting up the mongo-db stack

All relevant information can be found in the `docker-compose.mongo.yml` file (<a href=https://github.com/SE-Sustainability-OSS/ecodev-infra/blob/main/docker-compose.mongo.yml class="external-link" target="_blank">here</a>).

### Production mode

Remember that you need to setup traefik when in production before launching this stack. Go read [the traefik page](traefik.md) if not already done.

To start the stack in production, you can make use of the `Makefile` provided in the repo (`make help`) to list available commands.


you will need to create a `.env` <a href=https://www.codementor.io/@parthibakumarmurugesan/what-is-env-how-to-set-up-and-run-a-env-file-in-node-1pnyxw9yxj  class="external-link" target="_blank">file</a> and setting


- `mongoexpress_url` entry to the DNS address you created in order to reach the mongo-express interface. 
- `mongo_username`: the user authorized to connect to the database
- `mongo_password`:  the password associated with `mongo_username`


You can then safely launch 
```shell
make mongo-launch
```

mongo-express will be accessible at `mongoexpress_url`

