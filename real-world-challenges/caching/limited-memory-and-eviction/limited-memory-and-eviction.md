### Limited Memory and Eviction

Because memory is much more expensive and limited than disk storage, you generally cannot fit an entire dataset into a cache
. This creates the challenge of deciding which data to keep and which to remove as the cache fills up

### Solution

Systems must implement eviction policies like LRU (Least Recently Used), LFU (Least Frequently Used), or FIFO (First In First Out)
