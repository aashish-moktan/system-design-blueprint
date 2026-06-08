### Hotkeys

A "hotkey" is a specific cache entry that receives significantly more traffic than others, such as a celebrity's profile
. Even if the overall cache is healthy, the overwhelming demand for that one key can bottleneck a single cache node or shard

### Solution

This can be mitigated by replicating hotkeys across all shards in a cluster or using a local fallback cache (in-process caching) on the application server to avoid hitting the external cache repeatedly for that specific value
