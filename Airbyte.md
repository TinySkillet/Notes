[Airbyte](https://docs.airbyte.com/) is an open source infrastructure for building **EL** *(extract and load)* data pipelines. 

With **Airbyte**, we can build data pipelines and replicate data from a source to destination. It provides configuration on how frequently data is synced, what data is replicated, and how the data is written to in the destination.


#### Source
A **source** is an **API, file, database or data warehouse** that you want to ingest data from. The configured source is what you set up when you provide the variables needed for the **connector** to access records. The exact fields of the configuration depend on the connector, but in most cases, it provides **authentication information** (username, password, API key) and information about **which data to extract**, for example, the start date to sync records from, search query records have to match.

#### Destination
A destination is a data warehouse, data lake, database or an analytics tool where you want to load your ingested data.

#### Connection
A connection is an **automated data pipeline** that replicates data from a source to destination.


| Concept                                     | Description                                                    |
| ------------------------------------------- | -------------------------------------------------------------- |
| **Stream and Field Selection**              | What data should be replicated from the source to destination? |
| **Sync Mode**                               | How should the streams be replicated (read and written)?       |
| **Sync Schedule**                           | When should a data sync be triggered?                          |
| **Destination Namespace and Stream Prefix** | Where should the replicated data be written?                   |
| **Schema Propagation**                      | How should Airbyte handle schema drift in sources?             |



#### Stream
A stream is a group of related records. Depending on the destination, it may be called a table, file, or blob. We use the term `stream` to generalize the flow of data to various destinations.

