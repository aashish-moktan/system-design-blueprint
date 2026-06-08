# Challenge: Hotspots or Load Imbalance (The "Celebrity Problem")

Even with a high-cardinality shard key, some shards may experience significantly more traffic than others if they host a "celebrity" entity (e.g., Lionel Messi) whose data attracts massive global engagement.

---

## Solution — Compound Shard Keys

Instead of sharding by a single ID, you can append a suffix or piece of data (like a random number or time element) to the input of the hash function.

This further distributes that specific entity's data across multiple shards.

---

## Solution — Dedicated Celebrity Shards

You can detect high-traffic outliers and move them to specialized, larger hardware.

This often involves a directory-based lookup specifically for those users to route them to their dedicated shard, while others follow standard hashing.
