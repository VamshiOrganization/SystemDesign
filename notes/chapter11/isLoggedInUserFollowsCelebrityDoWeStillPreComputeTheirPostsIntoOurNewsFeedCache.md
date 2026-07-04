[← Back to q&a](q&a.md)

**Q)What happens to this retrieval flow if one of the people the user follows is a massive celebrity with 50 million followers? Do we still pre-compute their posts into our user cache?**

No, **we absolutely do not pre-compute a celebrity's posts into the followers' user caches.** If a celebrity with 50 million followers (like Virat Kohli or Elon Musk) clicks "Publish," using our standard **Fanout-on-Write (Push)** model would mean the Fanout Workers have to execute **50 million separate writes** to 50 million different Redis keys.

This causes two massive production system failures:

1.  **Cache Hotspots & Lag:** Your Kafka queue will back up instantly, creating a huge delay. Regular users will experience minutes of lag before seeing updates from their actual friends because the workers are choked handling the celebrity's 50 million tasks.
```
A **cache hotspot** happens when one specific piece of data becomes so popular that millions of users try to read it at the exact same millisecond, overloading the single server holding that data.
Even though caching is meant to speed things up, concentrating massive traffic onto one spot causes that specific cache server to crash or slow down. 

Quick Example

-   **The Celebrity Tweet:** Millions of people refresh Elon Musk’s profile page at the exact same second. The cache server holding his profile gets crushed, while servers holding regular users' profiles stay completely idle. 
How to Fix It

-   **Data Replication:** Copy the popular data across multiple cache servers.
-   **Cache Salting:** Add a random number to the cache key (e.g., `user_123_1`, `user_123_2`) to spread the data across different servers.
-   **Local In-Memory Cache:** Store the ultra-popular data directly on the web application servers so they do not even need to network-call the main cache layer.
```
 ```
**choked** means a single component is overloaded and slowing down the entire system.

It acts like a **bottleneck**—no matter how fast the rest of your system is, the overall speed is limited by this one restricted area.

Quick Examples

-   **Database overload:** A fast web server waiting on a slow database.
-   **Network limit:** Too much data trying to pass through a narrow bandwidth pipe.

How to Fix It

-   **Scale up:** Add more resources (like CPU or memory).
-   **Load balance:** Spread the traffic across multiple servers.
-   **Cache data:** Store frequent requests so the system does less work.
```
2.  **Storage Waste:** If 40 million of those followers haven't logged into the app in weeks, you waste expensive Redis memory pre-computing a feed they might never see.
    
### The Architecture Solution: The Hybrid Model

To handle this at scale, a senior architect implements a **Hybrid Architecture** (combining both **Push** and **Pull** models dynamically based on user status).

Here is how the system handles the data split:

#### 1. The Write Flow (Publishing)

-   **Standard Users:** When a regular user posts, we use **Fanout-on-Write (Push)**. Their post ID is actively pushed to their friends' Redis cache keys via Kafka and Fanout Workers.
    
-   **Celebrities:** When a user designated as a "Celebrity" posts, the system cuts off the fanout pipeline entirely. The post ID is saved **only** to the celebrity's own timeline cache/database. It is **never** pushed to the followers' caches.
    

#### 2. The Read Flow (Retrieval / Pull)

When a regular user opens their app, their **News Feed Service** performs an on-the-fly **aggregation (Pull)**:
```
──> 1. Read User's Cache ──> [Returns regular friends' posts] │ [News Feed Service] │
└──> 2. Fetch Celebrities Followed ──> 
			[Pull latest celebrity posts] 
					│ 
					▼ 
			[Merge & Sort by Timestamp] 
					│
					▼
			[Deliver JSON to Client]
```

-   **Fetch the Regular Feed:** The service reads the user's personal Redis Sorted Set (`user:feed:<user_id>`) to instantly grab the pre-computed post IDs of their regular friends ($O(1)$ latency).
- **Fetch the Celebrity Feed:** The service checks who this user follows. If they follow any celebrities, the service goes to those specific celebrities' timeline caches and pulls their most recent posts
**Merge and Sort:** The service combines the regular friend posts with the celebrity posts, sorts them by timestamp in descending order, and sends the final hydrated feed to the user's screen.

### How to Implement This in Java/Spring Boot

To execute this, your system needs to distinguish between standard users and celebrities. You can define a threshold (e.g., any user with $>25,000$ followers is marked as a celebrity).

#### The Fanout Service Guard (Write Path)
```
@Service
public class HybridFanoutService {

    @Autowired private GraphDBService graphDBService;
    @Autowired private KafkaTemplate<String, FanoutTask> kafkaTemplate;

    public void processPost(PostCreatedEvent event) {
        // Check follower count constraint
        long followerCount = graphDBService.getFollowerCount(event.authorId());

        if (followerCount > 25000) {
            // CELEBRITY: Isolate the write. Do not generate 50 million Kafka tasks!
            saveToCelebrityTimelineCache(event);
        } else {
            // REGULAR USER: Safe to push to friends
            List<String> friendIds = graphDBService.getFriendIds(event.authorId());
            kafkaTemplate.send("fanout-tasks", new FanoutTask(event.postId(), friendIds));
        }
    }
}
```

### Trade-offs of the Hybrid Model

While the Hybrid model saves your database and Redis cache from melting down during celebrity posts, it introduces a new trade-off: **increased read latency**.

Instead of doing a single $O(1)$ lookup from one Redis key, the News Feed Service now has to execute multiple lookups in parallel (the user's feed + the timeline of every celebrity they follow) and sort them in application memory. For a senior engineer, this is a highly acceptable trade-off because read latency can be tightly managed, whereas a database crash from a 50-million-write stampede cannot.