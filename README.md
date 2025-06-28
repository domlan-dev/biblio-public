# Biblio (*Documents storage system*)

Biblio is a tool for organizing and storing various documentation, consisting of files in PDF format. It consists of:
- Biblio, main Net Core 8.0 SPA
- Postgresql
- MongoDB
- Redis
- RabbitMQ

## Postgresql
*Used to store configuration data as Metas, Tags and User Options*

PostgreSQL also known as Postgres, is a free and open-source relational database management system (RDBMS) emphasizing extensibility and SQL compliance. PostgreSQL features transactions with atomicity,consistency, isolation, durability (ACID) properties, automatically updatable views, materialized views, triggers, foreign keys, and stored procedures. It is supported on all major operating systems, including Windows, Linux, macOS, FreeBSD, and OpenBSD, and handles a range of workloads from single machines to data warehouses, data lakes, or web services with many concurrent users.

## MongoDB
*Used to store documents*

MongoDB is a source-available, cross-platform, document-oriented database program. Classified as a NoSQL database product, MongoDB utilizes JSON-like documents with optional schemas. MongoDB is developed by MongoDB Inc. and current versions are licensed under the Server Side Public License (SSPL). MongoDB is a member of the MACH Alliance.

## Redis
*Used to speed up information retrieval operations from relational and NoSql databases*

Redis (Remote Dictionary Server) is a source-available, in-memory storage, used as a distributed, in-memory key–value database, cache and message broker, with optional durability. Because it holds all data in memory and because of its design, Redis offers low-latency reads and writes, making it particularly suitable for use cases that require a cache. Redis is the most popular NoSQL database, and one of the most popular databases overall. Redis is used in companies like Twitter, Airbnb, Tinder, Yahoo, Adobe, Hulu, Amazon and OpenAI.

## RabbitMQ
*Used for asynchronous task as OCR recognition, ML model definition etc*

RabbitMQ is an open-source message broker software that facilitates communication between different parts of an application by sending and receiving messages through a queue system. It implements the Advanced Message Queuing Protocol (AMQP) and allows for asynchronous, decoupled, and scalable communication between services, often used in microservice architectures and distributed systems.

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

# Distributed Application Stack for Swarm or Kubernetes with Externalized Volumes

This stack configuration enables the rapid deployment of a fully containerized, distributed application architecture using **Docker Swarm** or **Kubernetes**. It is designed to run seamlessly in orchestrated environments and is composed of modular services, including:

- A web UI frontend  
- Deep learning and OCR processing components  
- Relational (PostgreSQL) and document-based (MongoDB) databases  
- Caching (Redis)  
- Messaging system (RabbitMQ)

Thanks to container orchestration, launching this entire ecosystem across multiple nodes requires only a single `docker stack deploy` command (or equivalent in Kubernetes). The services are distributed across **worker nodes** via placement constraints and interconnected through a common **overlay network**.

---

## Volume Management and Distributed Storage

A key architectural feature is the use of **bind-mounted volumes** that are **externalized from the cluster nodes**. These volumes are not local to any single node but instead reside on **a shared storage host**, typically accessible via **NFS** or similar network file systems.

This ensures persistent, consistent data across the cluster, even if containers are rescheduled on different nodes. However, it is **crucial to prepare the shared storage before deploying the stack**.

### Example: Setting Up NFS Shared Volumes on a Remote Host

Before deploying the stack, create and expose the necessary folders on a separate machine (not part of the Swarm/K8s cluster):

```bash
# On the NFS host (e.g., 192.168.1.100)
sudo mkdir -p /mnt/swarm-volumes/{pgdata,dbdata7,redisdata,shared-temp,rabbitmq-lib,rabbitmq-log}
sudo chown -R nobody:nogroup /mnt/swarm-volumes
sudo chmod -R 777 /mnt/swarm-volumes

# Edit /etc/exports on the NFS server
echo "/mnt/swarm-volumes  *(rw,sync,no_subtree_check,no_root_squash)" | sudo tee -a /etc/exports

# Apply NFS export changes
sudo exportfs -a
```

On each node in your Swarm or Kubernetes cluster, mount the shared NFS directories:

```bash
# Example on each node
sudo apt install nfs-common
sudo mkdir -p /mnt/swarm-volumes
sudo mount -t nfs 192.168.1.100:/mnt/swarm-volumes /mnt/swarm-volumes
```

Make sure this mount is persistent (e.g., via `/etc/fstab`) and accessible at the same path across all nodes.

---

## Portainer Compatibility and YAML Version

The `stack-compose.yml` provided uses **version 3.8**, which is not the latest, but is fully aligned with the requirements of **PortainerIO**, a popular UI-based management tool for Docker Swarm and Kubernetes. This makes it ideal for users who prefer visual stack management and simplified deployment pipelines.

---

## Conclusion

This setup offers a practical and scalable way to manage complex microservices architectures using Docker-native orchestration or Kubernetes. Its **emphasis on external shared volumes** provides high availability and fault tolerance for critical data. Once the shared volumes are configured and the cluster is in place, deploying the full stack is as easy as:

```bash
docker stack deploy -c stack-compose.yml my-stack
```

or for Kubernetes (after translation via Kompose or Helm):

```bash
kubectl apply -f k8s-manifest.yaml
```

## Issues or suggestions
Feel free to reach out to me to report issues or suggestions. It would be greatly appreciated.
You can send me your requests via GitHub, from the specific [section](https://github.com/domlan-dev/biblio-public/issues).