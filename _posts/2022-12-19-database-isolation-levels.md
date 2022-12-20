---
layout: post
title: 'Database Concepts: Isolation'
date: '2022-12-19 14:43:23 +0530'
---
### Read Phenomena in Concurrent Transactions

When a lot of transactions are trying to update the same resource in parallel, then as a result of the updates performed against DB, some anomalies or phenomena can occur as below.

#### Loss of Data

If the two transactions want to change the same columns, the second transaction will overwrite the first one, therefore losing the first transaction update. So **an update is lost when a user overrides the current database state without realizing that someone else changed it between the moment of data loading and the moment the update** occurs.

#### Dirty Read

A Dirty read is a situation when **a transaction reads data that has not yet been committed**. For example, Let’s say transaction 1 updates a row and leaves it uncommitted, meanwhile, Transaction 2 reads the updated row. If transaction 1 rolls back the change, transaction 2 will have read data that is considered never to have existed. 

#### Non Repeatable Read

Non Repeatable read occurs when a **transaction reads the same row twice and gets a different value each time**. For example, suppose transaction T1 reads data. Due to concurrency, another transaction T2 updates the same data and commit, Now if transaction T1 rereads the same data, it will retrieve a different value. 

#### Phantom Read

Phantom Read occurs when **two same queries are executed, but the rows retrieved by the two, are different**. For example, suppose transaction T1 retrieves a set of rows that satisfy some search criteria. Now, Transaction T2 generates some new rows that match the search criteria for transaction T1. If transaction T1 re-executes the statement that reads the rows, it gets a different set of rows this time 

#### Serialization Anomaly

Serialization anomaly means that the result of successfully committing a group of transactions is inconsistent with all possible orderings of running those transactions one at a time.

### DB Isolation Levels

In order to avoid the above anomalies, the DB supports following isolation levels to guarantee to avoid the transaction anomalies.

#### Read Uncommitted

Read Uncommitted is the lowest isolation level. In this level, **one transaction may read not yet committed changes made by other transaction**, thereby allowing dirty reads. In this level, transactions are not isolated from each other. 

#### Read Committed

This isolation level guarantees that **any data read is committed at the moment it is read**. Thus it does not allows dirty read. The **transaction holds a read or write lock on the current row**, and thus prevent other transactions from reading, updating or deleting it. 

#### Repeatable Read

This is the most restrictive isolation level. The **transaction holds read locks on all rows it references and writes locks on all rows it inserts, updates, or deletes**. Since other transaction cannot read, update or delete these rows, consequently it avoids non-repeatable read. 

#### Snapshot

This level takes a snapshot of current data. Every **transaction works on its own copy of data**. When User A tries to update or insert or read anything, we ask him to re-verify the table row once again from the starting time of its execution, so that he can work on fresh data. with this level. We are not giving full faith to User A that he is going to work on fresh data but giving high-level changes of data integrity. 

#### Serializable

This is the Highest isolation level. A serializable execution is guaranteed to be serializable. Serializable **execution is defined to be an execution of operations in which concurrently executing transactions appears to be serially executing**.

### Consistency Guarantees 

| Isolation Level  | Dirty Read             | Nonrepeatable Read | Phantom Read           | Lost Update Anomaly | Serialization Anomaly |
| ---------------- | ---------------------- | ------------------ | ---------------------- | ------------------- | --------------------- |
| Read uncommitted | Allowed, but not in PG | Possible           | Possible               | Possible            | Possible              |
| Read committed   | Not possible           | Possible           | Possible               | Possible            | Possible              |
| Repeatable read  | Not possible           | Not possible       | Allowed, but not in PG | Not possible        | Possible              |
| Serializable     | Not possible           | Not possible       | Not possible           | Not possible        | Not possible          |

### Further Reading

1. https://www.postgresql.org/docs/13/transaction-iso.html
2. https://levelup.gitconnected.com/understanding-isolation-levels-in-a-database-transaction-af78aea3f44
3. https://mkdev.me/posts/transaction-isolation-levels-with-postgresql-as-an-example