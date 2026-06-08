# Comprehensive Guide to Caching in System Design

## Overview of Caching

Caching is a temporary storage strategy that keeps recently used data in a faster layer—typically memory (RAM)—so it can be retrieved more quickly than from a primary database.

- **Speed Comparison:** Accessing data from RAM takes approximately 100 nanoseconds. This is 10,000 times faster than the 1 millisecond required to access data from a disk-based SSD.
- **Trade-offs:** While caching adds architectural complexity and uses more storage, it is essential for scaling systems that serve thousands of requests per second.

---

## Where Caching Lives

There are four primary layers where caching can be implemented:

### 1. External Caching

- **Technology:** Uses dedicated services like Redis or Memcached on separate servers.
- **Interview Context:** This is the most common approach discussed in interviews because it provides a global, shared view for all application servers.

### 2. In-process Caching

- **Mechanism:** Storing data directly within the application's memory.
- **Trade-offs:** This is the fastest method as it avoids network hops, but it can lead to data inconsistency across different servers.

### 3. Content Delivery Networks (CDNs)

- **Mechanism:** A geographically distributed network of servers.
- **Use Case:** Designed to cache content—typically static media like images and videos—closer to the user to reduce network latency.

### 4. Client-side Caching

- **Mechanism:** Data stored directly on the user's device (browser or mobile app).
- **Use Case:** Ideal for offline functionality, though it gives developers less control over data freshness.

---

## Caching Architectures

Architectures define how the application, cache, and database interact:

- **Cache-Aside:** The application checks the cache first. On a cache miss, it fetches from the database, updates the cache, and returns the value. This is the recommended default for interviews because it keeps the cache lean.
- **Write-Through:** Data is written to the cache and database synchronously. It ensures data is always fresh but results in slower writes and can "pollute" the cache with data that may never be read.
- **Write-Behind (Write-Back):** Data is written to the cache first, and the database is updated asynchronously in batches. This allows for high write throughput but carries a risk of data loss if the cache crashes before the database updates.
- **Read-Through:** The cache acts as a proxy, handling the database lookup itself on a cache miss. This is primarily how CDNs function.

---

## Eviction Policies

Since memory is limited, systems must decide which data to remove when the cache is full:

- **Least Recently Used (LRU):** Evicts items that haven't been accessed for the longest time. It is the most common default.
- **Least Frequently Used (LFU):** Evicts items based on how rarely they are accessed, regardless of how recently they were used.
- **First In First Out(FIFO):** Evicts items based on insertion order.
- **Time-to-Live (TTL):** Automatically removes items after a set period, which is crucial for maintaining data freshness.

---

## Common Challenges

### Cache Stampede (Thundering Herd)

- **Problem:** Occurs when a popular key expires and a flood of requests hits the database simultaneously.
- **Solutions:** Request coalescing (limiting database rebuilds to a single request) or cache warming (proactively refreshing keys before they expire).

### Cache Consistency

- **Problem:** When the cache and database hold different values.
- **Solutions:** Invalidating the cache on write, using short TTLs, or accepting eventual consistency for non-critical data.

### Hotkeys

- **Problem:** When a single cache entry (e.g., a celebrity profile) receives overwhelming traffic, bottlenecking a specific shard.
- **Solutions:** Replicating hotkeys across multiple shards or using a local fallback cache within the application.

---

## Interview Strategy

In a system design interview, you should only introduce caching if you can justify it based on:

1. **Read-heavy workloads** that overwhelm the database.
2. **Expensive queries** that involve complex joins or computations.
3. **Strict latency requirements** specified in non-functional requirements.

> 💡 **Pro-Tip:** When discussing your design, clearly identify the bottleneck, specify the cache keys and values, choose an architecture, select an eviction policy, and address potential downsides.

# Key Takeaways

- Caching improves performance by storing data in faster memory (RAM)
- External cache (Redis) is the most common production approach
- Cache-Aside is the default pattern in interviews
- Write-Through = consistency first, performance later
- Write-Behind = performance first, consistency later
- Major problems:
  - Cache stampede
  - Cache inconsistency
  - Hot keys
- Eviction policies:
  - LRU (most common)
  - LFU (frequency-based)
  - FIFO (simple)
  - TTL (time-based)
- Always justify caching with a real bottleneck
- Always discuss trade-offs and failure scenarios
