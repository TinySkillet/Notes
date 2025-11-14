
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


