# Challenge: Maintaining Data Consistency

Traditional atomic database transactions are easy on a single instance but become "messy" when an operation spans multiple shards (e.g., a bank transfer between two users on different machines).

---

## Solution — Avoidance

The "golden rule" is to design the system to avoid cross-shard transactions whenever possible by keeping related data on the same machine.

---

## Solution — Two-Phase Commit (2PC)

This uses a central coordinator to ensure all shards are ready to commit.

However, it is slow and fragile in production systems.

---

## Solution — Saga Pattern

This breaks a transaction into a sequence of smaller steps, where each step has a compensating action (a manual undo) that runs if a subsequent step fails, ensuring the system eventually returns to a consistent state.
