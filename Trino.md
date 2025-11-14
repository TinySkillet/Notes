
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

```sh
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
```


The Trino coordinator receives this API call and immediately loads the sales_reports catalog into memory.

---



Here are the official Trino terms and what they mean in practice.

### 1. Coordinator

The Coordinator is the brain and the single entry point of the Trino cluster. When you submit a query, it's the only component you directly interact with.

Its Responsibilities:

- Receives Queries: It takes the SQL query from a client (like the Trino CLI, a BI tool like Tableau, or your own application).

- Parses and Plans: It parses the SQL to ensure it's valid. Then, it creates a highly optimized "query plan"—a step-by-step recipe for how to fetch and process the data most efficiently.

- Manages Workers: It assigns specific tasks from the query plan to the available Worker nodes. It doesn't process the data itself; it orchestrates the work.

- Gathers Results: It collects the final results from the Workers and sends them back to you.

**There is only one Coordinator in a Trino cluster. It's the master planner.**

### 2. Worker

A Worker is the muscle of the cluster. It's a server whose only job is to execute the tasks given to it by the Coordinator.

Its Responsibilities:

- Executes Tasks: This is the heavy lifting. A task might be "scan the first 10 million rows of the customers table" or "join this data I have with data from another worker."

- Connects to Data Sources: The Workers make the actual connections to your databases (PostgreSQL, S3, etc.) to read the data.

- Processes Data in Memory: Trino is an in-memory engine, meaning Workers perform filtering, transformations, and joins on the data directly in their RAM, which is what makes it so fast.


> [!NOTE] 
> We can have many (from one to thousands of) Workers. Adding more Workers is how you scale Trino to handle more queries and larger datasets.

### 3. Connector

A Connector is a "plugin" that teaches Trino how to communicate with a specific type of data source. It's the translator.

- The PostgreSQL Connector knows how to speak the PostgreSQL protocol.

- The Hive Connector knows how to read data from HDFS or S3 and talk to the Hive Metastore.

- The MongoDB Connector knows how to query MongoDB collections.


> [!NOTE] 
> We have one connector for each type of system you want to query. The connector contains the logic, but it doesn't contain the connection details like hostname or password.

### 4. Catalog

This is the most important concept for your use case. A Catalog is a configured instance of a Connector. It's the bridge that connects Trino to one specific data source.

- You use the PostgreSQL Connector to create a PostgreSQL Catalog.

- This catalog is defined by a set of properties (e.g., in a sales-db.properties file or via the API). These properties tell the connector where to connect:

	- connection-url=jdbc:postgresql://hostname:5432/database
	-  connection-user=myuser
	- connection-password=mypassword


**The Crucial Difference**: We only have one PostgreSQL Connector in your Trino installation, but you could have ten different PostgreSQL catalogs.

- sales_db (pointing to the sales PostgreSQL server)
- inventory_db (pointing to the inventory PostgreSQL server)
- hr_prod (pointing to the HR PostgreSQL server)


> [!NOTE]
> The catalog name is what you use in your SQL queries to tell Trino where to look for the data.



Python Library

`trino-python-client`
https://github.com/trinodb/trino-python-client

``` python
import trino

# 1. Establish the connection to the Trino coordinator
conn = trino.dbapi.connect(
    host='localhost',    
    port=8080,
    user='your_username',
    catalog='postgresql',
    schema='public'      
)

cur = conn.cursor()

try:
    cur.execute("SELECT customer_id, name, city FROM customers WHERE city = 'New York'")

    # 4. Fetch the results
    rows = cur.fetchall()

    # 5. Process the results
    for row in rows:
        print(row)

finally:
    # 6. Always close the connection
    conn.close()
```



