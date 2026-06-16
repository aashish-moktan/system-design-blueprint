# Comprehensive Guide to Data Modeling in System Design

## Overview of Data Modeling

Data modeling defines how an application's data is structured, stored,
and related. It determines:

- What entities/tables exist
- How entities are connected
- What fields each entity contains
- How data can be accessed efficiently

A good data model is based on:

- **Data Volume:** How much data the system stores and processes.
- **Access Patterns:** How data is read and written.
- **Consistency Requirements:** How accurate and synchronized the data
  must be.

---

# Data Modeling Process

## 1. Core Entities (Early Phase)

Identify the main nouns of the system. These become the primary tables.

Example:

    User
    Post
    Comment
    Like
    Follow

Focus on:

- Main entities
- Relationships between entities

---

## 2. High-Level Design Phase

Refine the schema based on APIs and requirements.

Define:

- Columns
- Primary keys
- Foreign keys
- Relationships
- Indexes
- Constraints

Example:

    Users
    -----
    id (PK)
    name

    Posts
    -----
    id (PK)
    user_id (FK)
    content
    created_at

Add indexes based on query patterns.

---

# Database Selection Strategy

## Relational Database (Recommended Default)

Examples:

- PostgreSQL
- MySQL

Best for:

- Structured data
- Complex relationships
- Strong consistency

Provides:

- Foreign keys
- Transactions
- Data integrity

---

## Document Database

Example:

- MongoDB

Best for:

- Flexible schemas
- Frequently changing data structures

Trade-offs:

- Less relational consistency
- More duplication

---

## Key-Value Store

Example:

- Redis

Used for:

- Caching
- Sessions
- Rate limiting
- Fast lookups

Usually works alongside a primary database.

---

## Wide Column Database

Examples:

- Cassandra
- Bigtable

Used for:

- Massive write workloads
- Event data
- Time-series data

---

## Graph Database

Example:

- Neo4j

Used for:

- Relationship-heavy systems
- Recommendation systems
- Fraud detection

---

# Core Data Modeling Challenges

## Data Inconsistency

Problem:

Invalid relationships can create orphaned data.

Example:

    Post → User does not exist

Solution:

### Foreign Keys

    Posts.user_id → Users.id

### Normalization

Store each piece of information in one location.

Benefits:

- Avoid duplicate data
- Prevent update anomalies
- Maintain consistency

---

## Slow Query Performance

Problem:

Full table scans become expensive.

Without index:

    O(N)

Solution:

Use indexes.

Example:

    INDEX(user_id)

Indexes improve filtering and sorting performance.

---

## Expensive Joins at Scale

Problem:

Large joins become slow as data grows.

Example:

    Users
     JOIN
    Posts
     JOIN
    Comments

Solution:

Denormalization.

Duplicate frequently accessed data to reduce joins.

Commonly used in:

- Cache layers
- Read models
- Precomputed views

---

## Massive Write Volume

Problem:

A single database may not handle very high write traffic.

Solutions:

### Wide Column Database

Used for:

- Logs
- Events
- Metrics

### Write Buffering

Architecture:

    Application
         |
     Message Queue
         |
     Database

Benefits:

- Handles traffic spikes
- Enables batch writes

---

## Data Volume Exceeds One Node

Problem:

A single database machine cannot handle all data.

Solution:

### Sharding

Split data across multiple machines.

Example:

    Shard 1 → Users 1-1M
    Shard 2 → Users 1M-2M
    Shard 3 → Users 2M-3M

Shard key should:

- Match query patterns
- Distribute load evenly
- Keep related data together

---

## Invalid Data Input

Problem:

Incorrect data can enter the system.

Solution:

Database constraints.

Examples:

    NOT NULL

Required fields.

    UNIQUE

Prevent duplicates.

    CHECK

Validate conditions.

---

# Interview Workflow

## Step 1: Choose Database

Default:

    PostgreSQL

unless requirements suggest otherwise.

---

## Step 2: Identify Entities

Example:

    User
    Post
    Comment

---

## Step 3: Define Columns

Add only required fields.

---

## Step 4: Define Keys

- Primary keys → Unique identification
- Foreign keys → Relationships

---

## Step 5: Determine Indexes

Work backward from APIs.

Add indexes for:

- Filtering
- Sorting
- Frequent queries

---

## Step 6: Evaluate Denormalization

Only introduce when:

- Joins become expensive
- Read performance is critical

---

## Step 7: Evaluate Sharding

Only introduce when:

- Data exceeds one machine
- Database capacity becomes a bottleneck

---

# Key Takeaways

- Data modeling defines entities, relationships, and storage
  structure.
- Identify core entities early, then refine schema during API design.
- Database selection depends on scale, structure, and consistency
  needs.
- PostgreSQL is the recommended default choice for most interviews.
- Use normalization for consistency.
- Use indexes for query performance.
- Use denormalization for faster reads.
- Use queues or wide-column databases for massive writes.
- Use sharding when data exceeds one machine.
- Always justify decisions using:
  - Data volume
  - Access patterns
  - Consistency requirements
