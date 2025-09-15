#data-connectors 


EL data pipelines, more commonly known as **ELT** *(Extract, Load, Transform)* data pipelines, represent a modern approach to data integration. 

Unlike traditional **ETL** (Extract, Transform, Load) pipelines, **ELT** pipelines first extract raw data from various sources and load it directly into a target system, typically a date warehouse or a data lake. The transformation of the data then occurs within the target system. 

##### Breakdown of the ELT process
1. **Extract**: Data is collected from various sources, which can include databases, APIs, flat files, streaming sources, and SaaS tools.
2. **Load**: The raw, unstructured data is loaded directly into a scalable and often cloud-based data warehouse or data lake. This step is generally faster in ELT than in ETL because it bypasses an intermediate transformation stage.
3. **Transform**: Once loaded, the data is transformed within the data warehouse using its processing power, often through SQL or other processing tool like Spark.

##### Use Case of ELT data pipeline
**Real-time Analytics for E-commerce and Retail**
 E-commerce companies need to understand customer behavior, product performance, and marketing campaign effectiveness in real time. ELT pipelines can ingest data from online stores, mobile apps, CRM systems, ad campaigns and social media. This raw data is loaded directly into a cloud data warehouse, allowing for quick transformations to build unified customer profiles, personalize recommendations, optimize pricing, and deliver targeted marketing campaigns.

*An e-commerce brand might use ELT to identify repeat customers who purchased specific items and then trigger automated campaigns with personalized offers.*



##### **FURTHER READING**
**[[Airbyte]]







