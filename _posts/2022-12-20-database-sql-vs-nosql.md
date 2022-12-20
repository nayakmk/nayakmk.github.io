---
layout: post
title: 'Database: SQL vs NoSQL'
date: '2022-12-20 23:29:02 +0530'
categories: [DISTRIBUTED, DATABASE]
tags: [database, base, acid, sql, nosql]
---

### SQL 

- SQL Databases follows ACID consistency model (Strong consistency or write consistency).
- Relational databases are designed for transactional and strongly consistent online transaction processing (OLTP) applications and are good for online analytical processing (OLAP).

### NoSQL

* NoSQL databases are **designed for a number of data access patterns that include low-latency applications**. 
* NoSQL search databases are designed for analytics over semi-structured data.  
* These types of database include key-value, document, columnar, graph.
* Most NoSQL databases provides **BASE** consistency model (Eventual consistency). Chooses availability over consistency.

### ACID 

ACID is a set of properties that describe the behavior of a database transaction. The acronym ACID stands for **Atomicity, Consistency, Isolation, and Durability**. These properties are used to ensure that database transactions are reliable and maintain the integrity of the data in the database.

**Atomicity**: A database transaction is atomic if it is treated as a single unit of work, meaning that either all of the changes made in the transaction are committed, or none of them are. This ensures that the database remains in a consistent state, even if the transaction is interrupted or fails.

**Consistency**: A database transaction must maintain the consistency of the data in the database. This means that the transaction must not violate any of the constraints or rules that apply to the data, and it must leave the database in a valid state.

**Isolation**: A database transaction must be isolated from other transactions that are running concurrently. This means that the changes made in one transaction should not be visible to other transactions until the first transaction is committed. This helps to prevent conflicts and ensure the integrity of the data.

**Durability**: A database transaction must be durable, meaning that the changes made in the transaction should be permanently recorded in the database, even if the system fails or the database is closed. This ensures that the data is not lost and can be recovered if necessary.

In summary, the ACID properties ensure that database transactions are reliable and maintain the integrity of the data in the database, even in the face of system failures or concurrent updates.

### BASE 

* BA (Basic Availability)
* S (Soft-State)
* E (Eventual Consistency) )

BASE is an acronym that stands for **Basically Available, Soft state, Eventually consistent**. It is often used to describe the behavior of distributed databases or systems that do not follow the traditional ACID (Atomicity, Consistency, Isolation, Durability) model of database transactions.

BASE systems are designed to be highly available, meaning that they are able to continue processing requests even when some of their components fail or are unavailable. They are also designed to be soft state, meaning that the state of the system may change over time and may not be immediately consistent. Finally, BASE systems are eventually consistent, meaning that the data in the system will eventually converge to a consistent state, although it may take some time for this to happen.

In contrast to ACID systems, which strive to maintain the consistency and integrity of the data at all times, BASE systems are more focused on availability and the ability to handle large amounts of data and requests in a distributed environment. This can make them more suitable for certain types of applications, such as those that need to handle a high volume of requests or that need to be highly available. However, it can also make them more difficult to design and implement, and they may not be suitable for all types of applications.