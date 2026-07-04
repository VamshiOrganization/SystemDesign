[← Back to q&a](../q&a.md)


When a **Network Partition** happens in a Multi-AZ Master-Replica architecture, you cannot have both perfect consistency and perfect availability. You must choose.

For a **News Feed System**, we almost always prioritize **Availability over Consistency (AP system)**.

Let's break down exactly why, what happens to the nodes, and how Redis handles it.

### The CAP Theorem Choice in a News Feed

-   **Consistency (CP):** If a partition happens, the system refuses the write request because it cannot guarantee all replicas across zones will see it immediately. The user sees an error screen.
    
-   **Availability (AP):** If a partition happens, the system accepts the write on whatever primary node it can talk to, even if that primary cannot replicate the data to the other side of the network partition right away. The user's post goes through, but their friends in the disconnected zone won't see it until the network heals.
    

#### Why we choose AP for News Feeds:

If a user posts "Having a great coffee!" and their friend sees it 5 seconds late because of a network split between AWS Availability Zones, **nobody dies**. The business value of the app is unaffected. However, if the app throws an error and completely blocks users from posting or reading their feeds because of a transient cross-zone network blip, the user experience feels broken.

### How it Works During a Partition: The Split-Brain Problem

Imagine your system is deployed across two Availability Zones (AZ-1 and AZ-2). A backhoe digs up a fiber optic cable, causing a network partition between them.

```
 [ AZ-1 (Isolated) ]                  [ AZ-2 (Isolated) ]
┌────────────────────┐              ┌────────────────────┐
│  Primary Redis A   │   X ─── X    │  Replica Redis B   │
│                    │  Partition   │                    │
│ Can see 30% of app │   X ─── X    │ Can see 70% of app │
└────────────────────┘              └────────────────────┘

```

1.  **The AZ-2 Dilemma:** Replica B realizes it can no longer talk to Primary A. The nodes in AZ-2 run a quorum election. Because AZ-2 holds the majority of the app infrastructure, it promotes **Replica B to be the new Primary B**.
    
2.  **The Split-Brain:** For a brief window, Primary A is still running in AZ-1 accepting writes from a small subset of isolated web servers, while Primary B is running in AZ-2 accepting writes from the rest of the app. You have two primaries at once.
    
3.  **The Healing (Asynchronous Reconciliation):** When the network partition heals, the cluster configurations merge. Redis recognizes that Primary B has the true majority. Primary A is demoted back to a replica.
    
4.  **The Data Loss Trade-off:** Any writes that happened on Primary A during the partition that were never replicated are **overwritten and lost** when it resynchronizes with Primary B.
    

As a senior engineer, you accept this minor risk of data loss (a few missing likes or posts during a rare network outage) to keep the global system up and running.

### Designing for Stronger Consistency (If Required)

If you were designing a banking ledger or an inventory counter instead of a news feed, you would switch to a **CP (Consistency)** mindset.

In Redis, you would enforce this by changing the configuration parameter:

Properties

```
# Don't allow writes if fewer than 'N' replicas acknowledge it
min-replicas-to-write 1
min-replicas-max-lag 10

```

With this turned on, if Primary A loses its connection to its replica during a partition, it will intentionally **reject all incoming writes**, preserving strict consistency at the cost of blocking your users.