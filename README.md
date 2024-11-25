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

## Installation
The application can be launched using a multicontainer instance in Docker. Before starting to use the app, define a specific key for SSL traffic:
*remember to define a complex password in this placeholder <HTTPS_CERT_PASSWORD>*
```
dotnet dev-certs https -ep ${HOME}/.aspnet/https/aspnetapp.pfx -p <HTTPS_CERT_PASSWORD>
dotnet dev-certs https --trust
```
*remember to define these placeholders, according relative position*
- <HTTPS_CERT_PATH>
- <HTTPS_CERT_PASSWORD>
- <POSTGRES_DBNAME>
- <POSTGRES_USERNAME>
- <POSTGRES_PASSWORD>
- <MONGODB_USERNAME>
- <MONGODB_PASSWORD>
- <REDIS_DB>
- <REDIS_PASSWORD>
- <PATH_BIBLIO_DATA>

```
services:
  biblio.ui:
    image: domlan/biblio:1.1124
    environment:      
      - ASPNETCORE_ENVIRONMENT=Staging
      - ASPNETCORE_HTTP_PORTS=8080
      - ASPNETCORE_HTTPS_PORTS=8081      
      - ASPNETCORE_Kestrel__Certificates__Default__Path=/https/aspnetapp.pfx
      - ASPNETCORE_Kestrel__Certificates__Default__Password=<HTTPS_CERT_PASSWORD>
      - BIBLIO_POSTGRES_CONNECTIONSTRING=Host=postgres;Port=5432;Database=<POSTGRES_DBNAME>;Username=<POSTGRES_USERNAME>;Password=<POSTGRES_PASSWORD>;
      - BIBLIO_MONGODB_CONNECTIONSTRING=mongodb://<MONGODB_USERNAME>:<MONGODB_PASSWORD>@mongodb:27017/?retryWrites=true&w=majority
      - BIBLIO_REDIS_CONNECTIONSTRING=redis:6379,password=<REDIS_PASSWORD>,defaultDatabase=<REDIS_DB>,ssl=false,connectTimeout=10000,connectRetry=2"
    ports:
      - 52456:8080
      - 53478:8081
    volumes:
      - $HOME/.microsoft/usersecrets:/app/.microsoft/usersecrets:ro
      - $HOME/.aspnet/https:/https:ro
    depends_on:
      - postgres
      - mongodb
  
  postgres:
    image: postgres:14.5-alpine
    ports:
      - 5432:5432
    volumes:
      - pgdata:/var/lib/postgresql/data
    environment:
      - POSTGRES_PASSWORD=<POSTGRES_PASSWORD>
      - POSTGRES_USER=<POSTGRES_USERNAME>
      - POSTGRES_DB=<POSTGRES_DBNAME>

  mongodb:
    image: mongo:8.0.3-noble
    restart: always
    ports:
      - 27017:27017
    volumes:
      - dbdata7:/data/db
    environment:
      - MONGO_INITDB_ROOT_USERNAME=<MONGODB_USERNAME>
      - MONGO_INITDB_ROOT_PASSWORD=<MONGODB_PASSWORD>

  redis:
    image: redis:8.0-M02-alpine3.20
    restart: always
    ports:
      - 6379:6379
    volumes:
      - redisdata:/root/redis
      - redisdata:/usr/local/etc/redis/redis.conf
    environment:
      - REDIS_PASSWORD=<REDIS_PASSWORD>
      - REDIS_PORT=6379
      - REDIS_DATABASES=<REDIS_DB>

volumes:
  pgdata:
    driver: local
    driver_opts:
      type: 'none'
      o: 'bind'
      device: '$HOME/<PATH_BIBLIO_DATA>/data/postgres'
  dbdata7:
    driver: local
    driver_opts:
      type: 'none'
      o: 'bind'
      device: '$HOME/<PATH_BIBLIO_DATA>/data/db'
  redisdata:
    driver: local
    driver_opts:
      type: 'none'
      o: 'bind'
      device: '$HOME/<PATH_BIBLIO_DATA>/data/redis'
```

## Setup
Start cloning GitHub repository
```
git clone git@github.com:domlan-dev/biblio-public.git
```
