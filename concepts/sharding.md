# Sharding in System Design

## Core Definition and Motivation

Sharding is the process of splitting data across multiple standalone machines so that no single database is responsible for the entire dataset.

Each shard functions as an independent database with its own CPU, memory, storage, and connection pool, storing only a subset of the total data. Together, these shards constitute the complete dataset, allowing for horizontal scaling of storage and throughput.

The motivation for sharding arises when an application hits a "ceiling" that vertical scaling (upgrading to larger hardware) cannot solve. These limits include:

- Storage Constraints: Approaching the physical capacity of a single instance (e.g., 70–140 TB)
- Throughput Limits: Exceeding the write/read capacity of a single machine (e.g., 10,000–50,000 writes per second)
- Operational Strain: Issues like backups taking an excessive amount of time or queries slowing down due to resource saturation

---

## The Sharding Strategy

A sharding design requires two primary decisions: what to shard by (the key) and how to distribute those values.

### 1. The Shard Key

The shard key is the specific field used to group data.

A "good" shard key must possess three properties:

- High Cardinality: A large number of unique values to allow for broad distribution (e.g., user_ID rather than a boolean like is_premium)
- Even Distribution: Values should naturally spread out to prevent some shards from being significantly larger than others
- Query Alignment: The key should match common query patterns so that most requests can be satisfied by a single shard (e.g., sharding by user_ID when the most common action is viewing a user's profile)

---

### 2. Distribution Strategies

- Range-Based Sharding: Splits data into numeric or alphabetical ranges. While intuitive, it often leads to uneven distribution or hotspots (e.g., new users all hitting the highest range shard).

- Hash-Based Sharding: The industry default. It uses a hash function to scramble the key and a modulo operation to assign shards, ensuring even distribution.

- Consistent Hashing: A specialized hash-based approach that places keys and shards on a virtual ring. This minimizes data reshuffling (rebalancing) when shards are added or removed.

- Directory-Based Sharding: Uses a lookup table to map keys to specific shards. It offers maximum flexibility (e.g., moving a specific user to a new shard) but adds latency via an extra hop and creates a single point of failure.

---

## Major Challenges and Solutions

Sharding introduces significant operational complexity and specific technical hurdles:

- Hotspots (The Celebrity Problem): A high-traffic user (e.g., Messi) can overwhelm a single shard.
  - Solutions: Use a compound shard key (adding a suffix to distribute data) or move celebrities to dedicated shards using a directory-based lookup.

- Cross-Shard Operations: Queries that require data from multiple shards (e.g., "top 10 posts across the platform") are expensive "scatter-gather" operations.
  - Solutions: Optimize shard key for query alignment, cache global queries, or denormalize data.

- Data Consistency: Atomic transactions are difficult when data spans shards.
  - Solutions: Avoid cross-shard transactions where possible; use the Saga pattern or Two-Phase Commit (2PC), though 2PC is often slow and fragile.

---

## Execution in System Design Interviews

In interviews, sharding should not be a reflexive answer. Use a structured approach:

- Justify with Math: Show that a single database cannot handle storage or throughput limits.
- Propose a Shard Key: Based on the most frequent access patterns.
- Choose a Strategy: Default to hash-based sharding with consistent hashing.
- Address Trade-offs: Handle cross-shard queries, hotspots, and future scaling.

---

## Key Takeaways

- Sharding is horizontal scaling by splitting data across multiple databases.
- Each shard is an independent database handling a subset of data.
- Sharding is needed when vertical scaling hits storage, throughput, or operational limits.
- A good shard key has high cardinality, even distribution, and query alignment.
- Hash-based and consistent hashing are the most commonly used strategies.
- Major challenges include hotspots, cross-shard queries, and data consistency.
- Always justify sharding with system limits and discuss trade-offs in interviews.
