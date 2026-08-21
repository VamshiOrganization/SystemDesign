[<~ Back to HLD](../hld.md)

## Distributed Systems:  Search Typeahead Techniques

### **Video Date: (08-09-2026)** and **File Reference:** [hld-class-04-notes.pdf](https://docs.google.com/viewer?url=https://raw.githubusercontent.com/VamshiOrganization/SystemDesign/main/notes/classes/hld/class-04/hld-class-04-notes.pdf) 

### **1. Product & Problem Decomposition** — Pages 1–2

-   **1. Decomposition Mechanics:** Breaking complex products into distinct sub-products (Suggest-as-you-type read path, Popularity tracking write path, Ranking/Compute, and Result serving).
    
-   **2. Repeatable Breakdown Recipe:** Identifying actors/verbs, separating read vs. write paths, characterizing workloads (handling ~20× request amplification), setting 100 ms latency budgets, and defining acceptable staleness.
    

### **2. Scope & Requirements Engineering** — Pages 2–3

-   **3. Ruthless Scoping:** Establishing goals and explicit non-goals (e.g., setting spell correction and wildcards out of scope) to bound complexity.
    
-   **4. Clarifying Questions Framework:** Evaluating interaction axes (charset, output contract, consistency, latency, scale) with questions that directly alter system architecture.
    

### **3. Mathematical Estimation & Purpose-Built Data Structures** — Pages 3–4

-   **5. Decision-Driven Estimates:** Evaluating trade-offs using data (flat string storage ~11 TB/year vs. trie ~13.7 TB/year; 2M read QPS vs. 100K write QPS).
    
-   **6. Purpose-Built Data Structures:** Designing specialized trie structures with precomputed top-10 completions per node ($O(\text{prefix length} + 10)$).
    
-   **Catalog of Custom Structures:** Use cases for Bloom filters, Inverted indexes, LSM trees, HyperLogLog, Geohashes, and Consistent-hash rings.
    

### **4. Write-Path Optimization & Time-Decay Math** — Pages 4–6

-   **7. Precomputation (Write-Time Work):** Shifting heavy computations (sorting/aggregations) from read time to write time.
    
-   **8. The Three-Stage Funnel:** Taming write workloads through sampling (~5% of events), memory buffering/batching, and threshold fast-paths for trending queries.
    
-   **9. Exponential Time-Decay:** Formula-driven popularity decay ($\text{Score} = \text{New} + \frac{\text{Previous}}{1.2}$) for trending accuracy.
    

### **5. Infrastructure Strategies & Data Reshaping** — Pages 6–7

-   **10. Sharding & Skew Management:** Addressing power-law data distribution, hot vs. cold shards, mapping coordination services, and consistent hashing.
    
-   **11. Storage Tiering:** Hot/cold separation (keeping top trie levels in RAM and deep levels on disk).
    
-   **12. Data Reshaping for Managed Stores:** Refactoring complex data structures into key-value paradigms (Prefix $\rightarrow$ Top-10 list) to leverage battle-tested managed databases.
    

### **6. Technical Communication & Summary** — Pages 7–9

-   **13. Interview Communication Framework:** Iterative proposal-and-repair loops, explicit signposting, tying decisions to numbers, and managing rabbit holes.
    
-   **14. Summary Matrix:** Comprehensive reference mapping design concepts (e.g., amplification, buffering, decay, tiering) across Typeahead and real-world system domains.