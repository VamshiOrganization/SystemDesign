[← Back to q&a](../q&a.md)

## 2: Handling Infinite Historical Data (Reading Years of Old Data)

You are 100% correct. We cannot store a user's entire 10-year posting history inside an expensive in-memory Redis Sorted Set. If we did, the cost of RAM across 10 million users would be financially unsustainable.

### The Solution: Time-Based Data Tiering (Sliding Window Cache)

We split storage into Hot Data (Memory) and Cold Data (Disk).

#### 1. The Hot Tier (The Cache Window)

Your **Fanout Workers** cap the Redis cache size. For example, they maintain a maximum of **100 to 200 post IDs** per user in the Redis Sorted Set (`ZSET`).

-   Whenever a new post is pushed, the worker adds it to the `ZSET` and then runs a trim command:  `ZREVRANGEBYSCORE` or `ZREMRANGEBYRANK` to drop everything older than the 200th post.
    
-   Since 95% of social media users only scroll through the first few pages of their feed, this handles almost all daily active traffic seamlessly out of memory.
    

#### 2. The Cold Tier (The Database Fallback)

What happens if a user is bored and keeps scrolling down


### Problem 2: Users scrolling back to view years-old data

Your intuition is 100% correct: **We cannot keep infinite historical feed data in Redis.** Redis stores everything in RAM, which is incredibly expensive.

#### The Solution: Capped Caches + Database Pagination

We treat the News Feed Cache as a **Sliding Window**.

1.  **Cap the Redis Cache Size:** A user rarely scrolls through more than 100 or 200 posts in a single session. Therefore, during the Fanout phase, the **Fanout Worker** trims the Redis Sorted Set so it _only_ contains the latest **100 Post IDs** for that user.
```
// Redis command run by the worker to keep only the latest 100 items
ZREMRANGEBYRANK user:feed:user_123 0 -101
```

**The Cache Miss Workflow (Deep Scrolling):** When the user scrolls past the first 100 posts, their client app sends a request for the next page: `GET /v1/me/feed?max_id=post_099&limit=20`.

-   The **News Feed Service** checks Redis for anything older than `post_099`.
    
-   **Cache Miss:** Redis returns empty because the cache is capped at 100.
    
-   **Fallback to Cold Storage:** The service now knows this is a "Deep Scroll" request. It safely bypasses the cache and queries a relational database cluster (like PostgreSQL with Read Replicas) or a wide-column NoSQL database (like Cassandra) that stores the raw timeline records long-term on cheap disk storage (NVMe/SSD).
```
[User scrolls past 100 posts] 
              │
              ▼
    [News Feed Service]
              │
              ├──> Check Redis (Capped at 100 items) ──> [Cache Miss!]
              │
              └──> Fallback to Disk Storage (Cassandra / DB Read Replicas)
                   Query: "SELECT * FROM user_timeline WHERE user_id = X AND timestamp < Y LIMIT 20"

```

#### Why this is safe for the DB:

Only a tiny fraction of users (less than 1% to 5%) actually scroll back months or years into their feed. Because the traffic volume for historical data is so low, your databases can easily handle these paginated disk reads without falling over.