# Biblio (*Documents storage system*)

Biblio is a tool for organizing and storing various documentation, consisting of files in PDF format. It consists of:
- Biblio, main Net Core 8.0 SPA
- Postgresql
- MongoDB
- Redis

## Postgresql
*Used to store configuration data as Metas, Tags and User Options*

PostgreSQL also known as Postgres, is a free and open-source relational database management system (RDBMS) emphasizing extensibility and SQL compliance. PostgreSQL features transactions with atomicity,consistency, isolation, durability (ACID) properties, automatically updatable views, materialized views, triggers, foreign keys, and stored procedures. It is supported on all major operating systems, including Windows, Linux, macOS, FreeBSD, and OpenBSD, and handles a range of workloads from single machines to data warehouses, data lakes, or web services with many concurrent users.

## MongoDB
*Used to store documents*

MongoDB is a source-available, cross-platform, document-oriented database program. Classified as a NoSQL database product, MongoDB utilizes JSON-like documents with optional schemas. MongoDB is developed by MongoDB Inc. and current versions are licensed under the Server Side Public License (SSPL). MongoDB is a member of the MACH Alliance.

## Redis
*Used to speed up information retrieval operations from relational and NoSql databases*

Redis (Remote Dictionary Server) is a source-available, in-memory storage, used as a distributed, in-memory key–value database, cache and message broker, with optional durability. Because it holds all data in memory and because of its design, Redis offers low-latency reads and writes, making it particularly suitable for use cases that require a cache. Redis is the most popular NoSQL database, and one of the most popular databases overall. Redis is used in companies like Twitter, Airbnb, Tinder, Yahoo, Adobe, Hulu, Amazon and OpenAI.

## Setup
Prerequisite (for Certificate): install Net Core (https://learn.microsoft.com/en-us/dotnet/core/install/linux)

If you use a web server or load balancer for routing (Httpd, NetScaler, Apache, Nginx), you can exclude this set of settings. The SSL offload task will be performed at the load balancer or web server level and this Service (Biblio) will listen on HTTP port 53478
Start cloning GitHub repository
```
git clone git@github.com:domlan-dev/biblio-public.git
```
Setup environment file (.env) in your local repository, according your preference: password level complexity, path naming convention etc.
Create initial path for data (PostgreSQL, MongoDB and Redis). For a configuration like this:
```
...
PATH_BIBLIO_DATA_POSTGRES=$HOME/biblio/data/postgres
PATH_BIBLIO_DATA_MONGO=$HOME/biblio/data/db
PATH_BIBLIO_DATA_REDIS=$HOME/biblio/data/redis
...
```
you can use:
```
cd
mkdir -p biblio/data
cd biblio/data
mkdir postgres
mkdir db
mkdir redis
```

Start stack vith Docker Compose

*move to your Home*
```
cd 
```

*move to your cloned repository directory (eg: /home/yourname/Source/Repos/biblio-public)*
```
cd Source/Repos/biblio-public
```

*start Biblio - attached mode*
```
docker compose up
```

*or start Biblio - detached mode*
```
docker compose up -d
```

Open your browser and navigate to http://<yourMachineIp>:53478 (eg: http://192.168.88.40:53478)

## Issues or suggestions
Feel free to reach out to me to report issues or suggestions. It would be greatly appreciated.
You can send me your requests via GitHub, from the specific [section](https://github.com/domlan-dev/biblio-public/issues).