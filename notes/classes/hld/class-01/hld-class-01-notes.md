## High-Level Design (HLD) Fundamentals Index

### **File Reference:** `hld-class-01-notes.pdf` (Total Pages: 11)

### **1. Introduction & Overview** — Page 3

-   **What We Covered in This Class:** Overview of HLD, Design Doc culture, Databases, and Cost Estimation.
    

### **2. Design Documents: Culture and Practice** — Page 3

-   **Engineering Culture at Tech Companies:** Purpose, clarity of thought, and alignment before writing code.
    
-   **Core Practices:** Adaptability across teams (Front-end vs. Back-end vs. Infra), mini vs. full design docs, and peer reviews.
    

### **3. Design Document Template** — Pages 4–6

-   **3.1 Problem Statement:** User/business pain points, urgency ("Why now?"), cost of inaction, and goals/non-goals.
    
-   **3.2 Critical User Journeys (CUJs):** Step-by-step user flows, wireframes, and test baselines.
    
-   **3.3 Timeline and Task Breakdown:** Milestone distribution tables with estimates and dependencies.
    
-   **3.4 High-Level Design (HLD):** Architecture diagrams, data flow, tech stack choices, scale/capacity, cost estimates, and NFRs.
    
-   **3.5 Low-Level Design (LLD):** API contracts, data schemas/indexes, class structures, and failure handling.
    
-   **3.6 Test Plan and Bug Hunt:** Unit/integration/E2E testing, bug hunt environment setups, and P0/P1 exit criteria.
    
-   **3.7 Release Plan:** Staged rollouts ($1\% \rightarrow 100\%$), feature flags, monitoring/alerts, and rollback protocols.
    
-   **3.8 Success Criteria:** Product/business metrics, technical metrics (p95/p99 latency, error rates), and measurement plans.
    
-   **3.9 Optional Sections:** Trade-offs, security/privacy, risk logs, and appendix.
    

### **4. System Design Fundamentals: How the Internet Works** — Page 7

-   **End-to-End URL Journey:** URL Parsing $\rightarrow$ DNS Resolution $\rightarrow$ TCP Handshake $\rightarrow$ TLS Negotiation $\rightarrow$ HTTP Request/Response $\rightarrow$ Server-Side Architecture $\rightarrow$ Browser Rendering.
    
-   **Importance to HLD:** Connecting components like Load Balancers, CDNs, Connection Pools, and Caches to the internet skeleton.
    

### **5. Categories of Databases and Data Infrastructure** — Pages 8–9

-   **Relational Databases (SQL):** PostgreSQL, MySQL.
    
-   **Blob / Object Storage:** Amazon S3, Azure Blob, GCS.
    
-   **Time Series Databases:** InfluxDB, Prometheus, TimescaleDB.
    
-   **Search Databases:** Elasticsearch, OpenSearch, Solr.
    
-   **Vector Databases:** Pinecone, Milvus, Weaviate, pgvector.
    
-   **Wide-Column Databases:** Cassandra, HBase, Bigtable.
    
-   **Caches / In-Memory Databases:** Redis, Memcached.
    
-   **Queues and Streaming Platforms:** RabbitMQ, Apache Kafka.
    
-   **Homework Exercise 1:** Technology deep-dive across categories.
    

### **6. Cost Estimation in High-Level Design** — Page 10

-   **First-Class Design Constraint:** Back-of-the-envelope estimation for economic viability.
    
-   **Key Metrics:** Traffic (RPS), Storage growth, Compute peak headroom, Network egress, and Managed service pricing.
    
-   **Business Unit Metrics:** Cost per 1,000 requests, per active user, or per GB stored.
    

### **7. Cloud Provider Exploration & Service Comparison** — Pages 10–11

-   **Homework Exercise 2:** Hands-on setup with AWS, Azure, and GCP free tiers.
    
-   **Billing Models:** Per-request, Fixed/provisioned, and Usage-based.
    
-   **Cross-Cloud Service Mapping Table:** AWS vs. Azure vs. GCP across Object Storage, VMs, Containers, Serverless, DBs, Caches, Queues, and CDNs.
    

### **8. Homework Summary Table** — Page 11

-   **Summary of deliverables:** Internet journey explanation, Database deep dives, and Cloud hands-on cost calculations.
