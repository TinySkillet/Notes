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


### Solution: User Trino

Trino is a fork of Presto, create by the original creators of Presto.

Trino directly solves the restart problem with its **Dynamic Catalog Management** feature.

It provides a:
- REST API to manage data source on a live, running cluster with zero downtime.

