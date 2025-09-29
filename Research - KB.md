#### **RAG**
Retrieval Augmented Generation is a technique that combines a language model with a retriever (like a search engine) to improve the accuracy and factuality of generated responses. 

Instead of relying on what the model has memorized during training, RAG actively retrieves relevant information from an external knowledge base (e.g., Wikipedia, private documents, vector databases) before generating an answer.

#### **How it works: Step by step**

1. **User Query**
   You ask a question

2. **Retriever**
   The system searches a document database (e.g. Wikipedia or company's docs) using semantic search (e.g. via embeddings and a vector database)

3. **Top-K Documents**
   The retriever returns, say, the top 5 most relevant chunks or passages

4. **Generator (LLM)**
   These retrieved chunks are passed along with your query to the LLM (like GPT-4), which uses them as context to generate an answer.
   
5. **Final Answer**
   You get a response, that is grounded in the actual documents retrieved.


#### **Example Use Case**
**Let's say:**
You work at a law firm and want to build an AI assistant for legal research.

- You store thousands of legal documents and case files. 
- When a lawyer asks a question, RAG:
    1. Retrieves relevant legal clauses/cases.
    2. Passes them to GPT.
    3. Returns a grounded summary.



### **Knowledge Base**

A **knowledge base** is a centralized, structured repository of information that a computer system - including AI models - can use to understand, answer questions, make decisions, or solve problems.

1. **Structured Knowledge Base**
   - Built using **formal logic**, databases and graphs.
   - Information is encoded in a way that's easy for machines to query and reason about.
   - Examples:
	 - Wikidata, Knowledge graph (Google Knowledge Graph)
	
1. **Unstructured or semi-structured KB**
   - Contains natural language text, documents FAQs or articles.
   - Maybe used in RAG systems to augment LLMs.
   - Stored in formats like:
     - Markdown, PDFs, HTML pages.
     - Vector databases (for semantic search)



##### A knowledge base has:
	Facts
	Entities (named things)
	Relationships (links between entities)
	Rules 
	Documents



**Structured KB**
```json
{
	"entity": "Earth",
	"type": "planet",
	"properties": {
		"orbits": "Sun",
		"has_life": true,
	}
}
```

**Unstructured KB**
```

Earth is the third planet from the Sun and the only known astronomical object to
harbor life.
```


##### Where do Knowledge bases live?
- Databases (SQL, NoSQL)
- Triples stores (for knowledge graphs using RDF)
- Vector stores (FAISS, Pinecone, etc)
- Flat files (Markdown docs, PDFs)


See: [[JSON KB in RAG]]


 
 