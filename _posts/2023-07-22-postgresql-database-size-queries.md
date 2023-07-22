---
layout: post
title: PostgreSQL Database Size Queries
date: '2023-07-22 17:36:53 +0530'
tags: [PostgreSQL, Database, pg_database_size, pg_relation_size, pg_total_relation_size]
categories: [Database]
---

In PostgreSQL, monitoring database size is crucial for managing disk space and optimizing performance. PostgreSQL provides several functions to retrieve the size of the database and individual relations (tables and indexes). In this post, we will explore three important functions: `pg_database_size()`, `pg_relation_size()`, and `pg_total_relation_size()`. We will demonstrate their usage with examples.

## 1. pg_database_size()

The `pg_database_size()` function returns the size of a specific database in bytes.

**Example:**

```sql
SELECT pg_database_size('my_database') AS size;
```

This query will return the size of the database named "my_database."

## 2. pg_relation_size()

The `pg_relation_size()` function returns the size of a specific relation (table or index) within a database in bytes.

**Example:**

```sql
SELECT pg_relation_size('public.my_table') AS size;
```

This query will return the size of the table named "my_table" within the "public" schema.

## 3. pg_total_relation_size()

The `pg_total_relation_size()` function returns the total size of a specific relation, including the main relation and all associated indexes, in bytes.

**Example:**

```sql
SELECT pg_total_relation_size('public.my_table') AS size;
```

This query will return the total size of the table named "my_table" along with its associated indexes within the "public" schema.

## Using the Functions Together

Let's combine these functions to retrieve the sizes of multiple relations within a database.

**Example:**

```sql
SELECT table_name,
       pg_total_relation_size(table_name) AS total_size,
       pg_relation_size(table_name) AS table_size,
       pg_total_relation_size(table_name) - pg_relation_size(table_name) AS index_size
FROM information_schema.tables
WHERE table_schema = 'public';
```

This query will display the sizes of all tables in the "public" schema along with their respective total size, table size, and index size.

## Conclusion

Monitoring the size of a PostgreSQL database and its relations is essential for efficient database management. The `pg_database_size()`, `pg_relation_size()`, and `pg_total_relation_size()` functions provide valuable insights into the disk space usage of databases and individual relations. By utilizing these functions, administrators and developers can make informed decisions regarding database maintenance, optimization, and capacity planning.