[← Back to q&a](q&a.md)

In **Figure 11-6**, the mapping mentioned as `<postId, userId>` actually represents a **tuple of data stored inside a collection**, rather than a standard Key-Value lookup where the `postId` is the main Redis key.

Instead, the **Key is always the Logged-in User’s ID**, and the value is a collection containing the `postId` entries.

#### How the Lookup works in Redis:

We use a **Redis Sorted Set (`ZSET`)**.

-   **The Key:** `user:feed:<userId>` (e.g., `user:feed:vamshi_123`)
    
-   **The Value (Members inside the set):** A list of `postId`s.
    
-   **The Score (Sorting criteria):** The `timestamp` of when the post was created.
    

When a user calls the News Feed Service, the application executes a simple Redis command:

```
ZREVRANGE user:feed:vamshi_123 0 9
```
This single call fetches the top 10 most recent postIds for that specific user in $O(\log(N) + M)$ time (where $N$ is the number of elements in the set, and $M$ is the number of elements returned). Since we strictly cap our cache size at 100 items, $N$ is tiny, making this execution run in sub-millisecond time—essentially acting like an $O(1)$ operational read.

### 2. Deep Dive: Cache Architecture (Figure 11-8)

As a senior backend engineer, you cannot solve everything with just one single cache cluster. In **Figure 11-8**, Alex Xu introduces a multi-layered **Polyglot Cache Architecture**. The system breaks down caching into 5 highly distinct layers

  
| Cache Layer | What is the Key? | What is the Value? | Why Do We Separate It? |  
|---|---|---|---|  
| **1. News Feed Cache** | `user:feed:<userId>` | **Sorted Set** containing `[postId, timestamp]` | Enables ultra-fast retrieval and ranking of a user's timeline without recalculating the feed on every request. |  
| **2. Content Cache** | `post:<postId>` | **Full Post JSON** (text, image URLs, author, metadata, timestamps) | Eliminates frequent reads from the relational database. Typically divided into **Hot Cache** (popular posts) and **Normal Cache** (regular posts). |  
| **3. Social Graph Cache** | `user:followers:<userId>`<br>`user:following:<userId>` | List or Set of related `userIds` | Accelerates friend/follower lookups during feed generation and fanout processing. |  
| **4. Action Cache** | `post:actions:<postId>` | Collections of user interactions (likes, comments, replies, shares) | Stores interaction state efficiently without expensive SQL `JOIN` operations. |  
| **5. Counter Cache** | `post:counters:<postId>` | Integer counters (likes, shares, comments, views) | Supports high-speed atomic updates using Redis commands like `INCR`, avoiding database row locking during viral traffic. |:

### 3. Putting it Together: The Real-Time Hydration Process

When you invoke the read path, the **News Feed Service** orchestrates a pipeline across these specific caches:

```
                  ┌──> [Step 1] Read News Feed Cache (user:feed:123) ────> Returns: [post_99, post_88]
                  │
[News Feed Service]──> [Step 2] Bulk Read Content Cache (MGET post_99, post_88) ─> Returns: Raw Text/Images
                  │
                  └──> [Step 3] Bulk Read Counter Cache (MGET counters) ──> Returns: Like/Reply Counts
                                            │
                                            ▼
                             [Merge into JSON & Return to Client]

```

This multi-tiered caching approach explains how social media apps scale seamlessly. The main timeline lookup stays ultra-lean because it handles nothing but lightweight primitive IDs. The heavy text blocks, dynamic counters, and graph boundaries are isolated into dedicated, specialized key-value spaces.

