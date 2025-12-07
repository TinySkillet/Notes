## **Schedule**

  

- **Day 1 (Theory & Requirements):** Write **Chapters 1 & 2**. (Target: ~20 pages). Focus on the "Why" (Centralization risks) and the "What" (Hybrid P2P, Local-First).

- **Day 2 (The Blueprint):** Write **Chapter 3**. (Target: ~18 pages). Draw the DB Schema (ERD) and Network Topology diagrams.

- **Day 3 (The Code):** Write **Chapter 4**. (Target: ~22 pages). The core implementation details. Paste code snippets (structs, handlers) and explain the logic.

- **Day 4 (Analysis):** Write **Chapter 5 & 6**. (Target: ~10 pages). Evaluation (O(1) memory usage) and Conclusion.

  

---
  

## **Detailed Table of Contents**

### **Front Matter** (Pages i - v)

- Title Page

- Abstract (Keywords: Hybrid P2P, Gossip Protocol, Content-Addressable Storage, Local-First)

- Acknowledgements

- Table of Contents

- List of Figures/Tables  

---
  

### **Chapter 1: Introduction**

**Target:** 5 Pages | **Words:** ~1,500

  

- **1.1 Introduction to the Problem:**

- Risks of centralized cloud storage (privacy breaches, single point of failure, censorship, cost).

- **1.2 Introduction to the Solution:**

- A **Hybrid Unstructured P2P System**.

- Key Philosophy: **Local-First Architecture** (using embedded SQLite) + **End-to-End Encryption**.

- **1.3 Aims and Objectives:**

1. **Secure Transport:** Custom TCP protocol with AES encryption.

2. **Resilient Discovery:** Gossip protocol for decentralized peer finding.

3. **Data Integrity:** Content-Addressable Storage (CAS) using SHA-1.

4. **Production Viability:** Systemd integration, daemonization, and configuration management.

- **1.4 Scope:**

- Targeted for small-to-medium trusted groups (Mesh topology).

- Not using heavy frameworks like libp2p (built from scratch for educational value and performance control).

  

---

  

### **Chapter 2: Literature Review & Background**

**Target:** 12 Pages | **Words:** ~3,500

  

- **2.1 Peer-to-Peer Architectures:**

- **Structured (DHTs):** Chord, Kademlia (Complex, rigid).

- **Unstructured:** Gnutella (Simple, flooding-based).

- **Hybrid (Your Choice):** Why it fits—combines the resilience of mesh with the ease of bootstrapping.

- **2.2 Data Discovery & Routing:**

- **Gossip Protocols vs. Flooding:** Explain how your "Peer Exchange" works (selective sharing) vs. naive flooding (spamming everyone).

- **2.3 Storage Paradigms:**

- **Content Addressable Storage (CAS):** Explain hashing (SHA-1) as a file ID. Reference Git's object storage.

- **Local-First Software:** The shift back to owning data. Explain why SQLite per node is better than a shared remote DB.

- **Zero-Knowledge Privacy:** Encrypting *before* transmission means the storage node is blind to the content.

- **2.4 Transport & Serialization:**

- **TCP Streams:** Why streams are necessary for large files (vs. UDP packets).

- **Serialization:** JSON vs. Gob. Justify **Go Gob** (Native, strongly typed, highly efficient for binary data).

  

---

  

### **Chapter 3: System Design (The Blueprint)**

**Target:** 15 Pages | **Words:** ~3,500 (+ Diagrams)

  

- **3.1 High-Level Architecture:**

- **Diagram:** "New Node" -> Connects to "Bootstrap" -> Receives Peer List -> Connects to "Peer A/B".

- **3.2 Database Design (The Schema):**

- **ERD Diagram:** Visualizing the 4 tables:

1. `peers` (Discovery/Routing table)

2. `files` (Local Metadata)

3. `shares` (Replication/Gossip tracking)

4. `keys` (Encryption management)

- **3.3 The Custom RPC Protocol:**

- **State Machine:** Connection -> Handshake -> Active -> Streaming.

- **Stream Mode:** Explain the protocol upgrade mechanism (Command Mode -> Raw Stream Mode) for file transfers.

- **3.4 Cryptographic Design:**

- **Stream Ciphers:** Explain AES-CTR/OFB. Why streaming encryption is critical for performance (O(1) memory).

- **3.5 Deployment Architecture (DevOps):**

- **Infrastructure as Code:** The `systemd` service design.

- **Security Hardening:** Explain `NoNewPrivileges`, `ProtectSystem`, and `ReadWritePaths`.

  

---

  

### **Chapter 4: Implementation (The Code)**

**Target:** 20 Pages | **Words:** ~3,000 (+ Code/Screenshots)

  

- **4.1 Technology Stack:** Go (Concurrency/Channels), SQLite (ACID Persistence), Cobra (CLI).

- **4.2 Project Structure:**

- Tree view of the codebase (`p2p/`, `db/`, `server.go`) explaining Separation of Concerns.

- **4.3 Transport Layer (`tcp_transport.go`):**

- Code: The `AcceptLoop` and `HandleConn`.

- Code: The Gob decoder logic.

- **4.4 The Gossip Protocol (`peer_exchange.go`):**

- Code: The logic for receiving a peer list, filtering duplicates, and dialing new peers.

- **Diagram:** Sequence diagram of the "Handshake & Exchange".

- **4.5 Storage Engine & CAS (`storage.go`):**

- Code: `PathTransformFunc` (hashing logic).

- Code: `copyEncrypt` (streaming encryption).

- **4.6 Database Integration (`db/repo.go`):**

- Code: SQL queries for inserting files and "Upserting" peers.

- **4.7 CLI & Configuration:**

- **Precedence Logic:** Explain `CLI Flag > Config File > Defaults`.

- Screenshot: The CLI help menu (`--help`).

  

---

  

### **Chapter 5: Testing & Evaluation**

**Target:** 8 Pages | **Words:** ~2,000

  

- **5.1 Functional Testing:**

- **Scenario A: Bootstrapping:** Screenshot of Node A joining Node B.

- **Scenario B: Gossip Propagation:** Screenshot of logs showing "Received peer exchange with X peers".

- **Scenario C: Streaming Upload:** Logs showing a file transfer.

- **5.2 Reliability Testing:**

- **Persistence:** Restarting a node and verifying peers are still in the DB.

- **Error Handling:** Discuss how the system filters "broken pipe" errors for stability.

- **5.3 Architectural Efficiency (Performance):**

- **Memory Analysis:** Explain **O(1) Memory Usage**. Because of `io.Copy` and `LimitReader`, a 10GB file transfer uses the same RAM as a 1KB file. This is a major engineering win.

- **Latency:** "First-Come-First-Served" download logic reduces wait times.

  

---

  

### **Chapter 6: Conclusion**

**Target:** 5 Pages | **Words:** ~1,000

  

- **6.1 Summary:** A production-ready, daemonized, encrypted P2P system.

- **6.2 Critical Reflection:**

- **The Pivot:** Moving from in-memory storage to SQLite was a key decision for reliability and "Local-First" principles.

- **6.3 Future Work:**

- NAT Traversal (STUN/TURN) for internet-wide usage.

- File Chunking/Sharding for massive datasets.

  

---

  

### **Content Filling Strategy (The "Cheat Sheet")**

  

1. **Systemd Service:** Paste the full `p2p-storage@.service` file in Chapter 4. It fills space and demonstrates professional deployment knowledge.

2. **SQL Schema:** Paste the raw `CREATE TABLE` statements.

3. **Logs:** Use screenshots of your colored terminal logs. They look technical and prove the system works.

- *Caption:* "Figure 4.2: Node receiving Peer Exchange message and updating internal routing table."

1. **Structs:** Paste the Go structs (`MessageStoreFile`, `PeerInfo`). Explain fields line-by-line.