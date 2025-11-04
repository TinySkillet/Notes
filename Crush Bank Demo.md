IBM Spark 
IBM Presto engine (developed by Meta)
IBM COS for storage

Apache Iceberg as the catalog (inventory management system)

Presto is needed to query the data from UI

I Resource Unit = 1 USD / hr 
might change in the future though.


For best practice: If engines are not being used, pause them. It will stop costing you.

catalog.schema.tablename


Datalake-dev is the COS they're using for the project.

cb-staging is where all the data/schema will be stored.

`staging` catalog is stored under `cb-staging`.

