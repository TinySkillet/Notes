Here are the **Abstract**, **List of Figures**, and **List of Tables** tailored to your project.

I have written the **Abstract** to be substantial (approx. 250 words) and specific to your engineering achievements (O(1) memory, SQLite WAL, Custom TCP), avoiding generic AI-sounding fluff.

The **Lists** use the `Chapter.Number` format (e.g., Figure 4.1) which is standard for theses. If you used continuous numbering (e.g., Figure 1, Figure 2... Figure 20), simply adjust the numbers in the left column to match your final document.

---

### **ABSTRACT**

The centralized model of cloud storage, while convenient, introduces significant risks regarding data sovereignty, privacy, and single points of failure. As digital data generation accelerates, reliance on third-party providers like Google Drive or AWS creates a power asymmetry where users lose control over their information. This project addresses these vulnerabilities by engineering a **Decentralized Peer-to-Peer (P2P) Storage System** designed specifically for small-scale, trusted collaborative groups.

Unlike global-scale networks such as IPFS, which suffer from high complexity and resource overhead, this solution prioritizes simplicity, privacy, and local-first availability. The system is built as a Command Line Interface (CLI) application using the **Go programming language**. It implements a **Hybrid Unstructured Architecture**, utilizing a bootstrap node for initial discovery and a gossip protocol for mesh connectivity.

Key technical contributions of this research include the development of a custom **TCP transport layer** with strict message framing, avoiding the bloat of external networking libraries. To ensure data integrity, the system implements **Content-Addressable Storage (CAS)** using SHA-256 hashing. Privacy is enforced through **Encryption-at-Rest** using AES-256 in Counter (CTR) mode. A significant engineering achievement was the implementation of a streaming I/O architecture, which allows the transfer of multi-gigabyte files with **constant O(1) memory usage**, making the system viable for resource-constrained hardware. Furthermore, the integration of **SQLite in Write-Ahead Logging (WAL) mode** ensures robust state persistence and concurrency.

Testing confirmed that the system successfully replicates data across nodes, recovers automatically from peer failures, and maintains zero-knowledge privacy regarding stored content. The result is a functional, deployable artifact that provides a resilient alternative to centralized storage for private networks.

---

### **LIST OF FIGURES**

| Figure | Caption | Page |
| :--- | :--- | :--- |
| **3.1** | Distribution of primary storage methods among survey respondents | 17 |
| **3.2** | Frequency of data loss events reported by users | 18 |
| **3.3** | User sentiment regarding the importance of data privacy | 18 |
| **3.4** | Current user trust levels in centralized cloud providers | 19 |
| **3.5** | User willingness to adopt decentralized storage solutions | 19 |
| **3.6** | User willingness to contribute local storage resources | 20 |
| **3.7** | User tolerance for occasional network downtime | 20 |
| **3.8** | User acceptance of Command Line Interface (CLI) tools | 21 |
| **3.9** | User familiarity with BitTorrent technology | 21 |
| **3.10** | Frequency of user backup habits | 22 |
| **4.1** | Sea Level Use Case Diagram showing high-level system interactions | 25 |
| **4.2** | Fish Level Use Case Diagram detailing specific user operations | 26 |
| **4.3** | Hybrid Network Topology: Bootstrap discovery leading to Mesh communication | 27 |
| **4.4** | Layered Software Architecture showing separation of concerns | 28 |
| **4.5** | Activity Diagram detailing the File Upload and Replication logic | 29 |
| **4.6** | Flowchart illustrating Data Retrieval and search decision logic | 31 |
| **4.7** | Process Flow for Distributed File Deletion | 32 |
| **4.8** | Entity Relationship Diagram (ERD) for the SQLite Database | 33 |
| **4.9** | Path Transformation Logic for Content-Addressable Storage | 37 |
| **5.1** | Development Environment in VSCode showing active server logs | 39 |
| **6.1** | Unit Test execution results validating path generation logic | 48 |
| **6.2** | Evidence of file replication across three distinct node directories | 50 |
| **6.3** | Verification of Ciphertext: The stored file is unreadable without the client | 52 |
| **7.1** | The NAT Barrier: Illustration of inbound traffic blocking by routers | 55 |

---

### **LIST OF TABLES**

| Table | Caption | Page |
| :--- | :--- | :--- |
| **4.1** | Functional Requirements (MoSCoW Prioritization) | 25 |
| **4.2** | Non-Functional Requirements | 26 |
| **4.3** | Database Schema Definitions | 33 |
| **6.1** | Memory Usage Analysis: Standard Buffering vs. Streaming I/O | 51 |
| **7.1** | Project Objectives vs. Final Outcome (Traceability Matrix) | 53 |
| **8.1** | Comparison of Connectivity Solutions (UPnP vs. STUN/TURN) | 58 |