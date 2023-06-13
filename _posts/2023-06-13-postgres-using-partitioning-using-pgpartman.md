---
layout: post
title: 'Postgres: Using Partitioning using pg_partman'
date: 2023-06-13 09:30 +0530
categories: [DATABASE]
tags: [postgresql, database, partitioning]
---

### PostgreSQL with pg_partman extension

```sh
docker run --name postgres -e POSTGRES_PASSWORD=postgres -p 5432:5432 scarfacedeb/postgres-pg-partman
```

### Step By Step - Partitioning

1. Enable the **pg_partman** extension

   ```sql
   CREATE SCHEMA partman;
   CREATE EXTENSION pg_partman WITH SCHEMA partman;
   ```

2. Create the parent table which acts like **template**. 

   ```sql
   CREATE TABLE public.sales (
       id SERIAL,
       sale_date DATE,
       region TEXT,
       amount numeric,
       primary key (id, sale_date)
   )
   PARTITION BY RANGE (sale_date);
   ```

3. Create main partition, let's say yearly tables.

   ```sql
   SELECT partman.create_parent(
   p_parent_table => 'public.sales', p_control => 'sale_date', p_type => 'native',  p_interval=> 'yearly');
   ```

4. Create sub Partition, let's say by monthly tables. 

   ```sql
   SELECT partman.create_sub_parent(
   p_top_parent => 'public.sales',p_control => 'sale_date', p_type => 'native', p_interval=> 'monthly', p_native_check => 'yes',p_premake => 10);
   ```

### Reference

1. https://www.capitalone.com/tech/cloud/automating-partitioning/