---
layout: post
title: 'Database Concepts: Lost Update Anomaly'
date: '2022-12-19 18:53:55 +0530'
---

A **lost update** occurs when two different transactions are trying to update the same column on the same row within a database at the same time.

You can solve the lost update problem using following approaches:

1. Repeatable Read Isolation
2. Optimistic Locking
3. Select for Update

## Further Reading

https://vladmihalcea.com/a-beginners-guide-to-database-locking-and-the-lost-update-phenomena/