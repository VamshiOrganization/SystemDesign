**Quorum** is the minimum number of active nodes or servers that must agree on a data operation or system decision before it can be marked as complete. This voting mechanism ensures data consistency, prevents split-brain scenarios, and allows systems to tolerate server failures.

Core Quorum Concepts

-   **Nodes (N):** The total number of copies or replicas of data in the cluster.

-   **Write Quorum (W):** The number of nodes that must successfully receive and store a write before the client gets a success confirmation.

-   **Read Quorum (R):** The number of nodes that must respond to a read request so the client is guaranteed to get the latest data.

-   **The Majority Rule:** Most systems use a simple majority formula to calculate quorum: Quorum =  N/2  + 1 (e.g., in a 5-node cluster, a quorum is 3 nodes). 

Why Quorums Matter

-   **Strong Consistency:** By enforcing the rule R + W > N, any read quorum will overlap with the most recent write quorum, ensuring users never see stale data. 
-   **Fault Tolerance:** Clusters can continue reading and writing data even if some nodes crash or disconnect, as long as the quorum threshold is still met. 
-   **Split-Brain Prevention:** Prevents two isolated parts of a network from independently accepting conflicting writes and corrupting state. 
To see a visual demonstration of how node clusters use a majority vote to stay consistent during failures, watch this [video explanation](https://www.youtube.com/watch?v=jQ1Y4SSQZss&t=1)