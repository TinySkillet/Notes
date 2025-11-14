
## PRESTO

- Open Source SQL query engine
- Fast, Reliable and Efficient at Scale
- Only SQL
- Presto with Docker Sandbox
- Presto with Kubernetes



To configure a connection to a data source, you define a **catalog.** A catalog is a named configuration that references a specific connector and provides the necessary connection details. 

We could have a *postgresql_sales* catalog to connect to our sales database in PostgreSQL.


#### Architectural Blueprint

1. **API Endpoint**
2. **Backend Service**: Receives user submission, validates them and orchestrate the Preston configuration update.
3. **Configuration Store**: A database to store the metadata for the user-configured data sources. 
4. **Presto Cluster**: Presto coordinator and workers
5. **Automation/Orchestration Tool** (Kubernetes Operator): The mechanism that will deploy the new configuration files and trigger the Presto reload/restart.



#### Core Problem
Presto was originally designed to load its entire configuration, including all the available data source catalogs, when the service starts up.

It does not automatically watch for new files in the catalog directory.

**This is a major limitation for our user driven system because:**

1. It causes downtime: Every restart terminates all currently running queries
2. It's not scalable: It creates a manual bottleneck
3. It's disruptive: It affects all the users on the system.


### Solution: Use Trino

Trino is a fork of Presto, create by the original creators of Presto.

Trino directly solves the restart problem with its **Dynamic Catalog Management** feature.

It provides a:
- REST API to manage data source on a live, running cluster with zero downtime.


It is a universal translator for your data sources. Whether your data is in a traditional database, a data lake, or even a NoSQL store, Trino speaks their language and lets you query everything with standard SQL.

You can join data between different systems (e.g., PostgreSQL and MongoDB) in a single query as well.


> [!NOTE] Important
> Even though Trino understands and can efficiently execute SQL, Trino is not a database, as it does not include its own data storage system


Trino on Kubernets
https://trino.io/docs/current/installation/kubernetes.html


Trino on Docker
https://trino.io/docs/current/installation/containers.html



### Setting up Trino Cluster with Docker

1. **Project Directory**
```
trino-project/
├── docker-compose.yml
└── trino/
    └── etc/
        ├── config.properties
        ├── jvm.config
        └── node.properties
   ```

2. **trino/etc/jvm.config**
   This file controls the JVM settings.
```
coordinator=true
node-scheduler.include-coordinator=true
http-server.http.port=8080
discovery-server.enabled=true
discovery.uri=http://localhost:8080
   ```

3. **trino/etc/node.properties**
   The file contains settings specific to each node in the cluster.
```
node.environment=development
node.id=a-unique-id-for-this-node
node.data-dir=/data/trino
   ```

4. **docker-compose.yml**
```yaml
version: '3.7'
services:
  trino-coordinator:
    image: trinodb/trino:latest
    container_name: trino-coordinator
    ports:
      - "8080:8080"
    volumes:
      - ./trino/etc:/etc/trino
   ```
`docker compose up -d

5. **Open Web Browser**
   Go to http://localhost:8080 and you will see the Trino Web UI. It will show one active worker(the coordinator, since we configured it to act as a worker too.)



#### Connecting to data sources

1. **User provides connection info via UI.**
```
    Catalog Name: sales_reports

    Database Type: PostgreSQL

    Hostname: sales-db.mycompany.com

    Port: 5432

    Database Name: sales

    Username: trino_reader

    Password: a_very_secret_password
   ```

2. **Backend Service constructs the configuration**


   The backend takes the user's input and formats it into the configuration Trino understands. 
   
```json
{
  "connector.name": "postgresql",
  "connection-url": "jdbc:postgresql://sales-db.mycompany.com:5432/sales",
  "connection-user": "trino_reader",
  "connection-password": "a_very_secret_password"
}   
```

3. **Backend makes the call to trino coordinator.**

Our service sends an HTTP POST request to the Trino API to create a new catalog.


curl -X POST "http://trino-coordinator:8080/v1/catalog" \
--header "Content-Type: application/json" \
--header "X-Trino-User: my-admin-user" \
--data '{
    "catalogName": "sales_reports",
    "connectorName": "postgresql",
    "properties": {
        "connection-url": "jdbc:postgresql://sales-db.mycompany.com:5432/sales",
        "connection-user": "trino_reader",
        "connection-password": "a_very_secret_password"
    }
}'








