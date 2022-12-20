---
layout: post
title: 'Database Concepts: Multiversion Concurrency Control'
date: '2022-12-19 18:00:49 +0530'
---

### What is MVCC

MVCC, or Multiversion Concurrency Control, is a method used by database management systems to ensure that multiple transactions can be processed concurrently without conflicting with each other. It works by allowing multiple versions of a data record to exist simultaneously, with each version corresponding to a different transaction.

When a transaction wants to update a record, it creates a new version of the record and marks the old version as no longer being the current version. The new version is then used by the transaction, while other transactions continue to use the old version. When the transaction is completed, the new version becomes the current version and is used by all transactions.

MVCC allows transactions to be processed concurrently without the need for locks, which can improve performance and reduce the risk of deadlocks. It is often used in database systems that need to support high levels of concurrency, such as online transaction processing (OLTP) systems.

### Further Reading

https://www.postgresql.org/docs/7.1/mvcc.html

https://vladmihalcea.com/how-does-mvcc-multi-version-concurrency-control-work/

https://blog.justinhaygood.com/2020/02/04/hibernatejpa-multi-version-concurrency-control-with-postgres/

https://yehohanan7.medium.com/mvcc-9d6f67e6d3de