[← Back to README](../../README.md)

### Questions
- [1) In Figures 3-1 and 3-2, are the Fanout Service and News Feed Service the same or different?](3-1_3-2_isSameOrdifferent.md)
#### Analyzing Feed publishing  Figure 3-3: The Endpoint & The Cache
- [2 (i) Is the News Feed Cache a Redis cache?](isNewsFeedCacheaRediscache.md)
- [2 (ii) Explain Fanout Workers like I'm 5 (ELI5)](explainFanoutWorksers.md)
- [2 (iii) Why route through a Message Queue and Workers instead of writing directly from the Fanout Service to the Redis Cache?](whyFanoutNotDirectlyWriteToCache.md)
- [2 (iv) Why a t3.micro Struggles with Spring Boot](whyT3microStrugglesWithSpringBoot.md)
- [2 (v) The Full EC2 Sizing Matrix (For Spring Boot Apps)](fullEc3Metrix.md)
- [2 (vi) v1/me/feed? content=Hello&authToken={authToken} How the same request can call multiple services post service, fanout service, notification service](howPostRequestCallMultipleServices.md)
- [2 (vii)In Master-Replica Architecture As per CAP therom which one we need to select CP or AP ?](checkCP_Or_AP_ForNewsFeedCache.md)
#### Analyzing Newsfeed retrival Figure 3-4: The Endpoint & The Cache
- [3 (i) Is the Logged in User fetches his posts based on user Id in News Feed Cache?](isLoggedInUserGetPostsFromNewsFeedCache.md)
- [3 (ii) What happens if one of the people the user follows is a massive celebrity with 50 million followers? Do we still pre-compute their posts into our user cache?](isLoggedInUserFollowsCelebrityDoWeStillPreComputeTheirPostsIntoOurNewsFeedCache.md)
- [3 (iii) What happens News Feed Cache is Down Completely?](ifNewsFeedCacheIsDown.md)
- [3 (iv) Handling Infinite Historical Data (Reading Years of Old Data)?](handlingHistoricalData.md)

### Completed Topics
- End-to-end publishing flow
- MQ isolation, EC2 scaling tiers 
- Feed Retrieval mechanics 
- Celebrity Hybrid architecture.
- Cache Fault Tolerance
- Deep Scroll Pagination.
- CAP trade-offs

