# SQL Optimization Partitioning

> Source notebook: [DE SQL Notes/SQL\_Optimization\_Partitioning.ipynb](https://github.com/karthikeyanm-dev/DE-Notes/blob/main/DE%20SQL%20Notes/SQL_Optimization_Partitioning.ipynb). Notebook outputs are omitted; open the source to run cells.

```sql
%reload_ext sql
%sql mysql+mysqlconnector://root:123456@localhost
```

```sql
%%sql
-- create schema sql_optimization;
use sql_optimization;
```

## Table Partitioning in MySQL

### Introduction

As databases grow, storing millions or even billions of rows in a single table becomes increasingly difficult to manage and query efficiently.

Imagine a library containing **50 million books** stored in one giant room. Even if the books are arranged neatly, searching, maintaining, and organizing them becomes challenging.

Instead, the library divides books into separate rooms based on categories or publication years. Finding a book becomes much faster because you only search one room instead of the entire library.

MySQL follows the same concept using **Table Partitioning**.

Partitioning divides one large table into multiple smaller physical partitions while allowing users to query it as if it were a single table.

***

## Why Do We Need Partitioning?

Without partitioning:

* Queries may scan a huge amount of unnecessary data.
* Maintenance operations become slower.
* Archiving old data becomes difficult.
* Backup and recovery take longer.

Partitioning helps MySQL access only the required partition instead of reading the entire table.

***

## What is Table Partitioning?

### Professional Definition

Table Partitioning is a database optimization technique that divides a large table into multiple physical partitions based on a partitioning rule while presenting the table as a single logical object to the user.

***

### Beginner Definition

Partitioning is simply splitting one large table into smaller pieces so MySQL can find data faster.

***

### One-Line Definition

> Partitioning divides one table into multiple smaller partitions to improve performance and manageability.

***

## Real-Life Analogy

Imagine a school stores student records.

Instead of storing every student's record in one huge cupboard,

they organize them like this:

```
Cupboard

├── Class 1
├── Class 2
├── Class 3
├── Class 4
└── Class 5
```

When searching for a Class 3 student,

there is no need to search every cupboard.

Only the Class 3 section is searched.

Partitioning works exactly the same way.

***

## Types of Partitioning in MySQL

MySQL supports multiple partitioning strategies.

In this chapter, we will cover:

* RANGE Partition
* LIST Partition
* HASH Partition

***

## 1. RANGE Partition

### Definition

Rows are stored based on a range of values.

Each partition stores values that fall within a specific range.

***

### Best Used For

* Years
* Dates
* Sales History
* Order History
* Time-Series Data

***

### Syntax

```sql
CREATE TABLE orders (
    order_id INT,
    order_date DATE
)
PARTITION BY RANGE (YEAR(order_date))
(
    PARTITION p2023 VALUES LESS THAN (2024),
    PARTITION p2024 VALUES LESS THAN (2025),
    PARTITION pFuture VALUES LESS THAN MAXVALUE
);
```

***

### Data Distribution

| Order Date | Partition |
| ---------- | --------- |
| 2023-02-12 | p2023     |
| 2024-07-10 | p2024     |
| 2026-01-01 | pFuture   |

***

### Visualization

```
Orders Table

2023 Orders
│
├── Order 1
├── Order 2
└── Order 3

↓

Partition p2023
```

```
2024 Orders
│
├── Order 4
├── Order 5
└── Order 6

↓

Partition p2024
```

***

### Advantages

* Faster date filtering
* Easy archival
* Excellent for historical data
* Common in banking and e-commerce

***

```sql
%%sql
CREATE TABLE orders_partition (
    order_id INT,
    customer_name VARCHAR(50),
    order_date DATE,
    amount DECIMAL(10,2)
)
PARTITION BY RANGE (YEAR(order_date))
(
    PARTITION p2023 VALUES LESS THAN (2024),
    PARTITION p2024 VALUES LESS THAN (2025),
    PARTITION pFuture VALUES LESS THAN MAXVALUE
);
```

```sql
%%sql
INSERT INTO orders_partition VALUES
(101,'John','2023-02-15',1200),
(102,'David','2023-08-11',850),
(103,'Sara','2024-01-20',1500),
(104,'Emma','2024-10-08',950),
(105,'Alex','2026-03-05',1800);
```

```sql
%%sql
SELECT
    PARTITION_NAME,
    TABLE_ROWS
FROM INFORMATION_SCHEMA.PARTITIONS
WHERE TABLE_NAME='orders_partition';
```

## 2. LIST Partition

### Definition

Rows are stored according to specific predefined values.

Instead of defining ranges, you specify exactly which values belong to each partition.

***

### Best Used For

* Department
* Country
* Region
* State
* Category

***

### Syntax

```sql
CREATE TABLE employees (
    emp_id INT,
    department VARCHAR(20)
)
PARTITION BY LIST COLUMNS(department)
(
    PARTITION pIT VALUES IN ('IT'),
    PARTITION pHR VALUES IN ('HR'),
    PARTITION pSales VALUES IN ('Sales'),
    PARTITION pOther VALUES IN ('Finance','Marketing')
);
```

***

### Data Distribution

| Department | Partition |
| ---------- | --------- |
| IT         | pIT       |
| HR         | pHR       |
| Sales      | pSales    |
| Finance    | pOther    |
| Marketing  | pOther    |

***

### Visualization

```
Employees

IT
│
├── John
├── Alex
└── David

↓

Partition pIT
```

```
HR
│
├── Sara
└── Emma

↓

Partition pHR
```

***

### Advantages

* Simple organization
* Easy reporting
* Ideal for fixed categories

***

```sql
%%sql
CREATE TABLE employees_partition (
    emp_id INT,
    emp_name VARCHAR(50),
    department VARCHAR(20),
    salary INT
)
PARTITION BY LIST COLUMNS(department)
(
    PARTITION pIT VALUES IN ('IT'),
    PARTITION pHR VALUES IN ('HR'),
    PARTITION pSales VALUES IN ('Sales'),
    PARTITION pOther VALUES IN ('Finance','Marketing')
);
```

```sql
%%sql
INSERT INTO employees_partition VALUES
(1,'John','IT',60000),
(2,'Sara','HR',45000),
(3,'David','Sales',55000),
(4,'Emma','Finance',70000),
(5,'Alex','Marketing',65000);
```

```sql
%%sql
SELECT
    PARTITION_NAME,
    TABLE_ROWS
FROM INFORMATION_SCHEMA.PARTITIONS
WHERE TABLE_NAME='employees_partition';
```

## 3. HASH Partition

### Definition

Rows are distributed automatically using a hash function.

The database calculates a hash value and determines which partition should store each row.

***

### Best Used For

* Customer IDs
* User IDs
* Product IDs
* Large OLTP Systems

***

### Syntax

```sql
CREATE TABLE customers (
    customer_id INT,
    customer_name VARCHAR(100)
)
PARTITION BY HASH(customer_id)
PARTITIONS 4;
```

***

### How HASH Works

MySQL internally calculates:

```
Partition = customer_id % Number_of_partitions
```

Example:

```
Customer ID = 15

15 % 4 = 3

↓

Partition 3
```

***

### Example Distribution

| Customer ID | Result | Partition |
| ----------- | ------ | --------- |
| 1           | 1      | P1        |
| 2           | 2      | P2        |
| 3           | 3      | P3        |
| 4           | 0      | P0        |
| 5           | 1      | P1        |
| 6           | 2      | P2        |

***

### Advantages

* Uniform data distribution
* Automatic balancing
* Prevents hotspot partitions
* Excellent for high-traffic applications

***

## Comparison of Partition Types

| Feature                | RANGE           | LIST            | HASH              |
| ---------------------- | --------------- | --------------- | ----------------- |
| Based On               | Range of values | Specific values | Hash Function     |
| Partition Selected By  | User            | User            | MySQL             |
| Best For               | Dates, Years    | Categories      | Even Distribution |
| Automatic Distribution | ❌               | ❌               | ✅                 |
| Archiving              | Easy            | Moderate        | Difficult         |
| Load Balancing         | Low             | Medium          | Excellent         |

***

## Real-World Business Examples

| Industry     | Partition Type | Example                 |
| ------------ | -------------- | ----------------------- |
| Banking      | RANGE          | Transactions by Year    |
| E-Commerce   | RANGE          | Orders by Month         |
| Hospital     | LIST           | Patients by Region      |
| HR           | LIST           | Employees by Department |
| Gaming       | HASH           | Players by Player ID    |
| Social Media | HASH           | Users by User ID        |
| Logistics    | RANGE          | Shipment History        |

***

## Which Partition Should You Choose?

| Scenario            | Recommended Partition |
| ------------------- | --------------------- |
| Sales by Year       | RANGE                 |
| Orders by Month     | RANGE                 |
| Employee Department | LIST                  |
| Customer Region     | LIST                  |
| User ID             | HASH                  |
| Product ID          | HASH                  |

***

## Common Mistakes

#### 1. Choosing HASH for Date Queries

Dates should generally use RANGE partitioning.

***

#### 2. Creating Too Many Partitions

Thousands of partitions increase management overhead.

***

#### 3. Partitioning Small Tables

Small tables usually gain little or no benefit from partitioning.

***

#### 4. Ignoring Query Patterns

Choose the partitioning strategy based on how your application queries data.

***

```sql
%%sql
CREATE TABLE customers_partition (
    customer_id INT,
    customer_name VARCHAR(50),
    city VARCHAR(30)
)
PARTITION BY HASH(customer_id)
PARTITIONS 4;
```

```sql
%%sql
INSERT INTO customers_partition VALUES
(1,'John','Chennai'),
(2,'Sara','Delhi'),
(3,'David','Mumbai'),
(4,'Emma','Bangalore'),
(5,'Alex','Hyderabad'),
(6,'Kevin','Pune'),
(7,'Sophia','Kolkata'),
(8,'James','Jaipur');
```

```sql
%%sql
SELECT
    PARTITION_NAME,
    TABLE_ROWS
FROM INFORMATION_SCHEMA.PARTITIONS
WHERE TABLE_NAME='customers_partition';
```
