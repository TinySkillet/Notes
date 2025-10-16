1. If I have a data source,  say a REST API or a Postgres DB, would data from this be ingested into Bedrock or Milvus?

2. Are we going to execute raw SQL queries for RDBMS through agents? Should we convert SQL query results to text and feed them to bedrock or embed and store it into vector database?

3. Looked into DLT library in Python. It can load data from REST APIs, 30 + SQL databases, S3, Google Cloud, Google Drive, local file system, remote file system, Azure Blob.

- https://dlthub.com/docs


Agent le call garcha

Need to implement Continuous Change Detection (CDC) on the database

User should be able to choose which table needs to be ingested. agent (I guess user for now) can decide which column to be ingested


