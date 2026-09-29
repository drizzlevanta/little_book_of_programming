# Database
## Sharding
Sharding is a database scaling technique that splits a large dataset horizontally across multiple database instances (called shards), each holding a subset of the data.

How it works:
- A shard key (e.g., user ID, region) determines which shard stores a given record.
- Each shard is an independent database that handles reads/writes for its partition of data.
- A routing layer directs queries to the correct shard based on the key.
For example: A users table with 10M rows sharded by user_id % 4 across 4 databases — shard 0 gets IDs 0,4,8…, shard 1 gets 1,5,9…, etc.

Benefits:
- Horizontal scalability — add more shards as data grows
- Better performance — each shard handles less data/traffic
- Fault isolation — one shard failing doesn't take down the whole system
Trade-offs:
- Cross-shard queries are expensive (joins across shards)
- Rebalancing is complex when adding/removing shards
- Operational overhead — more databases to manage
- Hotspots can occur if the shard key distributes data unevenly
- 
Sharding vs. Partitioning: Partitioning splits data within a single database; sharding splits it across multiple databases/servers. Sharding is essentially distributed partitioning.