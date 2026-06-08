# Challenge: Cross-Shard Operations

When data is split across machines, any query that requires information from multiple shards (e.g., "top 10 posts across the whole platform") becomes an expensive "scatter-gather" operation that increases latency.

---

## Solution — Query Alignment

The primary defense is selecting a shard key that ensures your most common queries only need to hit a single shard.

---

## Solution — Caching

Results of expensive global queries can be stored in a cache (like Redis).

While this may lead to some data staleness, it significantly reduces the need for frequent cross-shard lookups.

---

## Solution — Denormalization

You can repeat data across shards so that related information lives together.

This makes reads faster by keeping them local to one shard, though it makes writes more complex.

---

## Solution — Precomputation

Use background jobs to pre-calculate and aggregate results for global queries so the data is ready before it is requested.
