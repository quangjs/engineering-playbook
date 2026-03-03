## 1. Relational (SQL) vs. Non-Relational (NoSQL)

This is the most common starting point. You must know when to pick which tool.

- **SQL (PostgreSQL, MySQL):** Best for structured data where **ACID compliance** (Atomicity, Consistency, Isolation, Durability) is non-negotiable (e.g., banking). Uses fixed schemas and joins.
- **NoSQL (MongoDB, Cassandra, DynamoDB):** Best for unstructured data, high-velocity writes, or horizontal scaling.
- **Document:** JSON-like (Mongo).
- **Key-Value:** Fast caching (Redis).
- **Wide-Column:** Large scale analytical data (Cassandra).

---

## 2. The Data Modeling Process

Interviewers often give you a prompt like _"Design the database for Twitter."_ You should follow these steps:

### A. Conceptual Modeling

Identify your **Entities** and their **Relationships**.

- **One-to-One (1:1):** User has one Profile.
- **One-to-Many (1:N):** User has many Posts.
- **Many-to-Many (M:N):** Students and Courses (requires a **Join Table** or **Link Table**).

### B. Normalization

The goal is to reduce data redundancy.

- **1NF:** Atomic values (no lists in a cell).
- **2NF:** Move subsets of data that apply to multiple rows to separate tables.
- **3NF:** Eliminate fields that don't depend solely on the Primary Key.

> **Note:** In high-scale systems, we sometimes **denormalize** (purposefully repeat data) to avoid expensive joins and speed up reads.

---

## 3. Indexing: The Performance Secret

If you don't mention indexing, you likely won't pass a senior backend interview.

- **How it works:** An index is a data structure (usually a **B-Tree** or **Hash Map**) that allows the DB to find rows without scanning the whole table.
- **Trade-off:** Faster reads, but slower writes (because the index must be updated every time data changes).
- **Composite Indexes:** Indexing multiple columns together (e.g., `last_name` + `first_name`). Order matters!

---

## 4. Transactions & Concurrency

How do you handle two people buying the last concert ticket at the same time?

- **ACID Properties:** Be ready to define each letter.
- **Isolation Levels:** Understand how "Dirty Reads" or "Phantom Reads" occur.
- **Optimistic vs. Pessimistic Locking:** \* **Pessimistic:** Lock the record so no one else can touch it (prevents conflict).
- **Optimistic:** Check if the version has changed before committing (handles conflict).

---

## 5. Scaling Strategies

When one database server isn't enough, you talk about:

- **Vertical Scaling:** Getting a bigger "box" (more RAM/CPU).
- **Read Replicas:** Copying data to other servers to handle massive read traffic.
- **Sharding (Horizontal Partitioning):** Splitting one big table into multiple smaller tables across different servers based on a "Shard Key" (e.g., UserID).

### A. Joining Data After Scaling

Once data is sharded or split across microservices, traditional SQL `JOIN`s become impossible or extremely expensive.

- **Application-Side Joins:** Fetch data from Service A, then from Service B, and join them in the app code. (Common in Microservices).
- **Denormalization:** Store redundant data (e.g., store `user_name` inside the `posts` table) to avoid joins entirely. _Trade-off: Complex updates (need to update multiple places)._
- **Data Materialization:** Use a tool like **Elasticsearch** or a **Read Model (CQRS)** that aggregates data from multiple sources into a single searchable document.
- **Global Tables:** For small, rarely changed datasets (e.g., `countries`), replicate the table to _every_ shard so local joins remain possible.

### B. Pagination After Scaling

- **Offset-based (`LIMIT 10 OFFSET 1000`):**
  - **The Problem:** In a sharded system, to get the 100th page, every shard must return its top 1000 records to the coordinator. If you have 50 shards, the coordinator must merge 50,000 rows just to show 10.
- **Cursor-based / Keyset Pagination (`WHERE id > last_seen_id LIMIT 10`):**
  - **The Solution:** Instead of "skipping" rows, you ask for data "after this specific point."
  - **Pros:** Consistent performance (O(log N) with index), works perfectly with sharding.
  - **Best Practice:** Use a deterministic sort (e.g., `ORDER BY created_at, id`) to avoid "skipping" items with the same timestamp.

---

## 6. Common Interview "Red Flags"

- **Using a string for a Primary Key** without a reason (usually use `UUID` or `BIGINT`).
- **Ignoring Soft Deletes:** Deleting data permanently (`DELETE`) vs. using a `deleted_at` timestamp.
- **Lack of Constraints:** Forgetting `NOT NULL`, `UNIQUE`, or `FOREIGN KEY` constraints.
