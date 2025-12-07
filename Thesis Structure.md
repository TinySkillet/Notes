- **Day 1 (Theory & Requirements):** Write **Chapters 1 & 2**. (Target: ~20 pages). Focus on why Centralized storage is bad and how Gossip/CAS works.
    
- **Day 2 (The Blueprint):** Write **Chapter 3**. (Target: ~18 pages). Draw the DB Schema and Network Topology diagrams.
    
- **Day 3 (The Code):** Write **Chapter 4**. (Target: ~22 pages). This is the easiest section to fill—paste your Go code (structs, handlers) and screenshots.
    
- **Day 4 (Analysis):** Write **Chapter 5 & 6**. (Target: ~10 pages). Evaluation and Conclusion.
    

---

### **Detailed Table of Contents & Content Guide**

#### **Front Matter** (Pages i - v)

- Title Page, Abstract (Update to mention "Hybrid P2P" and "Gossip Protocol"), Acknowledgements, Table of Contents, List of Figures/Tables.
    

---

#### **Chapter 1: Introduction**

- **Target:** 5 Pages | **Words:** ~1,500
    
- **1.1 Introduction to the Problem:** Centralized cloud issues (privacy, cost, shutdowns).
    
- **1.2 Introduction to the Project:** Define your solution: A **Hybrid Unstructured P2P system**. Mention it uses **Local-First Architecture** (SQLite) and **Custom TCP Streaming**.
    
- **1.3 Aims and Objectives:**
    
    1. Secure Transport (Custom TCP + AES).
        
    2. Resilient Discovery (Gossip Protocol + Bootstrapping).
        
    3. Data Integrity (Content Addressable Storage).
        
    4. Production Viability (Systemd integration/Config management).
        
- **1.4 Scope:** Small trusted groups. Not using libp2p. Hybrid topology (Mesh + Bootstrap).
    

---

#### **Chapter 2: Literature Review & Background**

- **Target:** 12 Pages | **Words:** ~3,500
    
- **2.1 Peer-to-Peer Architectures:**
    
    - Structured (DHTs like Chord/Kademlia) vs. **Unstructured** (Gnutella/Your Project).
        
    - Explain why you chose **Hybrid Unstructured** (simpler for small groups, highly resilient).
        
- **2.2 Data Discovery & Routing:**
    
    - **Gossip Protocols:** Explain the theory of "Peer Exchange." How nodes "infect" each other with peer lists.
        
- **2.3 Storage Paradigms:**
    
    - **CAS (Content Addressable Storage):** Explain SHA-1 hashing (as used in Git).
        
    - **Local-First Software:** The trend of keeping data on user devices (SQLite) vs cloud databases.
        
- **2.4 Transport & Serialization:**
    
    - TCP Streams vs Packets.
        
    - **Serialization:** JSON vs. Protocol Buffers vs. **Go Gob** (Explain why you chose Gob—Go native, fast).
        

---

#### **Chapter 3: System Design (The Blueprint)**

- **Target:** 15 Pages | **Words:** ~3,500 (+ Diagrams)
    
- **3.1 High-Level Architecture:**
    
    - Diagram: Show "New Node" connecting to "Bootstrap Node" then finding "Peer A" and "Peer B".
        
- **3.2 Database Design (The SQLite Schema):**
    
    - Crucial Section: Detail the 4 tables from ABOUT.md:
        
        1. peers (Discovery)
            
        2. files (Metadata)
            
        3. shares (Replication tracking)
            
        4. keys (Encryption)
            
    - Diagram: An ERD (Entity Relationship Diagram) of these tables.
        
- **3.3 The Custom RPC Protocol:**
    
    - Describe the RPC struct.
        
    - Explain the **Stream Mode**: How the connection upgrades from "Command Mode" to "Raw Stream" to handle large files without crashing RAM.
        
- **3.4 Cryptographic Design:**
    
    - Explain the **Stream Cipher** approach (AES CTR/OFB). Why streaming encryption is better than encrypting the whole file in memory.
        
- **3.5 Deployment Architecture:**
    
    - Explain the **Systemd Service** design (p2p-storage@.service). Discuss security hardening (NoNewPrivileges, ProtectSystem).
        

---

#### **Chapter 4: Implementation (The Code)**

- **Target:** 20 Pages | **Words:** ~3,000 (+ Lots of Code/Screenshots)
    
- **4.1 Technology Selection:** Go (Concurrency), SQLite (Persistence), Cobra (CLI).
    
- **4.2 Transport Layer (tcp_transport.go):**
    
    - Show the AcceptLoop.
        
    - Show the Gob decoder logic.
        
- **4.3 The Gossip Protocol (PeerExchange):**
    
    - Code Snippet: Show the logic where a node receives a peer list, filters duplicates, and dials new peers.
        
    - Diagram: Sequence diagram of the "Handshake."
        
- **4.4 Storage Engine & CAS (storage.go):**
    
    - Show the PathTransformFunc (how "abcde..." becomes folders abcde/f12...).
        
    - Show the copyEncrypt function.
        
- **4.5 Database Integration (db/repo.go):**
    
    - Show the SQL queries used to insert a file or find a peer.
        
- **4.6 CLI & Configuration:**
    
    - Explain the Tiered Config (CLI Flag -> Config File -> Defaults).
        
    - Screenshot: The CLI help menu.
        

---

#### **Chapter 5: Testing & Evaluation**

- **Target:** 8 Pages | **Words:** ~2,000
    
- **5.1 Functional Testing:**
    
    - **Scenario A: Bootstrapping.** (Screenshot: Node A joins Node B).
        
    - **Scenario B: The "Gossip" Effect.** (Screenshot: Node A tells Node B about Node C).
        
    - **Scenario C: Streaming Upload.** (Show logs of a large file transferring without memory spikes).
        
- **5.2 Reliability Testing:**
    
    - **Persistence Test:** Kill the node, restart it. Does it remember the peers? (Yes, because of SQLite).
        
    - **Disconnect Handling:** What happens if a peer quits mid-stream? (Discuss the error filtering mentioned in ABOUT.md).
        
- **5.3 Performance Analysis:**
    
    - Discuss the efficiency of **Streaming I/O**.
        
    - Discuss the "First-Come-First-Served" download logic.
        

---

#### **Chapter 6: Conclusion**

- **Target:** 5 Pages | **Words:** ~1,000
    
- **6.1 Summary:** You built a production-ready, daemonized, encrypted P2P system from scratch.
    
- **6.2 Critical Reflection:**
    
    - Pivot: Moving from simple memory storage to SQLite was hard but necessary for "Local-First" reliability.
        
- **6.3 Future Work:**
    
    - NAT Traversal (STUN/TURN).
        
    - File Chunking (revisiting the original idea for massive files).
        

---

### **How to fill pages effectively (The "Cheat Sheet")**

1. **The Systemd File:** Paste the entire p2p-storage@.service file in Chapter 4. Explain what ReadWritePaths does. That is 1.5 pages right there.
    
2. **SQL Schema:** Don't just list the tables. Paste the CREATE TABLE SQL statements.
    
3. **Logs:** Your logs are detailed (timestamps, colored info). Paste screenshots of the logs showing the "Gossip" happening.
    
    - Caption: "Figure 4.2: Node receiving Peer Exchange message and updating internal routing table."
        
4. **RPC Structs:** Paste the Go structs for MessageStoreFile, MessagePeerExchange, etc. Explain what each field does.