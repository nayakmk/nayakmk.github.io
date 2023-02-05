---
layout: post
title: 'Postgresql: Partitioning Concepts '
date: '2023-02-04 19:03:14 +0530'
categories: [DATABASE]
tags: [postgresql, database]
---
## Why Partitioning

Partitioning refers to splitting one large table into smaller physical pieces that can be stored in different storage media based on its use. 

## Types of Partitioning 

There are mainly two types of PostgreSQL Partitions: Vertical Partitioning and Horizontal Partitioning. In vertical partitioning, we divide column-wise and in horizontal partitioning, we divide row-wise.

### Horizontal Partitioning

This involves putting different rows into different tables.

### Vertical Partitioning

This involves creating tables with fewer columns and using additional tables to store the remaining columns.

## How to Create a Partition Table

1. Determine the partitioning strategy: You can choose from `RANGE`, `LIST`, `HASH`, or `COMPOSITE` partitioning strategies.
2. Create the parent table: This is the table that will hold the data for all partitions. You can create it using the standard `CREATE TABLE` syntax.
3. Create the child tables: These are the tables that will hold the actual data for each partition. You can create them using the `CREATE TABLE` syntax, but with the additional `INHERITS` clause to specify that they inherit from the parent table.
4. Define the partitioning rules: You can do this using the `PARTITION BY` clause in the child table definition. For example, if you are using the `RANGE` partitioning strategy, you will specify the start and end values for each partition.
5. Create a trigger function: This function will be used to redirect inserts to the appropriate partition. You can create it using the `CREATE FUNCTION` syntax and define its behavior using the `PL/pgSQL` language.
6. Attach the trigger to the parent table: You can do this using the `CREATE TRIGGER` syntax.

### Range Partition

Here's an example of range partitioning in PostgreSQL:

Suppose you have a table called "sales" that stores sales data and you want to partition it based on the "date" column. You can create a parent table and multiple child tables for each partition:

#### Approach 1

```sql
-- Create the parent table
CREATE TABLE sales_range (
  date date,
  item_id int,
  quantity int,
  price numeric
);

-- Create the first child table for sales data from 2020-01-01 to 2020-06-30
CREATE TABLE sales_range_2020_1
INHERITS (sales_range)
PARTITION BY RANGE (date);

CREATE TABLE sales_range_2020_2
INHERITS (sales_range)
PARTITION BY RANGE (date);

CREATE TABLE sales_range_2020_3
INHERITS (sales_range)
PARTITION BY RANGE (date);

-- Add check constraints to specify the range of dates for each partition
ALTER TABLE sales_range_2020_1
  ADD CONSTRAINT sales_range_2020_1_check
  CHECK (date >= '2020-01-01' AND date <= '2020-06-30');

ALTER TABLE sales_range_2020_2
  ADD CONSTRAINT sales_range_2020_2_check
  CHECK (date >= '2020-07-01' AND date <= '2020-12-31');

ALTER TABLE sales_range_2020_3
  ADD CONSTRAINT sales_range_2020_3_check
  CHECK (date >= '2021-01-01' AND date <= '2021-06-30');

-- Create a trigger function to automatically redirect inserts to the appropriate partition
CREATE OR REPLACE FUNCTION insert_sales_range()
RETURNS TRIGGER AS $$
BEGIN
  IF (NEW.date >= '2020-01-01' AND NEW.date <= '2020-06-30') THEN
    INSERT INTO sales_range_2020_1 VALUES (NEW.*);
  ELSIF (NEW.date >= '2020-07-01' AND NEW.date <= '2020-12-31') THEN
    INSERT INTO sales_range_2020_2 VALUES (NEW.*);
  ELSE
    INSERT INTO sales_range_2020_3 VALUES (NEW.*);
  END IF;
  RETURN NULL;
END;
$$ LANGUAGE plpgsql;

-- Attach the trigger to the parent table
CREATE TRIGGER insert_sales_range_trigger
  BEFORE INSERT ON sales_range
  FOR EACH ROW EXECUTE FUNCTION insert_sales_range();

```

#### Approach 2

```sql
-- Create the table
CREATE TABLE sales_range (
  date date,
  item_id int,
  quantity int,
  price numeric
) PARTITION BY RANGE (date);

-- Create the first partition table for sales data from 2020-01-01 to 2020-06-30
CREATE TABLE sales_range_2020_1 PARTITION OF sales_range FOR VALUES FROM ('2020-01-01') TO ('2020-06-30');

-- Create the second partition table for sales data from 2020-07-01 to 2020-12-31
CREATE TABLE sales_range_2020_2 PARTITION OF sales_range FOR VALUES FROM ('2020-07-01') TO ('2020-12-31');

-- Create the third partition table for sales data from 2021-01-01 to 2021-06-30
CREATE TABLE sales_range_2021_1 PARTITION OF sales_range FOR VALUES FROM ('2021-01-01') TO ('2021-06-30');
```

With this setup, any insert into the "sales_range" table will be automatically redirected to the appropriate child partition based on the "date" column.

#### Example Data

```sql
INSERT INTO public.sales_range
("date", item_id, quantity, price)
VALUES('2020-01-01', 1, 10, 10.00);

INSERT INTO public.sales_range
("date", item_id, quantity, price)
VALUES('2021-01-01', 2, 10, 10.00);
```

![Range Partitions View](/assets/img/postgresql-range-partition.png){: .normal }


### List Partition

Suppose you have a table called "employees" that stores employee data and you want to partition it based on the "department" column. You can create a parent table and multiple child tables for each partition:

#### Approach 1

```sql
-- Create the parent table
CREATE TABLE employees_list (
  id int,
  name text,
  department text,
  salary int
) PARTITION BY LIST (department);

-- Create the child tables for each department
CREATE TABLE employees_list_sales
INHERITS (employees_list)
PARTITION BY LIST (department);

CREATE TABLE employees_list_marketing
INHERITS (employees_list)
PARTITION BY LIST (department);

CREATE TABLE employees_list_finance
INHERITS (employees_list)
PARTITION BY LIST (department);

-- Add a constraint to each child table to specify the department
ALTER TABLE employees_list_sales
  ADD CONSTRAINT employees_list_sales_check
  CHECK (department = 'sales');

ALTER TABLE employees_list_marketing
  ADD CONSTRAINT employees_list_marketing_check
  CHECK (department = 'marketing');

ALTER TABLE employees_list_finance
  ADD CONSTRAINT employees_list_finance_check
  CHECK (department = 'finance');

-- Create a trigger function to automatically redirect inserts to the appropriate partition
CREATE OR REPLACE FUNCTION insert_employees_list()
RETURNS TRIGGER AS $$
BEGIN
  IF (NEW.department = 'sales') THEN
    INSERT INTO employees_list_sales VALUES (NEW.*);
  ELSIF (NEW.department = 'marketing') THEN
    INSERT INTO employees_list_marketing VALUES (NEW.*);
  ELSE
    INSERT INTO employees_list_finance VALUES (NEW.*);
  END IF;
  RETURN NULL;
END;
$$ LANGUAGE plpgsql;

-- Attach the trigger to the parent table
CREATE TRIGGER insert_employees_list_trigger
  BEFORE INSERT ON employees_list
  FOR EACH ROW EXECUTE FUNCTION insert_employees_list();

```

#### Approach 2

```sql
-- Create the table
CREATE TABLE employees_list (
  id int,
  name text,
  department text,
  salary int
) PARTITION BY LIST (department);

-- Create the partitions
CREATE TABLE employees_list_sales PARTITION OF employees_list FOR VALUES IN ('sales');
CREATE TABLE employees_list_marketing PARTITION OF employees_list FOR VALUES IN ('marketing');
CREATE TABLE employees_list_finance PARTITION OF employees_list FOR VALUES IN ('finance');
```

#### Example

```sql
INSERT INTO public.employees_list (id, "name", department, salary) VALUES(1, 'John', 'sales', 20);

INSERT INTO public.employees_list (id, "name", department, salary) VALUES(2, 'Eva', 'marketing', 30);

INSERT INTO public.employees_list (id, "name", department, salary) VALUES(3, 'Chris', 'finance', 40);

select * from employees_list;

1	John	sales	20
2	Eva	marketing	30
3	Chris	finance	40

select * from employees_list_sales;

1	John	sales	20

select * from employees_list_marketing;

2	Eva	marketing	30

select * from employees_list_finance;

3	Chris	finance	40
```

![List Partitions View](/assets/img/postgresql-list-partition.png){: .normal }

With this setup, any insert into the "employees_list" table will be automatically redirected to the appropriate child partition based on the "department" column.

### Hash Partition

Suppose you have a table called "transactions" that stores financial transaction data and you want to partition it based on the "transaction_id" column. You can create a parent table and multiple child tables for each partition:

```sql
-- Create the parent table
CREATE TABLE transactions_hash (
  transaction_id int,
  amount int,
  account_number text,
  date date
);

-- Create the child tables for each partition
CREATE TABLE transactions_hash_1
INHERITS (transactions_hash)
PARTITION BY HASH (transaction_id);

CREATE TABLE transactions_hash_2
INHERITS (transactions_hash)
PARTITION BY HASH (transaction_id);

CREATE TABLE transactions_hash_3
INHERITS (transactions_hash)
PARTITION BY HASH (transaction_id);

-- Create a trigger function to automatically redirect inserts to the appropriate partition
CREATE OR REPLACE FUNCTION insert_transactions_hash()
RETURNS TRIGGER AS $$
DECLARE
  partition_number int;
BEGIN
  partition_number := (NEW.transaction_id % 3) + 1;
  IF (partition_number = 1) THEN
    INSERT INTO transactions_hash_1 VALUES (NEW.*);
  ELSIF (partition_number = 2) THEN
    INSERT INTO transactions_hash_2 VALUES (NEW.*);
  ELSE
    INSERT INTO transactions_hash_3 VALUES (NEW.*);
  END IF;
  RETURN NULL;
END;
$$ LANGUAGE plpgsql;

-- Attach the trigger to the parent table
CREATE TRIGGER insert_transactions_hash_trigger
  BEFORE INSERT ON transactions_hash
  FOR EACH ROW EXECUTE FUNCTION insert_transactions_hash();

```

#### Approach 2

```sql
CREATE TABLE transactions_hash (
  transaction_id int,
  amount int,
  account_number text,
  date date
) PARTITION BY HASH (transaction_id);

CREATE TABLE transactions_hash_1
PARTITION OF transactions_hash FOR VALUES WITH (MODULUS 3,REMAINDER 0);

CREATE TABLE transactions_hash_2
PARTITION OF transactions_hash FOR VALUES WITH (MODULUS 3,REMAINDER 1);

CREATE TABLE transactions_hash_3
PARTITION OF transactions_hash FOR VALUES WITH (MODULUS 3,REMAINDER 2);
```

#### Example

```sql
INSERT INTO public.transactions_hash
(transaction_id, amount, account_number, "date")
VALUES(1, 10, '123', '2023-02-05');

INSERT INTO public.transactions_hash
(transaction_id, amount, account_number, "date")
VALUES(2, 10, '123', '2023-02-05');

INSERT INTO public.transactions_hash
(transaction_id, amount, account_number, "date")
VALUES(3, 10, '123', '2023-02-05');

INSERT INTO public.transactions_hash
(transaction_id, amount, account_number, "date")
VALUES(4, 10, '123', '2023-02-05');

INSERT INTO public.transactions_hash
(transaction_id, amount, account_number, "date")
VALUES(5, 10, '123', '2023-02-05');

INSERT INTO public.transactions_hash
(transaction_id, amount, account_number, "date")
VALUES(6, 10, '123', '2023-02-05');

INSERT INTO public.transactions_hash
(transaction_id, amount, account_number, "date")
VALUES(7, 10, '123', '2023-02-05');

$ select * from transactions_hash_1;

2	10	123	2023-02-05
4	10	123	2023-02-05
6	10	123	2023-02-05

$ select * from transactions_hash_2;

3	10	123	2023-02-05
7	10	123	2023-02-05

$ select * from transactions_hash_3;

1	10	123	2023-02-05
5	10	123	2023-02-05
```

![Hash Partitions View](/assets/img/postgresql-hashpartition.png){: .normal }

### Sub Partition

```sql
-- Create table
CREATE TABLE orders (
  order_id int,
  order_date date,
  customer_id int,
  product_id int,
  quantity int,
  amount float
) PARTITION BY RANGE (order_date);

-- Create first partition based on order date
CREATE TABLE orders_2022 PARTITION OF orders FOR VALUES FROM ('2022-01-01') TO ('2023-01-01') PARTITION BY LIST (product_id);

-- Create second partition based on order date
CREATE TABLE orders_2023 PARTITION OF orders FOR VALUES FROM ('2023-01-01') TO ('2024-01-01') PARTITION BY LIST (product_id);

-- Create sub partition A1 based on product ID on first partition based on order date
CREATE TABLE orders_2022_A1 PARTITION OF orders_2022 FOR values IN (1);

-- Create sub partition A2 based on product ID on first partition based on order date
CREATE TABLE orders_2022_A2 PARTITION OF orders_2022 FOR values IN (2);
```



#### Example

```sql
INSERT INTO public.orders
(order_id, order_date, customer_id, product_id, quantity, amount)
VALUES(1, '2022-02-01', 1, 1, 10, 10);

INSERT INTO public.orders
(order_id, order_date, customer_id, product_id, quantity, amount)
VALUES(2, '2022-02-01', 2, 2, 10, 10);

INSERT INTO public.orders
(order_id, order_date, customer_id, product_id, quantity, amount)
VALUES(3, '2022-02-01', 3, 1, 10, 10);

select * from public.orders_2022

1	2022-02-01	1	1	10	10.0
3	2022-02-01	3	1	10	10.0
2	2022-02-01	2	2	10	10.0

select * from public.orders_2022_A1

1	2022-02-01	1	1	10	10.0
3	2022-02-01	3	1	10	10.0

select * from public.orders_2022_A2 

2	2022-02-01	2	2	10	10.0
```

![Sub Partitions View](/assets/img/postgresql-sub-partition.png){: .normal }

## Further Reading

1. https://hevodata.com/learn/postgresql-partitions/
2. https://www.postgresql.org/docs/current/ddl-partitioning.html
3. https://www.percona.com/blog/2019/05/24/an-overview-of-sharding-in-postgresql-and-how-it-relates-to-mongodbs/
4. https://www.singlestore.com/blog/database-sharding-vs-partitioning-whats-the-difference/
5. https://www.percona.com/blog/2018/08/21/foreign-data-wrappers-postgresql-postgres_fdw/