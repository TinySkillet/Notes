**Plan for today**
- Research Temporal
- Front-end playground enhancement



**Sept 18 Progress**
1. The primary identifier of knowledge base is the `id` of `knowledge_base_mapping`. Previously it used to be `kb_id` which would either be manually generated (in case of Milvus), or auto generated (by Bedrock).
2. Switched to new database because I had to change the schema.
3. The `pgvector` extension in Postgres was being created each time a table was being created. Instead I have included this in the migration file.
4. All DAO methods in `app/daos/knowledge_base.py` that previously used `kb_id` to query or modify knowledge base records now use the `id` from the `knowledge_base_mapping` table.
5. Knowledge base name can include any characters now. Previously it used to have validation (not include spaces, hyphens). Knowledge base name that Bedrock requires is auto generated using *nanoID*. 
6. The collection names (postgres table, milvus collection) is also auto generated and configured using *nanoID*.  
7. Route