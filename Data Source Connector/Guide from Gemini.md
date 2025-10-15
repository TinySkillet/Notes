For your specific goal of building a *sellable, multi-platform product*, **the `dlt` library is the more appropriate and strategic choice.**

Let's break down why, comparing the two in the context of your requirements.

### Understanding the Roles: Service vs. Library

*   **`dlt` (Data Load Tool)** is an **open-source data pipeline library**. It is a component, not a full service. Its primary job is the **Extract and Load** part of a data pipeline:
    *   **Connectors (Sources):** It provides robust, easy-to-use connectors to pull data from databases (Postgres, MySQL, etc.), APIs, and files.
    *   **Transformation & Loading (Destinations):** It helps you structure this data and load it into a destination, which in your case would be your vector database, Milvus.
    *   **You build the system around it.** `dlt` doesn't perform the search; it feeds the system that does (your LLM + Milvus).

### Comparison Based on Your Needs

| Feature                           | Amazon Kendra                                                                                                                                | `dlt` (Data Load Tool)                                                                                                                                                                              | Your Best Fit                                                                                                                                                                   |
| :-------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Core Functionality**            | A complete, managed search service.                                                                                                          | A library for building data pipelines.                                                                                                                                                              | **`dlt`**: You already have the search/RAG part (Bedrock/Milvus). You just need the data ingestion component. Kendra would be redundant and replace your existing Milvus logic. |
| **Deployment**                    | AWS cloud only. Cannot be run locally or on other clouds.                                                                                    | A Python library. Can be deployed anywhere: locally, on-prem, any cloud provider.                                                                                                                   | **`dlt`**: This is a critical win for you. Your requirement to deploy locally and potentially for customers on their own infrastructure makes Kendra a non-starter.             |
| **Licensing & Commercialization** | Proprietary AWS service. You pay for usage. Bundling it into a commercial product is complex and expensive, locking your customers into AWS. | **Apache 2.0 License.** This is a permissive open-source license. You can freely use, modify, and embed it in your commercial product without paying licensing fees or open-sourcing your own code. | **`dlt`**: The Apache 2.0 license is exactly what you need to build a commercial product without restriction.                                                                   |
| **Customization & Control**       | Limited. You configure the service but cannot change its core indexing or ranking logic.                                                     | Full control. You write the Python code. You can add any custom logic for chunking, metadata extraction, embedding, and loading into Milvus.                                                        | **`dlt`**: Gives you the fine-grained control needed to optimize the RAG pipeline for different database schemas and customer needs.                                            |
| **Cost**                          | Pay-as-you-go pricing for indexing and queries. Can become expensive at scale, and this cost would have to be passed on to your customers.   | Free library. You only pay for the infrastructure you run it on (e.g., a small VM or container) and your own development time.                                                                      | **`dlt`**: The cost model is much more predictable and controllable for a product you intend to sell.                                                                           |
| **Integration**                   | Connects to your services via its API. It would replace Milvus.                                                                              | Integrates directly into your existing Python application code. It would be the tool you use to *populate* Milvus.                                                                                  | **`dlt`**: It fits perfectly as the "data source connector" module within your current architecture without forcing you to re-architect anything.                               |

### How `dlt` Fits Into Your Architecture

Your current RAG pipeline likely looks something like this:
1.  **Ingestion:** Manually upload files to S3/local.
2.  **Processing:** A script reads, chunks, and creates embeddings for the file content.
3.  **Storage:** The chunks and embeddings are stored in Milvus.
4.  **Retrieval:** A user query is used to find relevant chunks in Milvus.
5.  **Generation:** The query and retrieved chunks are sent to Bedrock (or your custom LLM) to generate an answer.

Here’s how you would integrate `dlt` to handle the database connectors:

1.  **Ingestion (with `dlt`):**
    *   Your service takes database credentials from your customer.
    *   A `dlt` pipeline script uses a **source** like `dlt.sources.sql_database` to connect to their Postgres or MySQL instance. You configure which tables and columns to pull.
    *   The `dlt` pipeline runs, extracting data row by row or in batches.
2.  **Processing (within the `dlt` pipeline):**
    *   You can add custom transformation steps (`@dlt.transformer`) to the pipeline to format the data from each row into a clean text document.
    *   You then chunk these documents and generate embeddings using your chosen model.
3.  **Storage (with `dlt`):**
    *   You use a `dlt` **destination** to load the final data. `dlt` has a native [Milvus destination](https://dlthub.com/docs/dlt-ecosystem/destinations/milvus), which is a perfect fit for you. You would configure it to load the text chunks, embeddings, and metadata directly into your Milvus collection.

### Conclusion

Your intuition is correct. **`dlt` is the better choice for your use case.**

Amazon Kendra is a powerful tool for organizations that want an out-of-the-box, internal enterprise search solution with minimal development overhead. However, it is not designed to be a portable, embeddable component for a commercial software product.

`dlt` provides the exact functionality you need—robust, production-ready database connectors—in a format (an open-source library) that gives you the flexibility, control, and commercial-friendly licensing required to build, deploy, and sell your knowledge base service.