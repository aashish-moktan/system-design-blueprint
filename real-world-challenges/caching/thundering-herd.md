### Cache Stampede (Thundering Herd)

This challenge occurs when a highly popular cache entry expires (usually via its Time-to-Live or TTL), causing a massive flood of simultaneous requests to hit the database at once to rebuild that entry
. This can turn a single query into millions of database hits in a single second, potentially leading to database failure

### Solution

The source mentions request coalescing (ensuring only the first request rebuilds the cache while others wait) and cache warming (proactively refreshing the key before it expires)
