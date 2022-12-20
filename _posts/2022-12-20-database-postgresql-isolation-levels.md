---
layout: post
title: 'Database: PostgreSQL Isolation Levels'
date: '2022-12-20 10:20:04 +0530'
categories: [DISTRIBUTED, DATABASE]
tags: [isolation, database, docker, postgres]
---

### Start PostgreSQL using Docker

> docker run -name pgsql-dev -e POSTGRES_PASSWORD=Welcome4$ -p 5432:5432 postgres 

### Start psql

Start bash:

```bash
docker exec -it pgsql-dev bash 
```

Launch psql:

```bash
psql -h localhost -U postgres
```

### Default Isolation Level

```sql
psql> show transaction isolation level;
transaction_isolation
-----------------------
read committed
(1 row)
```

### Changing Isolation Level - Per Transaction

```sql
psql> begin;
BEGIN

psql> set transaction isolation level read uncommitted;
SET

psql> commit;
COMMIT
```

### Read Un-Committed Example

```false
psql> set transaction isolation level read uncommitted;
SET

psql> show transaction isolation level;
transaction_isolation
-----------------------
 read uncommitted
(1 row)
```

### Read Committed Example

```false
psql> set transaction isolation level read committed;
SET

psql> show transaction isolation level;
transaction_isolation
-----------------------
 read committed
(1 row)
```

Dirty read is not possible in this isolation but we can see phantom read.

### Repeatable Read

To avoid phantom read, we can enable this isolation, which ensures that the other transaction sees the same data upon repeated query as the state before the transaction began.

```false
psql> set transaction isolation level repeatable read;
SET

psql> show transaction isolation level;
transaction_isolation
-----------------------
 repeatable read
(1 row)
```

In this isolation strategy, it is possible for the transaction to have any knowledge about what is happening in other transaction, resulting in DB allowing same/similar entries to DB, resulting in serializable anomaly.

```
ERROR: could not serialize access due to concurrent update
```

### Serializable

```false
psql> set transaction isolation level serializable;
SET

psql> show transaction isolation level;
transaction_isolation
-----------------------
 serializable
(1 row)
```

This isolation avoids, all form of anomalies from the DB e.g. Dirty Read, Non repeatable read, phantom read, and serialization anomaly. If another transaction tries to update/insert into DB while another transaction has performed the operation, then it's rejected.

```
ERROR: could not serialize access due to read/write dependencies among transactions
```

### Further Reading

https://www.sqlshack.com/getting-started-with-postgresql-on-docker/

**Beautiful Demonstration:**

https://dev.to/techschoolguru/understand-isolation-levels-read-phenomena-in-mysql-postgres-c2e