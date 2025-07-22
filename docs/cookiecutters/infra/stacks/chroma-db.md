# ChromaDB

**Ecoact CDA team**  uses <a href=https://www.trychroma.com class="external-link" target="_blank">ChromaDB</a> as its vector store.

Contrary to relational database, NoSQL systems can handle non tabular data. For a brief overview of the differences, see <a href=https://www.mongodb.com/nosql-explained/nosql-vs-sql class="external-link" target="_blank">this</a>.   
 One kind of NoSQL databases are <a href=https://www.cloudflare.com/learning/ai/what-is-vector-database class="external-link" target="_blank"> vector stores</a>. As its names implies, a vector store is used to store vectors with associated metadata.  
It also provides the ability to efficiently apply similarity search on vectors and is therefore really useful when working with text embeddings, especially for semantic search applications using dense representations.


## Setting up the chroma-db stack

All relevant information can be found in the `docker-compose.chroma.yml` file (<a href=https://github.com/SE-Sustainability-OSS/ecodev-infra/blob/main/docker-compose.chroma.yml class="external-link" target="_blank">here</a>).

### Production mode

Remember that you need to setup traefik when in production before launching this stack. Go read [the traefik page](traefik.md) if not already done.

To start the stack in production, you can make use of the `Makefile` provided in the repo (`make help`) to list available commands.


you will need to create a `.env` <a href=https://www.codementor.io/@parthibakumarmurugesan/what-is-env-how-to-set-up-and-run-a-env-file-in-node-1pnyxw9yxj  class="external-link" target="_blank">file</a> and setting


- `chroma_username`: the user authorized to connect to the database
- `chroma_password`:  the password associated with `chroma_username`
- `chroma_db_path`:  the folder in the host where the database will be stored


You can then safely launch 
```shell
make chroma-launch
```


