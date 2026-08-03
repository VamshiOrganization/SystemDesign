## System Design: Global Weather Data Platform Index
[<~ Back to HLD](../hld.md)

### **1. Problem Statement** — Page 1 [PDF](https://docs.google.com/viewer?url=https://raw.githubusercontent.com/VamshiOrganization/SystemDesign/main/notes/classes/hld/class-02/hld-class-02-notes.pdf)

-   **Scenario Overview:** Global weather stations, decentralized local servers, and 3-way access needs.
    
-   **Core Responsibilities:** Data Collection, Data Distribution, and Query API.
    
-   **Interview Tip:** Restating the problem to surface hidden assumptions and structure the interview.
    

### **2. Requirements** — Page 1

-   **2.1 Functional Requirements (FRs):** Collection (FR1), Distribution (FR2), Nearest-Station Query API (FR3).
    
-   **2.2 Non-Functional Requirements (NFRs):**
    
    -   Scalability (100K stations, 10K agencies, 1M req/day, 1,000 req/sec peak).
        
    -   Availability (99.9%), Durability (2-year retention).
        
    -   Latency ($p99 < 2$ seconds), Eventual Consistency, 100:1 Read-Heavy Workload.
        
-   **Interview Tip:** Establishing NFRs before drawing system diagrams.
    

### **3. Back-of-the-Envelope Estimation** — Page 2

-   **Calculations & Dimensions:**
    
    -   Write Throughput (335 writes/sec avg).
        
    -   Read Throughput (12 req/sec avg, 1,000 req/sec peak).
        
    -   Data Storage Growth (5.8 GB/day $\rightarrow$ ~4.2 TB raw / 12–15 TB with indexes & replicas over 2 years).
        
    -   Report Distribution Egress (1–5 GB snapshots $\times$ 10K agencies).
        
-   **Key Takeaways:** PostgreSQL capacity vs. Regional Edge Server CDN distribution cost justification.
    

### **4. API Design** — Page 2

-   **4.1 Query API (FR3):** `GET /v1/weather` parameters, JSON schema, and nearest-station resolution.
    
-   **4.2 Ingestion API (FR1):** `POST /v1/ingest/readings` authentication, request payload, and `202 Accepted` response.
    
-   **4.3 Distribution APIs (FR2):** Subscriptions, List Reports, and Webhook Notifications with time-limited Signed URLs.
    

### **5. High-Level Design (HLD)** — Pages 3–4

-   **5.1 Data Collection (Write Path):** Push vs. Pull (collector agents) vs. Batch upload $\rightarrow$ Kafka queue $\rightarrow$ Stream Processors $\rightarrow$ Dual-write to Postgres & S3.
    
-   **5.2 Data Distribution (Fan-out Path):** Object storage $\rightarrow$ Distribution/sync service $\rightarrow$ Edge server replication (Americas, Europe, Asia-Pacific) $\rightarrow$ Signed URL notifications.
    
-   **5.3 Query API (Read Path):** API Gateway $\rightarrow$ Weather Query Service $\rightarrow$ Geohash-keyed Redis Cache $\rightarrow$ PostGIS KNN lookup fallback.
    
-   **5.4 Architecture Diagram:** Full cross-functional system architecture diagram.
    

### **6. Low-Level Design (LLD): Database Schema** — Pages 4–5

-   **Database Tables (PostgreSQL + PostGIS):**
    
    -   `weather_stations` (GIST index on location).
        
    -   `readings` (Time-series partitioned by month for 2-year lifecycle drops).
        
    -   `daily_aggregates` (Pre-aggregated query table).
        
    -   `agencies`, `subscriptions`, and `report_snapshots`.
        
-   **6.1 Nearest-Station Query:** PostGIS K-nearest-neighbor operator (`<->`) SQL query implementation.
    

### **7. Metrics, Monitoring & Alarms** — Page 5

-   **Observability Matrix:** Key metrics and alarm thresholds across Ingestion, Processing/Storage, Distribution, Query API, Availability (SLO), and Cost.
    
-   **Tracing & Logging:** Request ID propagation, Grafana dashboards, and PagerDuty integration.
    

### **8. Key Design Decisions & Trade-offs** — Page 6

-   Trade-off analysis matrix covering Kafka, PostgreSQL/PostGIS, Precomputed Daily Aggregates, Edge Replication/Signed URLs, and Geohash-keyed Redis Caching.
    

### **9. How to Present This in an Interview** — Page 6

-   **45-Minute Interview Breakdown:** 7-step roadmap (Clarify $\rightarrow$ Requirements $\rightarrow$ Estimation $\rightarrow$ API Sketch $\rightarrow$ HLD Diagram $\rightarrow$ Deep Dive $\rightarrow$ Wrap-up).
    
-   **Golden Rule:** Connecting every diagram component directly to functional or non-functional requirements.
