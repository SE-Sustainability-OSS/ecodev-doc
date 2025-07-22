# PostGIS and pgAdmin

**Ecoact CDA team** uses (among other things) <a href=https://postgis.net/ class="external-link" target="_blank">Postgis</a> to interact
with geospatial data.

## What for?

Postgis essentially adds two new data types:

- <a href=http://postgis.net/workshops/postgis-intro/geometries.html class="external-link" target="_blank">Geometry</a>: aimed at describing a local metric coordinate system (like
the different <a href=https://en.wikipedia.org/wiki/Universal_Transverse_Mercator_coordinate_system  class="external-link" target="_blank">UTM</a> coordinate systems). 
- <a href=http://postgis.net/workshops/postgis-intro/geography.html class="external-link" target="_blank">Geography</a>: aimed at describing a large portion of the world.

This drastically eases all geospatial computation, that can now be done directly withing sql queries instead of relying on python libraries like
<a href=https://shapely.readthedocs.io/en/stable/manual.html  class="external-link" target="_blank">shapely</a>.

In particular, operations that require a spatial indexing and that could have relied on shapely <a href=https://shapely.readthedocs.io/en/stable/strtree.html  class="external-link" target="_blank">STRTree</a> can now direclty be done by the 
database, with the help of the <a href=https://geoalchemy-2.readthedocs.io/en/latest/  class="external-link" target="_blank">GeoAlchemy 2</a> packages.
This improves RAM consumption by a more than significant amount (not having to load all the geographies in memory).

Simple python example compatible with <a href=https://sqlmodel.tiangolo.com/ class="external-link" target="_blank">SQLModel</a>.

We first define a `WorldPoint` class like so

!!! info
    note the need of `model_config`


```python
"""
Module implementing the world point db associated class
"""
from geoalchemy2 import Geography
from pydantic import ConfigDict
from sqlmodel import Column
from sqlmodel import Field
from sqlmodel import SQLModel

class WorldPointBase(SQLModel):  # type: ignore
    """
    a World Point.

    Attributes are:
        - point_id: a unique point identifier
        - location: the geometrical definition of the point

    NB: model_config has to be use because of geoalchemy2 Geography
    """
    point_id: int = Field(index=True)
    model_config = ConfigDict(from_attributes=True, arbitrary_types_allowed=True)
    location: Geography = Field(sa_column=Column(Geography(geometry_type='POINT', srid=4326)))


class WorldPoint(WorldPointBase, table=True):  # type: ignore
    """
    DB version of a world point.
    """
    __tablename__ = 'world_point'
    id: int | None = Field(default=None, primary_key=True)
```

and can then use it like so (having inserted into db points every 0.1 lat/lon):

```python
%%time
from geoalchemy2 import WKTElement
from geoalchemy2 import functions
from ecodev_core import engine
from sqlmodel import select
from sqlmodel Session
from geoalchemy2.comparator import Comparator
from geoalchemy2.shape import to_shape

with Session(engine) as session:
    query = select(functions.ST_AsText(WorldPoint.location)).order_by(
        Comparator.distance_centroid(
            WorldPoint.location,
            from_shape(Point(4.03,45.57)))).limit(1)
    result = to_shape(WKTElement(session.exec(query).first()))
print(result, type(result))
```

Giving the result 

```shell
POINT (4 45.6) <class 'shapely.geometry.point.Point'>
CPU times: user 2.9 ms, sys: 0 ns, total: 2.9 ms
Wall time: 3.05 ms
```

Other interesting use cases can involve 

- `func.ST_Intersects` from `SQLModel`, to check if two polygons intersect for instance (or if a point is inside a polygon).
- `func.ST_Distance` from `SQLModel`, to calculate the geodesic distance between two points (equivalent of
<a href=https://geopy.readthedocs.io/en/stable/#module-geopy.distance  class="external-link" target="_blank">geopy geodesic distance</a>.)

The Geoalchemy 2 <a href=https://geoalchemy-2.readthedocs.io/en/latest/spatial_functions.html class="external-link" target="_blank">documentation</a> proves invaluable here. 

!!! warning
    `type_coerce` and the `type_` argument is needed for most of the functions as they only work for `Geogmetry` by default. 

## Setting up the db stack

All relevant information can be found in the `docker-compose.postgis.yml` file (<a href=https://github.com/SE-Sustainability-OSS/ecodev-infra/blob/main/docker-postgis.yml class="external-link" target="_blank">here</a>).

### Production mode

Remember that you need to setup traefik when in production before launching this stack. Go read [the traefik page](traefik.md) if not already done.

To start the stack in production, you can make use of the `Makefile` provided in the repo (`make help`) to list available commands.

To use it you will need to **create one folder** in the folder containing `docker-compose.yml`

- `postgis_db_data`

you will need to create a `.postgis.env` <a href=https://www.codementor.io/@parthibakumarmurugesan/what-is-env-how-to-set-up-and-run-a-env-file-in-node-1pnyxw9yxj  class="external-link" target="_blank">file</a> and setting

- `pgadmin_url` entry to the DNS address you created in order to reach the pgadmin interface. 
- `POSTGRES_DB`: the db name to which your application will connect to 
- `POSTGRES_USER`: the user authorized to connect to `POSTGRES_DB`
- `POSTGRES_PASSWORD`:  the password associated to `POSTGRES_USER`
- `PGADMIN_DEFAULT_EMAIL`: the username to connect to the pgAdmin web interface (accessible at `pgadmin_url`)
- `PGADMIN_DEFAULT_PASSWORD`:  the password to connect to the pgAdmin web interface (accessible at `pgadmin_url`)

You can then safely launch 
```shell
make postgisprod-launch
```

<div class="termy">
```console
$ make postgisprod-launch
---> 100%
Production Postgis DB successfully setup!
```
</div>

pgAdmin will be accessible at `pgadmin_url`

### Local dev mode

In this case you will make use of the additional  `docker-compose.postgis.override.yml` file. Here you will set in your `.env` **all parameters of the previous section** (bare 
`pgadmin_url`, since traefik is not used locally) plus 
- `postgis_pgadmin_port`: the local port on which you want to access to pgAdmin. If not set, 5052 is used by default. 

Then launch
```shell
make postgisdev-launch
```

<div class="termy">
```console
$ make postgisdev-launch
---> 100%
Local dev Postgis DB successfully setup!
```
</div>

pgAdmin will be accessible at `http://127.0.0.1:postgis_pgadmin_port`

## Add pgadmin extension 

If you manually create a new database, you will need to recreate the Postgis extension. This can be done via pgadmin.

```shell
 CREATE EXTENSION Postgis;
```

