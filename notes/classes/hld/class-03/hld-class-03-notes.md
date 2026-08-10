[<~ Back to HLD](../hld.md)

## Distributed Systems: CAP Theorem, PACELC & Consistency Index

### **Video Date: (08-08-2026)** and **File Reference:** [hld-class-03-notes.pdf](https://docs.google.com/viewer?url=https://raw.githubusercontent.com/VamshiOrganization/SystemDesign/main/notes/classes/hld/class-03/hld-class-03-notes.pdf) (Total Pages: 12) 

### **Assignment**
**Problem statement:** Ball-by-ball scores for matches are entered by a small number of official scorers at stadiums. Millions of fans want near-real-time scores on their phones, and third-party apps (news sites, fantasy platforms) want an API. Design the system.
**Functional requirements:** (1) Ingest score updates from scorers at each live match, (2) serve current score + recent ball-by-ball commentary to millions of users, (3) provide an API for third-party apps, (4) notify subscribed users of key events (wicket, century).
**Suggested NFRs:** ~50 concurrent matches, tiny write volume (1 write every few seconds per match) but 50M reads/day with peaks of 500K req/sec during an India match; p99 < 1 sec; eventual consistency acceptable (a score 3 seconds stale is fine); read:write ratio ~1,000,000:1.


### **1. Vertical vs. Horizontal Scaling** — Page 1

-   **Vertical Scaling (Scale Up):** Buying bigger hardware (CPU/RAM/Disk), operational simplicity vs. cost non-linearity, hard hardware ceilings, and single points of failure.
    
-   **Horizontal Scaling (Scale Out):** Spreading workloads across commodity nodes, linear scalability, fault tolerance vs. network failure modes, and distributed system complexity.
    

### **2. Distributed Trade-offs & The Core Problem** — Pages 1–3

-   **System Design Imperatives:** Managing unpredictable hardware failures, network partitions, and propagation delays.
    
-   **The Two-Replica Scenario:** Detailed breakdown of Node A (Bengaluru) receiving writes while Node B (Mumbai) becomes partitioned—illustrated decision paths between serving stale data vs. returning errors.
    

### **3. The CAP Theorem** — Pages 4–6

-   **4. Definition of Terms (Brewer, Gilbert & Lynch):**
    
    -   **C (Consistency):** Linearizability (every read sees the latest committed write or fails).
        
    -   **A (Availability):** Every non-failing node must return a non-error response (without latency bounds).
        
    -   **P (Partition Tolerance):** Ability to operate despite network messages being lost or delayed.
        
-   **5. What "P" Really Means:** Why "CA" is impossible in distributed environments; real-world partition causes (JVM GC pauses, switch failures, network timeouts, firewalls).
    
-   **6. CAP Availability vs. High Availability:** Distinguishing proof-bound CAP availability from production High Availability (SLOs, uptime percentages, latency bounds).
    

### **4. The Consistency Spectrum** — Page 7

-   **7. Consistency Hierarchy:**
    
    -   **Linearizable:** Single global time ordering.
        
    -   **Sequential:** Agreed global order without real-time guarantees.
        
    -   **Causal:** Operation causality maintained across reads.
        
    -   **Session Guarantees:** Monotonic reads and read-your-own-writes.
        
    -   **Eventual Consistency:** Asynchronous convergence without freshness bounds.
        
-   **Hardware Parallel:** CPU memory barriers vs. distributed cross-node coordination.
    
-   **Clarification:** ACID Isolation levels vs. Distributed Replica Consistency models.
    

### **5. PACELC Framework & Database Quadrants** — Pages 8–9

-   **8. PACELC Principle:** **P**artition $\rightarrow$ **A**vailability vs. **C**onsistency; **E**lse $\rightarrow$ **L**atency vs. **C**onsistency.
    
-   **9. The Four PACELC Quadrants:**
    
    -   **PA/EC:** MySQL semi-sync, Kafka (`acks=all`, `min.insync.replicas=1`), MongoDB (`w:majority` with failover).
        
    -   **PC/EC:** ZooKeeper, etcd, Consul, Google Spanner, CockroachDB, Cassandra (`ALL/ALL`), Kafka (`acks=all`, `min.insync.replicas=N`).
        
    -   **PA/EL:** Cassandra defaults (`ONE/QUORUM`), DynamoDB, Riak, MySQL async, Kafka (`acks=0`/`acks=1`).
        
    -   **PC/EL:** Yahoo! PNUTS ("Sherpa").
        
-   **Quorum Intuition:[Quorum.md]** $R + W > N$ formula dynamics and minority partition behavior.
    

### **6. Practical Application & Trade-offs** — Page 10

-   **10. Business Alignment:** Translating engineering constraints ("Down vs. Wrong vs. Slow") to business strategy.
    
-   **Inconsistency Mitigations:** Read repair, compensating transactions (e.g., overdraft fees), append-only reconciliation vs. inventory locks (Flash sales/payments).
    

### **7. Common Doubts & Summary** — Pages 11–12

-   **11. FAQ Resolution:** Detailed answers covering node crashes vs. partitions, loss of data risks in AP systems, tunable consistency in Cassandra, and interview positioning.
    
-   **12. The One-Slide Summary:** Core takeaways covering PACELC, business-driven database configuration, and consistency spectrum trade-offs.