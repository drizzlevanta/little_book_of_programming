# In-Memory Cache
## Use cases
1. Speed up read-heavy database query
2. Caching expensive computations
3. Reducing calls to slow or rate-limited external APIs
4. Rate limiting & abuse prevention
  
## Redis cache
Redis is an in-memory data store (key–value) that's commonly used as a cache. A Redis cache typically sits between your app and your database/external services:
- App receives a request for data
- Check Redis first
  - If found (cache hit): return quickly
  - If missing (cache miss): fetch from DB/API, then store in Redis with an expiration (TTL)
This reduces database load and improves latency.

## Redis vs DashMap
Redis becomes the better choice when you're running multiple server instances. If you have two or more Axum processes behind a load balancer, an in-memory DashMap on each instance means their caches diverge immediately. Redis gives you a single shared cache that all instances read from, so every user sees the same state regardless of which server handles their request.

## Dashmap vs moka
DashMap is a concurrent map. It gives you a thread-safe key-value store where multiple threads can read and write without a global lock (it uses internal sharding). You put things in, you get things out. Items stay there until you explicitly remove them.

Moka is a caching layer built on top of similar concurrency primitives, but with all the cache-specific behavior baked in: **automatic eviction, TTL(time to live), TTI(time to idle), size bounds, and async loading**.

## The hybrid approach
A common production pattern is to layer both:
Request -> In-memory (moka) -> Redis -> SeaORM/Postgres

- L1 (in-memory): Sub-microsecond. **Holds the hottest data** — active rooms, online users, last ~100 messages per active room.
- L2 (Redis): Shared across instances. Holds broader state, handles pub/sub for cross-instance fanout.
- L3 (Database via ORM): Source of truth. Cold reads and persistence.

## The thundering herd problem
Sometimes also called a **cache stampede**. The thundering herd problem in cache happens when many requests all miss the cache at the same time, then they all rush to rebuild the same data from the database or backend.
- A hot cache key expires
- 1000 users request that same data right after
- Instead of 1 backend query, you now get 1000 backend queries
- That sudden spike can overload your DB or upstream service
