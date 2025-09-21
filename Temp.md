**Plan for today**
- Research Temporal
- Front-end playground enhancement


**Sept 18 Progress**
1. The primary identifier of knowledge base is the `id` of `knowledge_base_mapping`. Previously it used to be `kb_id` which would either be manually generated (in case of Milvus), or auto generated (by Bedrock).
2. `kb_id` now can refer to the actual `id` of the Bedrock knowledge base, since it is auto-generated. For milvus it is null. 
3. Switched to new database because I had to change the schema.
4. The `pgvector` extension in Postgres was being created each time a table was being created. Instead I have included this in the migration file.
5. All DAO methods in `app/daos/knowledge_base.py` that previously used `kb_id` to query or modify knowledge base records now use the `id` from the `knowledge_base_mapping` table.
6. Knowledge base name can include any characters now. Previously it used to have validation (not include spaces, hyphens). Knowledge base name that Bedrock requires is auto generated using *nanoID* library. 
7. The collection names (postgres table, milvus collection) is also auto generated and configured using *nanoID*.  
8. Database engine is now configured with a connection pool to improve performance and manage database connections efficiently. 


---


Rijan Acharya
Samundra Magar


C is a procedural language. Everything happens steps by steps through functions. Data is stored in array, and whenever we need to modify data outside the function, we can use pointers or references.

If we need to store large amount of data, we allocate memory on the heap and use a pointer to reference and store the data into that memory.

Code reusability can be done through structs, header files, functions.


---


**OOP** solves issues of reusability and scalability by grouping closely related data and functions into a single data structure called class. Further inheritance lets you to reuse data & functionalities from another classes. 

Security - private, public, protected methods and properties
Large programs - chain of inheritances, association

Classes - programmatic model of something 
