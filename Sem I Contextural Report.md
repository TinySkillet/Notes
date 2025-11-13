# ABSTRACT

The amount of digital data around the world is exploding, and so are concerns about privacy, censorship, and dependence on centralized cloud services, making a compelling case for new types of storage architectures. In this research, I propose a decentralized peer-to-peer (P2P) storage system for small-scale or offline-first scenarios. The system uses a Distributed Hash Table (DHT) for fast content retrieval, SHA-256–based content-addressable storage to guarantee content integrity, and configurable redundancy (via replication or optional erasure coding) to provide availability even during node failures. If the libp2p networking library proves to be too complex, a TCP-based fallback protocol will offer a simpler option. Using a simple command-line interface (CLI), users can upload files, retrieve files, and monitor networks with minimal technical knowledge.

# 1. INTRODUCTION

## 1.1. BACKGROUND

Today, centralized cloud storage services like Google Drive and Amazon S3 lead the market. They run on an extensive infrastructure that is monitored 24/7. Still, they are controlled by centralized corporations, and users must trust them with their data.

Decentralized storage services adopt a different approach. In a decentralized P2P storage system (such as IPFS), files or portions of files are securely stored on many different independent nodes. This alleviates censorship issues pervasive in centralized systems as users are not forced to rely on a single source for their data (Benet, 2014; Clarke et al., 2001).

This paper focuses on the development of a custom decentralized P2P storage system, discussing the key challenges involved in such a system, including data distribution, security, and redundancy mechanisms. The study and its findings are intended to contribute to the broader field of distributed storage and to offer a simple approach to secure and reliable data storage.

## 1.2. PROBLEM STATEMENT

Conventional cloud storage services are prone to a single point of failure as well as the watchful eye of the authoritative central entity. In addition, these services have a subscription-based business model where users have to pay for storage beyond a certain threshold. This can get expensive as storage needs grow. While there are decentralized storage solutions like IPFS and Storj that address this, they use cryptocurrency incentives or complex setups, thus not considering users with limited technical backgrounds or those searching for small groups of collaborators. Hence, these challenges require decentralized storage solutions that can effectively address such needs.

## 1.3. PROPOSED SOLUTION

The proposed decentralized P2P storage offers a powerful, cost-effective, and easy decentralized alternative to centralized systems, particularly for low-to-middle scale targeted use cases such as the following:

*   Small research groups that need to share their datasets.
*   Local businesses needing secure file sharing.
*   Community businesses wanting to establish community-controlled data spaces.

By allowing users to contribute and utilize portions of their personal local storage, this system provides a vast amount of total available storage without requiring them to pay directly to a central provider. However, ensuring data performance, redundancy, and availability remains a challenging task. The framework proposed in this study provides a decentralized lookup and retrieval mechanism, a user-friendly CLI interface, and optimized redundancy mechanisms (such as erasure coding).

## 1.4. AIMS AND OBJECTIVES

The primary aim of this research is to develop a decentralized storage network tailored for small-scale, targeted use cases, providing secure data distribution, cost-effectiveness, and user-friendliness.

The key objectives to achieve this aim are:

*   Develop a secure and efficient decentralized P2P storage network for small user groups.
*   Ensure file availability through redundancy despite node failures.
*   Optimize data retrieval efficiency for targeted use case requirements.
*   Evaluate custom TCP as an alternative to libp2p/Kademlia for feasibility.

## 1.5. FEASIBILITY STUDY

The use of existing technologies, such as libp2p for networking, contributes to the viability of the project while also making implementation easier. Libp2p is a modular framework for building peer-to-peer applications, with pre-built components for data transport, connection management, and peer discovery (Woodbeck, 2021). However, the study will consider a simpler approach using a TCP-based communication protocol, should libp2p and Kademlia-based DHT over-complicate implementation. This ensures that the project can still be achieved within the parameters that have been set. Additionally, preliminary studies include video guides and basic implementation details for distributed storage systems employing customized TCP and CAS architecture. Having the option to take an alternative course of action, if needed, is important.

## 1.6. INTELLECTUAL CHALLENGE

Creating the proposed storage system is quite a big task because there are numerous problems. The starting point is that the system must have an instantaneous way for peers to find and communicate with each other for effective data retrieval. Next comes the issue of node redundancy, as some nodes could have bugs or go offline; thus, it is necessary to implement a system that estimates and avoids the loss of normal data from the corrupted node. Balancing the two opposing issues of keeping numerous copies of data and not using extra storage space is also a difficult challenge. Finally, the research will explore and compare existing networking tools like libp2p/Kademlia with the creation of a custom TCP-based system to determine which approach works best.

## 1.7. THESIS FORMAT

| Chapter | Section | Description |
| :--- | :--- | :--- |
| **Chapter 1: Introduction** | 1.1 Background | Introduces the context and general overview of the research area. |
| | 1.2 Problem Statement | Defines the core issue the research addresses. |
| | 1.3 Proposed Solution | Describes the intended approach or idea to resolve the problem. |
| | 1.4 Aims and Objectives | States the main goals and measurable objectives of the study. |
| | 1.5 Feasibility Study | Evaluates the practicality and viability of the proposed solution. |
| | 1.6 Intellectual Challenge | Discusses the complexities and academic challenges involved. |
| | 1.7 Thesis Format | Summarizes the structure and content of the thesis. |
| **Chapter 2: Project Plan** | 2.1 Gantt Chart | Visual timeline of project tasks and milestones. |
| | 2.2 Risk Analysis | Identification and mitigation of potential project risks. |
| | 2.3 Architecture Diagram | Diagram illustrating the system architecture. |
| | 2.4 Workflow Diagram | Diagram illustrating the process flow. |
| **Chapter 3: Literature Review** | 3.1 Overview | Introduction to the literature review section. |
| | 3.2 Review of Existing Projects | Discussion of similar or related past works. |
| | 3.3 Literature Review | Detailed analysis of scholarly articles and sources. |
| | 3.4 Summary of Literature Review | Summary of key findings and gaps in existing research. |
| **Chapter 4: Primary Research** | 4.1 Objective of Primary Research | Purpose and goals of the original research. |
| | 4.2 Survey Questionnaire | Design and details of survey or research instruments. |
| | 4.3 Data Collection and Analysis | Methods used and findings from collected data. |
| **Chapter 5: Artefact Planning** | 5.1 Requirement Analysis | Functional and non-functional requirements. |
| | 5.2 Software Requirements | Specific software needs and specifications. |
| | 5.3 Hardware Requirements | Hardware specifications required for implementation. |
| | 5.4 System Design | Architectural and technical design of the artefact/system. |
| **Chapter 6: Critical Analysis / Reflection** | 6.1 Testing Strategy | Testing methods and validation plans. |
| | 6.2 Evaluation Strategy | Criteria and process for evaluating the system. |
| **Chapter 7: Limitations** | — | Discusses the limitations and constraints encountered. |
| **Chapter 8: Conclusion** | — | Final summary of research outcomes and future work suggestions. |
| **References** | — | Complete list of cited works following academic style (e.g., Harvard). |


# 2. PROJECT PLAN

## 2.1. GANTT CHART

The following charts present the semester-wise breakdown of the project timeline:

### Semester I Timeline
*(Visual representation of the Gantt Chart would be displayed here.)*

### Semester II Timeline
*(Visual representation of the Gantt Chart would be displayed here.)*

## 2.2. RISK ANALYSIS

| Risk | Impact | Likelihood | Mitigation Strategy |
| :--- | :--- | :--- | :--- |
| Libp2p Complexity Exceeds Timeline | High | Moderate | If libp2p is found to be too complex to implement within the timeline, switch to a custom TCP-based networking approach, following available guides. |
| Custom TCP Implementation Issues | Moderate | Moderate | Start with minimal implementation and gradually optimize. Use well-documented approaches to avoid unexpected delays. |
| Data Integrity Issues | High | Low | Implement strong checksums (SHA-256), redundancy, and a data verification mechanism. |
| Security Vulnerabilities | High | Moderate | Implement encryption and design with security in mind. |
| Erasure Coding Implementation Delays | Low | Moderate | Prioritize replication over erasure coding and treat it as an optional stretch goal. |
| Data Inconsistency Across Nodes | High | Moderate | Implement eventual consistency with periodic replication checks. |
| Network Latency / Peer Churn | High | Moderate | Use timeouts, retries, and a peer health-check mechanism to manage connectivity. |
| Performance Bottlenecks | Moderate | Moderate | Optimize data chunking and caching mechanisms. Conduct performance testing early and iteratively improve. |
| Scalability Limitations | Moderate | High | Monitor performance, conduct tests, and design with scalability in mind. |

## 2.3. ARCHITECTURE DIAGRAM

*(An architectural diagram would be displayed here illustrating the decentralized P2P system.)*

The diagram illustrates the process of how a user interacts with the decentralized storage system, and how data is stored and retrieved. The process follows this sequence:

1.  **User Request**: The process begins when a user makes a request through the CLI Interface.
2.  The user request goes to the Node Client.
3.  **Hash File**: The file is then hashed using SHA-256 to ensure data integrity.
4.  **Locate Peers**: The system locates peers in the distributed network using a DHT Client.
5.  **Store Chunks**: The file is divided into chunks and stored across the network.
6.  **Query**: When a user requests the file, the system queries the DHT to find the peers storing the relevant chunks.
7.  **Reassemble**: The Node Client reassembles the chunks.
8.  **Verify Chunks**: The system verifies the integrity of the retrieved chunks.
9.  The reassembled file is presented to the user through the CLI Interface.

## 2.4. WORKFLOW DIAGRAM

The sequence of operations begins when the user provides a command either to upload (`put`) or download (`get`) a file. In the case of uploads, the system first divides the file into parts, then, for each part, it creates a SHA-256 hash code to use as a unique identification number, and after that, it stores them locally. Finally, it updates the DHT to reflect where those files can be found. During downloads, the system searches for piece locations on the DHT, gets them from other nodes, checks their integrity with the hash code, and then combines them into the original file. After that, the user is given the file. Storage as well as retrieval techniques are made possible through the content-addressed method, and their implementation over the peer-to-peer network results in efficiency and fault tolerance.

Alongside the user operations, a background process known as the **Replication Manager** works concurrently. This process takes care of the network by sending heartbeat signals to peers to check their connectivity status. If a peer goes offline, it detects the failure and ensures that any affected chunks are available from other replicas. It then replicates those chunks to other online nodes and updates the DHT with the new location of the storage. If no peers go offline, the system just continues with monitoring. This active replication keeps the data accessible even in the case of some nodes leaving the network.

---

# 3. LITERATURE REVIEW

## 3.1. OVERVIEW

In contrast to centralized systems, decentralized P2P storage systems pose several major advantages. They are recommended for their effective handling of problems like single points of failure, data privacy concerns, and limited scalability. Protocols such as HTTP that are normally utilized in central systems fail to distribute files effectively in a decentralized environment. That is why the first researchers sought out distributed protocols such as InterPlanetary File System (IPFS) (Benet, 2014) and collaborative file systems like CFS (Dabek et al., 2001). Blockchain, fault-tolerant systems, and the development of superior encryption techniques have all contributed to a significant rise in security and reliability (Tran et al., 2021; Islamay et al., 2022). Therefore, these systems have already found their purposes in education, government, and collaborative networks for fostering transparency, user control, and protection against cyber threats (Khayrutdinov et al., 2024).

## 3.2. REVIEW OF EXISTING PROJECTS

### 3.2.1. INTERPLANETARY FILE SYSTEM (IPFS)

The InterPlanetary File System (IPFS) provides a means for decentralized file sharing (Benet, 2014). Every document is assigned a unique, content-based address, ensuring accurate identification and integrity verification. Network nodes work together in a distributed hash table, which ensures fast location and retrieval of files (Bocek & Stiller, 2007). The design mitigates attacks from failures and censorship. However, retrieval times can vary due to network congestion and the availability of nodes (Athreya et al., 2021).

### 3.2.2. FILECOIN

Filecoin is an IPFS project that includes an economic incentive layer (Protocol Labs, 2017). It is essentially a storage network in which storage providers of different data files receive Filecoin tokens after securely proving the cryptographic evidence of ongoing data retention. Clients and providers create a storage contract on the blockchain, ensuring transparency and trustworthiness. While the model is participatory, file retrieval may experience higher latency compared to existing centralized network services (Pamies Juarez et al., 2010).

### 3.2.3. FREENET

Freenet offers a privacy-focused platform for anonymous file sharing (Clarke et al., 2001). The system breaks each file into fragments, encrypts them, and shares the fragments among various nodes. The request for a file is often routed through several intermediary nodes to conceal the requester's identity. This multipath, clear-path anonymity layer can be indirect and may have side effects, such as reduced access speed. Additionally, the unavailability of data and low retrieval rates can be issues associated with limited network usage.

## 3.3. DETAILED LITERATURE REVIEW

### 3.3.1. Peer-to-Peer Distributed Storage Using InterPlanetary File System (Athreya et al., 2021)
This paper considers the role of IPFS as a decentralized storage solution, demonstrating its benefits over HTTP in terms of data reliability, fault tolerance, and non-repudiation. The content-addressable architecture of IPFS ensures the availability of data across distributed nodes, thereby reducing dependence on centralized servers. Although the paper emphasizes IPFS's applicability to high-availability operations, such as academic and government data management, it does not include empirical performance testing. This work will build on the benefits of IPFS in content addressing but will prioritize performance issues relevant to small-scale use cases.

### 3.3.2. Decentralized File Sharing (Vaidya et al., 2023)
This work focuses on decentralized file sharing for mobile devices. The authors assert that distributing data among mobile nodes addresses security and privacy concerns typically associated with centralized cloud storage. Yet, the research paper is silent on some of the real breaches observed in daily situations such as slow network speeds and bandwidth limits that are frequently experienced in mobile environments. This research acknowledges these limitations and will consider them in the design and evaluation of the proposed system.

### 3.3.3. Decentralization without Blockchains (Gorai, 2024)
This research paper propagates the idea of decentralizing the system without a blockchain, highlighting the efficiency of classic distributed systems. The author illustrates the possibility of reliable operation using fault-tolerant structures such as DHTs, which are free from the computational cost of blockchain, thus correlating with this research's objective of being lightweight and efficient. The study is very useful to applications that are sensitive to costs, which is a main factor in small-scale cases.

### 3.3.4. A Novel Approach for Developing Decentralized Storage and Sharing Systems (Tran et al., 2021)
The paper presents a dual-mode integrated system involving IPFS, blockchain, and encryption algorithms. The adoption of Multi-Authority Attribute-Based Encryption (MA-ABE), which offers the authors the multi-tenant capability for precise access control, is a significant advantage. However, the work does not take into consideration the fact that data privacy enhancement may incur the cost of computational power, as MA-ABE can be computationally intensive, especially for small-scale use cases.

### 3.3.5. A peer-to-peer storage system design (Hu, 2022)
This research work investigates P2P systems' storage efficiency by employing file segmentation and Reed-Solomon codes for redundancy. The researchers demonstrate that file division not only optimizes storage use but also increases the system's fault tolerance. Nevertheless, this method typically increases storage overhead by storing duplicate data. The current research work will also include redundancy mechanisms to assure data availability but will seek to prioritize the minimization of storage overhead, which is one of the main concerns in resource-limited environments.

### 3.3.6. Peer-to-Peer Large-Scale Collaborative Storage Networks (Bocek & Stiller, 2007)
The research presented here is among the first to investigate collaborative storage networks, its main proposal being the use of a voting mechanism for ensuring the integrity of the data in the network. Its provision of a strong incentive for participation is a groundbreaking aspect of this idea, while, on the other hand, its dependence on peer consensus presents challenges in conflict resolution. This research will investigate this issue with the help of content-addressable storage, which offers a reliable and automated verification method.

### 3.3.7. Secure decentralized storage networks (Greig, 2017)
This study critically analyzes decentralized storage networks, especially MaidSafe, and mainly addresses issues related to client-side vulnerabilities. The author expresses that the main weak points of the system are the endpoints that do not provide sufficient security. Apart from the urgent problems being highlighted, the analysis does not provide much help in solving them. This research, apart from its security claims, is going to present a network design and evaluation that will address the issues of both the client and the network.

### 3.3.8. Decentralized File Storage System (Tiwari et al., 2024)
A decentralized alternative to Google Drive is proposed in this paper using a P2P architecture that promotes privacy and availability. The implementation details or performance measures of the study are not provided as it remains theoretical. This research aims to fulfill this gap by offering a practical implementation and assessing its performance from a small-scale use case perspective.

### 3.3.9. The Model and Application of Data Storage in a Decentralized Network (Khayrutdinov et al., 2024)
This paper is centered on the deployment of decentralized storage in the education sector, particularly dealing with smart contracts automating data transactions. Besides the development of a user-oriented decentralized application (dApp) for file management in universities, the research does not discuss the issues caused by user resistance to the technology.

### 3.3.10. Decentralized identifiers for peer-to-peer service discovery (Farmer et al., 2021)
The paper presents decentralized identifiers (DIDs) for P2P network service discovery. Although it is original, the framework does not have any implementation details, especially for load balancing. This research will consider load distribution within the scope of small-scale use cases.

### 3.3.11. Implementation of Blockchain and Peer-to-peer Network for Digital Document Management (Islamay et al., 2022)
The purpose of this study is to present blockchain as a new technology for digital document management and, in the process, showcase the extent to which records that cannot be changed by the user enhance legal authenticity. Network congestion is the primary reason behind the testing results that showed limitations in transaction speed. This research investigates the case of decentralization without the use of blockchain to reduce overhead and increase performance for cases where legally strong authenticity is not the principal issue.

### 3.3.12. Content Addressed, Versioned, P2P File System (Benet, 2014)
This initial paper presents the revolutionary IPFS, a content-addressable P2P protocol. Apart from being a powerful alternative to HTTP, IPFS has its shortcomings, like a lack of built-in incentives and being not very efficient in managing updates. This research builds upon the core principles of content addressing from IPFS.

### 3.3.13. Filecoin (Protocol Labs, 2017)
Filecoin, constructed on IPFS, offers a storage service with the help of an economic model. Adoption, though, is jeopardized by issues such as token price fluctuation and elevated hardware needs. Although the current study recognizes the role played by Filecoin in subsidized storage, its aspiration is to create a system that is less dependent on hardware resources and is more user-friendly for people with limited capabilities.

### 3.3.14. Availability and redundancy in harmony: Measuring retrieval times in P2P storage systems (Pamies-Juarez et al., 2010)
The paper assesses the optimization of redundancy applied to erasure coding. Erasure coding is a very useful storage mechanism; however, it also entails some extra builds. This research is going to be using redundancy mechanisms that are implemented in a way that makes the data available, not just focusing on the storage overhead and the complexity of realization but also on the requirements laid down and the preferences of small deployments.

## 3.4. SUMMARY OF LITERATURE REVIEW

The reviewed literature highlights the evolution of decentralized storage systems, with IPFS and Filecoin establishing standards for content addressing and incentive models. Early systems like Freenet and CFS underscore the ongoing challenges in balancing privacy, performance, and scalability. Erasure coding and adaptive redundancy strategies offer solutions to storage efficiency but require further refinement.

### KEY INSIGHTS

1.  **Efficiency of DHTs**: DHTs offer a viable solution for decentralized lookup and retrieval, as demonstrated by early systems like Freenet and CFS and their continued relevance in more recent work.
2.  **Redundancy Trade-offs**: Redundancy mechanisms, while crucial for data availability, introduce trade-offs in storage overhead and implementation complexity, requiring careful consideration for specific use cases.
3.  **Privacy vs. Performance**: Content-addressable storage, as pioneered by IPFS, provides a robust method for ensuring data integrity and efficient retrieval in decentralized environments.

### CRITICAL GAPS

1.  **Optimized Redundancy for Small-Scale Systems**: There is a need for further research on redundancy mechanisms tailored to the specific constraints and requirements of small-scale decentralized storage systems.
2.  **Performance Evaluation in Realistic Scenarios**: More empirical studies are needed to evaluate the performance of decentralized storage systems in realistic network conditions and under varying workloads.
3.  **Usability and Accessibility**: Further investigation is required to enhance the usability and accessibility of decentralized storage systems, particularly for users in non-technical fields.

## 3.5. JUSTIFICATION

I decided to use Go because of its simple syntax and robust support for network programming. Its goroutines and channels make concurrent programming trivial. Deployment is simple with a single compiled binary. For networking, I opted for libp2p. It is modular and comes with an out-of-the-box Kademlia DHT. If I am unable to understand libp2p, then I will instead follow other tutorials to build a custom TCP example. A stretch goal is Reed–Solomon erasure coding. It may increase availability by adding redundancy. If I have time, I might add it once the core system is up and running. However, I will use Kanban for development. Kanban enables me to plan and be flexible when needs alter.

---

# 4. PRIMARY RESEARCH

## 4.1. OBJECTIVE OF PRIMARY RESEARCH

The purpose of primary research was to understand user preferences, concerns, and requirements as well as familiarity with decentralized P2P storage solutions. The specific objectives include:

1.  Assess current file storage habits and pain points
2.  Evaluate user attitudes toward data privacy and security
3.  Gauge interest in and openness to decentralized storage solutions
4.  Determine users' technical comfort levels with relevant technologies
5.  Identify desirable features for a potential decentralized storage product
6.  Understand users' backup behaviors and data protection habits

## 4.2. MARKET RESEARCH

Several decentralized storage solutions currently exist in the market:

1.  **IPFS**: A protocol designed to create a permanent and decentralized method of storing and sharing files, addressing the HTTP protocol's limitations.
2.  **BitTorrent**: While primarily known for file sharing, BitTorrent represents one of the most widely adopted P2P technologies for distributing data.
3.  **Filecoin**: Built on top of IPFS, it's a blockchain-based cooperative digital storage and data retrieval method.
4.  **Storj**: An open-source cloud storage platform that encrypts files and distributes them across a decentralized network.

These existing solutions have varying degrees of adoption and face challenges including:

*   Technical complexity limiting mainstream adoption
*   Balancing security with usability
*   Ensuring adequate node availability and reliability
*   Managing performance expectations compared to centralized solutions
*   Building trust in novel decentralized architectures

---

# 5. REQUIREMENT ANALYSIS

## 5.1. FUNCTIONAL REQUIREMENTS

Below are the functional requirements categorized using the MoSCoW notation:

| CATEGORY | REQUIREMENT | MoSCoW PRIORITY |
| :--- | :--- | :--- |
| **P2P Networking** | The system must allow nodes (peers) to connect and communicate without a central server. | MUST |
| | Nodes should be able to discover and connect to other peers dynamically. | MUST |
| | The system must support peer disconnection and reconnection while maintaining data availability. | MUST |
| **File Storage and Retrieval** | Users must be able to upload files to the network. | MUST |
| | Files must be split into chunks and distributed across multiple peers. | MUST |
| | Users should be able to retrieve their files efficiently. | MUST |
| | The system must verify data integrity during retrieval. | MUST |
| **Content Addressing** | Files should be identified using cryptographic hashes. | MUST |
| | The system must support hash-based lookup for retrieving stored files. | MUST |
| **Redundancy and Data Availability** | The system must implement replication (or erasure coding) to prevent data loss. | MUST |
| | The system should periodically check and restore missing or corrupted data. | SHOULD |
| **Security and Access Control** | Files must be protected from unauthorized access. | MUST |
| | Only the uploader (or authorized users) should be able to retrieve and decrypt files (if encryption is implemented). | SHOULD |
| | The system should prevent unauthorized modifications of stored data. | SHOULD |
| **Efficient Routing and Lookup** | The system must efficiently locate file chunks in the network using a Distributed Hash Table (DHT). | MUST |
| | Queries should return results in a reasonable time, even as the network grows. | SHOULD |
| **User Interaction and CLI** | Users must be able to interact with the system via a CLI. | MUST |
| | Commands should include uploading, downloading, listing stored files, and checking network status. | MUST |

## 5.2. NON-FUNCTIONAL REQUIREMENTS

Below are the non-functional requirements categorized using the MoSCoW notation:

| CATEGORY | REQUIREMENT | MoSCoW PRIORITY |
| :--- | :--- | :--- |
| **Performance & Scalability** | The system must handle a growing number of peers and files without significant performance degradation. | MUST |
| | The system should optimize bandwidth usage for data transfers. | COULD |
| **Fault Tolerance** | The system should handle peer failures without causing data loss. | SHOULD |
| | A mechanism should exist to detect and redistribute orphaned data chunks. | COULD |
| **Logging & Monitoring** | The system should log key events (e.g., file uploads, retrievals, node joins, and failures). | SHOULD |
| | Users should be able to check storage status and system health. | COULD |

## 5.3. SOFTWARE REQUIREMENTS

Below are the software requirements categorized into respective categories:

| CATEGORY | REQUIREMENT | DESCRIPTION |
| :--- | :--- | :--- |
| **Programming Language** | GO (Golang) | The system will be implemented using Go due to its efficiency, concurrency support, and strong networking libraries. |
| **Networking Library** | Libp2p (or custom TCP if needed) | Used for peer-to-peer networking, node discovery, and data routing. If libp2p is too complex, a custom TCP-based solution will be implemented. |
| **Database** | No centralized database | The system is decentralized, relying on a distributed hash table (DHT) for data indexing and retrieval. |
| **Storage Mechanism** | Content-Addressable Network (CAS) | Files will be stored using cryptographic hashing for unique identification and retrieval. |
| **Data Verification** | SHA-256 | Used to ensure data integrity and prevent unauthorized access. |
| **Development Tools** | Visual Studio Code | Preferred IDE for development. |
| **Version Control** | Git & GitHub | For managing source code. |
| **Testing Framework** | Go Testing Package | Built-in testing framework in Go for unit and integration testing. |
| **Dependency Management** | Go Modules | Manages dependencies and ensures reproducibility across different environments. |
| **Operating System** | Windows | The system will be developed in Windows. |

## 5.4. HARDWARE REQUIREMENTS

### 5.4.1. DEVELOPMENT

The system will be developed on a personal computer with the following specifications:

| COMPONENT | SPECIFICATION |
| :--- | :--- |
| **Processor** | Intel Core i7 – 1260P |
| **RAM** | 16 GB DDR5-4800 |
| **Storage** | 1 TB SSD |
| **Operating System** | Windows 11 |
| **Network** | Stable internet connection |

### 5.4.2. DEPLOYMENT

Since the system is decentralized, there is no central server for deployment. Instead, users will run the software on their own devices. The minimum speculated hardware required for running a peer node is:

| COMPONENT | MINIMUM REQUIREMENT |
| :--- | :--- |
| **Processor** | Intel Core i3 8th Gen or equivalent |
| **RAM** | 4 GB |
| **Storage** | 128 GB HDD/SSD |
| **Operating System** | Windows, Linux |
| **Network** | Stable internet connection |

For testing, cloud-based virtual machines could be used to simulate multiple peers.

## 5.5. SYSTEM DESIGN

### 5.5.1. USE CASE DIAGRAMS

*(Use case diagrams would be displayed here.)*

### 5.5.2. DATABASE SCHEMA

Since the system is a decentralized P2P storage system, it does not use a regular centralized database. Instead of using a relational or NoSQL database, data is distributed to multiple peers using a content-addressable storage (CAS) model (Benet, 2014; Bocek & Stiller, 2007). Files are processed into chunks, hashed with the SHA-256 algorithm, and distributed among the nodes in the network (Bocek & Stiller, 2007).

A distributed hash table (DHT) is used to maintain metadata such as chunk locations and redundancy information so that peers can efficiently find and retrieve stored data (Dabek et al., 2001). With that, the system has no single point of failure and has increased resilience (Clarke et al., 2001).

---

# 6. TESTING / EVALUATION PLAN

## 6.1. TESTING STRATEGY

This section outlines the testing methods that would be used if the system were implemented.

### 6.1.1. UNIT TESTING

*   **What will be tested**: Individual system functions like file chunking and data hashing.
*   **Method**: Use Go’s testing framework.

### 6.1.2. INTEGRATION TESTING

*   **What will be tested**: System component interactions like file storage and peer communication.
*   **Method**: Set up multiple nodes and check if they can share and retrieve files properly.

### 6.1.3. PERFORMANCE TESTING

*   **What will be tested**: System performance under different network conditions.
*   **Method**: Measure speed and responsiveness with tools like Apache JMeter or GoBench.

### 6.1.4. FAULT TOLERANCE TESTING

*   **What will be tested**: The system’s ability to handle peer failure and data loss.
*   **Method**: Simulate node failure and check if the system can restore data from other peers.

### 6.1.5. SECURITY TESTING

*   **What will be tested**: System security, including encryption and unauthorized access prevention.
*   **Method**: Use security audits and testing tools.

## 6.2. EVALUATION STRATEGY

The evaluation strategy focuses on how the system will be assessed once it is implemented.

### 6.2.1. FUNCTIONALITY

*   **What will be evaluated**: If the core features work as expected.
*   **Method**: Test data chunking, file retrieval, and peer communication.

### 6.2.2. PERFORMANCE

*   **What will be evaluated**: System speed and reliability.
*   **Method**: Test how quickly files are retrieved and how the network performs under load.

### 6.2.3. SCALABILITY

*   **What will be evaluated**: How well the system handles more peers and data.
*   **Method**: Test how the system performs as the number of peers grows.

### 6.2.4. RELIABILITY

*   **What will be evaluated**: The system’s ability to recover from node failures.
*   **Method**: Simulate node failures and check if data is still accessible.

### 6.2.5. USABILITY

*   **What will be evaluated**: How easy it is for users to interact with the system.
*   **Method**: Gather feedback from users on their experience with uploading and retrieving files.

### 6.2.6. SECURITY

*   **What will be evaluated**: How well the system protects data.
*   **Method**: Use penetration testing to find security flaws.

---

# 7. CRITICAL ANALYSIS AND IMPLEMENTATION PLAN

## 7.1. CRITICAL ANALYSIS

The proposed system relies on peer availability, meaning data retrieval may be affected if many peers go offline. Since there is no central server, file availability depends on how many nodes store and share the data. If too many peers leave the network, some files or chunks may become temporarily inaccessible until redundancy mechanisms restore them (Dabek et al., 2001).

Another challenge is network latency, which could impact performance in real-world deployments. Because data is stored across multiple peers, retrieval times depend on factors such as network speed, peer location, and system load. In a distributed environment, delays in routing or retrieving data could lead to slower performance compared to centralized storage solutions (Bocek and Stiller, 2007).

## 7.2. IMPLEMENTATION

The choice between libp2p and a custom TCP-based solution presents a complexity trade-off. While libp2p provides built-in networking and DHT functionalities, it also introduces additional overhead and may require extensive configuration to meet performance goals. On the other hand, implementing a custom TCP-based solution could offer greater control but would require significant development effort. The decision between these approaches will ultimately depend on balancing complexity, efficiency, and time constraints.

Another important consideration is peer churn—the frequent joining and leaving of nodes. To reduce the risk of data unavailability during high churn, I plan to maintain a few always-on nodes, possibly hosted in the cloud. These nodes will act as stable peers that store critical chunks. This can help ensure minimum availability even if many regular peers go offline (Pamies-Juarez et al., 2010).

While content is addressed using SHA-256 hashes, the number of replicas required for reliable retrieval is not fixed. I will determine the minimum number of peers each chunk should live on through testing. This threshold will help balance availability and storage efficiency.

---

# 8. CONCLUSION

This report argues for decentralized P2P storage systems as a viable replacement for traditional centralized cloud storage. This system works differently from conventional cloud services, which depend on a centralized authority for data management. This decentralized option improves data redundancy and eliminates reliance on a single provider, providing a resilient and censorship-resistant solution.

While systems like IPFS and Storj already exist in this domain, they are often complex to set up and maintain. The goal of this research is not to compete with these large-scale solutions but to provide a lightweight and easy-to-use alternative for users who need a decentralized storage system with minimal overhead. By focusing on simplicity and efficiency, the system is designed to be accessible to users who may not have extensive technical expertise.

An early focus for updates will be on security and performance. The notable addition is that all stored data will be encrypted using AES to maintain confidentiality. The prospect of a custom TCP-based solution will continue to be assessed to see if it could potentially be a more efficient option than libp2p (while preserving core aspects of decentralization).

As decentralized storage grows more relevant amid a “data privacy crisis” and the rise of cloud computing service monopolies, this research adds to a body of work studying alternative methods of storing data. The results of this study may provide the basis for further improvements in peer-to-peer storage and decentralized network protocols.

---

# 9. REFERENCES

Athreya, A.M. et al. (2021) ‘Peer-to-Peer Distributed Storage Using InterPlanetary File System’, in. Springer, Singapore, pp. 711–721. Available at: https://doi.org/10.1007/978-981-15-3514-7_54.

Benet, J. (2014) *IPFS - Content Addressed, Versioned, P2P File System*. Available at: https://ipfs.io/.

Bocek, T., & Stiller, B. (2007). *Peer-to-Peer Large-Scale Collaborative Storage Networks* (pp. 225–228). Springer, Berlin, Heidelberg. Available at: https://doi.org/10.1007/978-3-540-72986-0_31.

Clarke, I., Sandberg, O., Wiley, B. and Hong, T.W. (2001) ‘Freenet: A distributed anonymous information storage and retrieval system’, in *International Workshop on Designing Privacy Enhancing Technologies*.

Dabek, F., Kaashoek, M.F., Karger, D., Morris, R. and Stoica, I. (2001) ‘Wide-area cooperative storage with CFS’, in *ACM SOSP*.

Farmer, C., Pick, S. and Hill, A. (2021) ‘Decentralized identifiers for peer-to-peer service discovery’, pp. 1–6. Available at: https://doi.org/10.23919/IFIPNETWORKING52078.2021.9472201.

Gorai, S. (2024) ‘Decentralization without Blockchains’, pp. 24–32. Available at: https://doi.org/10.4324/9781003507352-3.

Greig, P. (2017) ‘Secure decentralized storage networks’. Available at: https://pureportal.strath.ac.uk/en/publications/secure-decentralised-storage-networks.

Hasan, R., Anwar, Z., Yurcik, W., Brumbaugh, L., & Campbell, R. (2005). ‘A survey of peer-to-peer storage techniques for distributed file systems’. *International Conference on Information Technology: Coding and Computing (ITCC’05) - Volume II*, Las Vegas, NV, USA, 205–213. Available at: https://doi.org/10.1109/ITCC.2005.42.

Hassanzadeh-Nazarabadi, Y., Küpçü, A., & Özkasap, Ö. (2018). ‘Decentralized and locality aware replication method for DHT-based P2P storage systems’. *Future Generation Computer Systems*, 84, 32–46. ISSN 0167-739X. Available at: https://doi.org/10.1016/j.future.2018.02.007.

Hassanzadeh-Nazarabadi, Y., Küpçü, A., & Özkasap, Ö. (2020). ‘Decentralized utility- and locality-aware replication for heterogeneous DHT-based P2P cloud storage systems’. *IEEE Transactions on Parallel and Distributed Systems*, 31(5), 1183–1193. doi: 10.1109/TPDS.2019.2960018. Available at: https://doi.org/10.1109/TPDS.2019.2960018.

Hassanzadeh-Nazarabadi, Y., Taheri-Boshrooyeh, S., Otoum, S., Ucar, S., and Özkasap, Ö. (2021). *DHT-based communications survey: Architectures and use cases*. arXiv preprint arXiv:2109.10787. Available at: https://doi.org/10.48550/arXiv.2109.10787.

Hu, X. (2022) ‘A peer-to-peer storage system design’, *4th International Conference on Information Science, Electrical, and Automation Engineering (ISEAE 2022)*, 12257, p. 122573L. Available at: https://doi.org/10.1117/12.2640216.

Islamay, B.F., Purwanto, Y.P. and Ruriawan, M.F. (2022) ‘Implementation of Blockchain and Peer-to-peer Network for Digital Document Management’, pp. 1–6. Available at: https://doi.org/10.1109/ICACNIS57039.2022.10055538.

Khayrutdinov, D.D., Volkov, D.A. and Mukhina, A.G. (2024) ‘The Model and Application of Data Storage in a Decentralized Network’, pp. 1840–1843. Available at: https://doi.org/10.1109/edm61683.2024.10615052.

Kothari, R., Jakheliya, B., & Sawant, V. (2019). ‘A distributed peer-to-peer storage network’. *2019 International Conference on Smart Systems and Inventive Technology (ICSSIT)*, Tirunelveli, India, 576–582. doi: 10.1109/ICSSIT46314.2019.8987748. Available at: https://doi.org/10.1109/ICSSIT46314.2019.8987748.

Pacitti, E., Akbarinia, R., & El-Dick, M. (2012). *P2P techniques for decentralized applications*. Synthesis lectures on data management. San Rafael: Morgan & Claypool Publishers. ISBN 1608458229, 9781608458226.

Pamies-Juarez, L., García-López, P., & Sánchez-Artigas, M. (2010). ‘Availability and redundancy in harmony: Measuring retrieval times in P2P storage systems’. In *2010 IEEE Tenth International Conference on Peer-to-Peer Computing (P2P)*, Delft, Netherlands, 1–10. Available at: https://doi.org/10.1109/P2P.2010.5570002.

Protocol Labs (2017) *Filecoin: A Decentralized Storage Network*. Available at: https://filecoin.io/filecoin.pdf.

Tiwari, K., Dhanda, N. and Verma, R. (2024) ‘Decentralized File Storage System’, pp. 1–8. Available at: https://doi.org/10.1109/icccnt61001.2024.10725259.

Tran, C.-T. et al. (2021) ‘A Novel Approach for Developing Decentralized Storage and Sharing Systems’, pp. 85–15. Available at: https://doi.org/10.1145/3457337.3457845.

Vaidya, C. et al. (2023) ‘Decentralized File Sharing’, pp. 1–6. Available at: https://doi.org/10.1109/SCEECS57921.2023.10062977.

Woodbeck, A. (2021). *Network programming with Go*. San Francisco: No Starch Press. ISBN 978-1-7185-0088-4 (print), ISBN 978-1-7185-0089-1 (ebook).

YouTube. (2024). *Decentralized storage*. Available at: https://www.youtube.com/watch?v=rOtMGJVp2MU.

YouTube. (2024). *Lecture 1: Introduction*. Available at: https://www.youtube.com/watch?v=cOP8WApzIQO&list=PLRw6alwE39_tb2fER14-WkMbsVGQk9_UB&index=2.

YouTube. (2024). *What is decentralized storage?* Available at: https://www.youtube.com/watch?v=MNMVAva4vJFM&t=4s.

---

# 10. APPENDIX

## PROJECT PROPOSAL

### Part 1 – Project Proposal

*   **Student Name**: Aditya Poudel
*   **Student Number**: 2216027
*   **Degree Pathway (course)**: BSc. (Hons) Computer Science and Software Engineering
*   **Supervisor Name**: Tara G.C.
*   **Title of Project**: Decentralized Peer-to-Peer Data Storage System

#### Abstract of the project

This research aims to investigate a peer-to-peer data storage system where users share their storage space across a distributed network. Unlike traditional storage, which relies on central servers, the system fragments files and stores them across multiple nodes with built-in redundancy. This ensures that files remain available even when some computers drop off the network. A Distributed Hash Table (DHT) will be employed for peer discovery and file location. The system will be built using Go, leveraging its concurrency capabilities. A simple text-based interface will be created to demonstrate how the system works. The research includes plans for erasure coding as a stretch goal, which would provide a smarter way to create backups than making full copies. By leveraging a decentralized architecture, the system seeks to provide a scalable, resilient, and efficient alternative to traditional storage solutions.

#### Project deliverables

*   Complete Application - Decentralized Peer-to-Peer Storage System
*   Contextual report
*   Final Report
*   Poster

#### Description of your artefact

The proposed decentralized peer-to-peer (P2P) storage system breaks away from the traditional server-client centralized model. Instead of storing everything on a server, the data is spread across a network of independent peers who share their storage space. This approach has real advantages:

*   The system stays up and running even when some nodes go offline since there is no single point of failure.
*   As more people join, the network naturally grows stronger without needing major infrastructure upgrades.
*   Using storage that is already out there avoids the costs of building and maintaining data centers.

This research would benefit users who want to protect their data against big companies that provide a centralized storage system. The primary motivation behind this research is the potential for people, especially in Nepal, to benefit from this kind of decentralized solution that enhances privacy and provides greater control over their own data.

#### Aim & Objectives

**Aim**: To build a scalable and secure decentralized peer-to-peer storage system.

**Objectives**:

*   Implement a DHT overlay for peer discovery.
*   Enable file chunking and distributed storage with integrity verification.
*   Develop redundancy mechanisms to ensure data availability.
*   Support efficient data retrieval and reconstruction.
*   Provide a CLI for user interaction.

#### List of Features

**Core Features**:

*   DHT-based peer network using libp2p or Kademlia.
*   File chunking and cryptographic hashing.
*   Redundancy via configurable replication or erasure coding.
*   Peer health-checks and basic retry mechanisms.

**Advanced Features (Stretch Goals)**:

*   Erasure coding for reduced storage overhead.
*   AES (Advanced Encryption Standard) encryption for chunk privacy.
*   Automated redundancy monitoring and lazy repair policy for recovery.