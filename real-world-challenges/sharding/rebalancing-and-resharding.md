# Challenge: Rebalancing and Resharding

Adding or removing shards in a standard hash-based system typically requires a massive reshuffling of data because the mathematical mapping changes for nearly every record.

---

## Solution — Consistent Hashing

This industry-standard technique places keys and shards on a virtual ring.

When the fleet scales, only a small fraction of the data needs to be moved, making scaling smooth.
