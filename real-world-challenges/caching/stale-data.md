### Cache Consistency (Stale Data)

Consistency issues arise because the cache and the database can return different values for the same piece of data
. This typically happens when a system reads from the cache but writes new data to the database, leaving the "old" value in the cache until it is evicted or expires

### Solution

Common strategies include invalidating on write (deleting the cache key immediately when the database is updated), using short TTLs, or simply accepting eventual consistency if the data is not mission-critical, like a social media feed
