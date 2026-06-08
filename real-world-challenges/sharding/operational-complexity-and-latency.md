# Challenge: Operational Complexity and Latency

More flexible strategies like directory-based sharding add complexity and risks to the system architecture.

---

## Solution — Default to Hash-Based Sharding

To avoid the extra latency (the "extra hop") and the single point of failure inherent in maintaining a lookup directory, it is recommended to default to hash-based sharding with consistent hashing in system design.
