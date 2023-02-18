---
layout: post
title: 'Database Concepts: Write Skew'
date: '2022-12-19 17:57:37 +0530'
---

## What is Write Skew

Phantom write is the generalization of a lost update as in lost update different transaction modified the same object but in write skew, they modified different objects.

Write skew can also occur in a database update operation when multiple transactions attempt to update the same set of records concurrently, resulting in conflicts and delays in processing. This can happen when two or more transactions try to update the same record at the same time, causing one transaction to wait until the other transaction completes before it can proceed.

One way to address write skew in a database update operation is to use locking mechanisms to prevent concurrent updates to the same set of records. For example, a database can use row-level locking, where each transaction locks the specific rows it needs to update, preventing other transactions from modifying those same rows until the lock is released. However, this can also cause contention and reduce the concurrency of the system.

Another approach to addressing write skew in database updates is to use optimistic concurrency control, where each transaction checks if the data it needs to update has been modified by another transaction since it was last read. If the data has been modified, the transaction is aborted and has to be retried with the latest version of the data. This approach can increase concurrency but may result in higher abort rates.

It is also essential to design the schema and indexing strategy of the database in a way that minimizes write skew. For example, if the database schema is designed such that multiple transactions frequently need to update the same set of records, it may be beneficial to create additional indexes or to partition the data in a way that minimizes conflicts.

Overall, addressing write skew in a database update operation requires a careful balancing of locking, concurrency, and data partitioning strategies to ensure optimal performance and data consistency.