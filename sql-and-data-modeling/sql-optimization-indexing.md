# SQL Optimization Indexing

> Source notebook: [DE SQL Notes/SQL\_Optimization\_Indexing.ipynb](https://github.com/karthikeyanm-dev/DE-Notes/blob/main/DE%20SQL%20Notes/SQL_Optimization_Indexing.ipynb). Notebook outputs are omitted; open the source to run cells.

```sql
%reload_ext sql
%sql mysql+mysqlconnector://root:123456@localhost
```

```sql
%%sql
-- create schema sql_optimization;
use sql_optimization;
```

```sql
%%sql
CREATE TABLE employees (
    emp_id      INT PRIMARY KEY,
    emp_name    VARCHAR(50)  NOT NULL,
    department  VARCHAR(30)  NOT NULL,
    city        VARCHAR(30)  NOT NULL,
    salary      DECIMAL(10,2) NOT NULL,
    join_date   DATE          NOT NULL
);
 
INSERT INTO employees (emp_id, emp_name, department, city, salary, join_date) VALUES
(1,  'Ram',    'IT',      'Chennai',   30000, '2023-01-10'),
(2,  'Divya',  'IT',      'Bengaluru', 70000, '2021-03-15'),
(3,  'Kumar',  'IT',      'Chennai',   40000, '2022-07-20'),
(4,  'Priya',  'HR',      'Mumbai',    45000, '2022-02-10'),
(5,  'Arun',   'HR',      'Chennai',   55000, '2020-09-05'),
(6,  'John',   'Sales',   'Delhi',     60000, '2021-11-12'),
(7,  'Meena',  'Sales',   'Mumbai',    60000, '2023-05-01'),
(8,  'Ravi',   'Sales',   'Chennai',   35000, '2024-01-15'),
(9,  'Sara',   'Finance', 'Bengaluru', 52000, '2022-10-01'),
(10, 'Vikram', 'Finance', 'Delhi',     48000, '2023-08-19');
```

```sql
%%sql
CREATE TABLE orders (
    order_id    INT PRIMARY KEY,
    customer_id INT           NOT NULL,
    order_date  DATE          NOT NULL,
    amount      DECIMAL(10,2) NOT NULL,
    status      VARCHAR(20)   NOT NULL
);
 
INSERT INTO orders (order_id, customer_id, order_date, amount, status) VALUES
(1, 501, '2024-01-02', 1200.00, 'DELIVERED'),
(2, 502, '2024-01-03',  450.00, 'DELIVERED'),
(3, 501, '2024-01-05',  980.00, 'CANCELLED'),
(4, 503, '2024-01-06', 2200.00, 'DELIVERED'),
(5, 504, '2024-01-08',  150.00, 'PENDING'),
(6, 502, '2024-01-09',  610.00, 'DELIVERED'),
(7, 505, '2024-01-11', 3000.00, 'DELIVERED'),
(8, 501, '2024-01-14',  720.00, 'PENDING');
```

## Chapter 20 — How SQL Executes a Query

Until now, we have learned **how to write SQL queries**.

We know how to retrieve data using `SELECT`, filter rows using `WHERE`, combine tables using `JOIN`, summarize data using `GROUP BY`, analyze data using Window Functions, and merge result sets using Set Operators.

But have you ever wondered what happens **inside the database** after you press **Enter**?

When you execute a query, does the database immediately start reading the table?

The answer is **No**.

Before reading a single row from the database, SQL performs several internal steps.

It checks your query, verifies whether the tables and columns exist, finds the fastest execution strategy, retrieves the required data, and finally returns the result.

Understanding this process is one of the most important skills for SQL developers because every performance optimization begins with knowing **how the database thinks**.

This chapter explains the complete journey of a SQL query—from the moment you execute it until the results appear on your screen.

***

## Learning Objectives

After completing this chapter, you will be able to:

* Understand the complete SQL query lifecycle.
* Explain the responsibilities of the Parser, Semantic Analyzer, Query Optimizer, Execution Engine, and Storage Engine.
* Understand how the database chooses an execution plan.
* Follow the complete execution flow of a SQL query.
* Prepare for learning execution plans (`EXPLAIN`) and indexes.

***

## Chapter Roadmap

```
You Write SQL

↓

Parser

↓

Semantic Analyzer

↓

Query Optimizer

↓

Execution Engine

↓

Storage Engine

↓

Result Returned
```

Every query executed by a relational database follows this pipeline.

Although the internal implementation differs between MySQL, PostgreSQL, SQL Server, and Oracle, the overall execution process is remarkably similar.

***

## 20.1 Why Query Execution Matters

Many developers focus only on writing SQL queries.

Professional developers also understand **how those queries are executed**.

Consider the following query.

```sql
SELECT *
FROM employee
WHERE salary > 50000;
```

The query is only one line long.

However, internally, the database performs dozens of operations before returning the result.

For example:

* Is the SQL syntax valid?
* Does the `employee` table exist?
* Does the `salary` column exist?
* Does the user have permission to access the table?
* Should the database scan the entire table?
* Is there an index that can make the query faster?
* What is the cheapest execution plan?

Only after answering these questions does the database execute the query.

Understanding these internal decisions helps you:

* Write faster SQL queries.
* Read execution plans confidently.
* Diagnose slow queries.
* Design efficient indexes.
* Perform better in SQL interviews.

***

## Real-World Analogy

Imagine ordering food at a restaurant.

You don't walk into the kitchen and immediately receive your meal.

Instead, several steps happen.

```
Customer Places Order

↓

Waiter Checks Order

↓

Chef Reviews Ingredients

↓

Chef Chooses Cooking Method

↓

Kitchen Prepares Food

↓

Food Served
```

The SQL engine follows a similar process.

```
Write Query

↓

Parser

↓

Semantic Analyzer

↓

Optimizer

↓

Execution Engine

↓

Storage Engine

↓

Result
```

Each component has a specific responsibility.

If any step fails, the query stops immediately.

***

## Think Like the Database

Suppose you execute:

```sql
SELECT employee_name
FROM employee
WHERE salary > 50000;
```

You might think:

```
Read Employee Table

↓

Return Result
```

The database thinks differently.

```
Receive Query

↓

Validate Syntax

↓

Validate Objects

↓

Create Execution Plan

↓

Retrieve Data

↓

Return Result
```

This difference is what separates beginner SQL users from database professionals.

***

## What's Coming Next?

In the following sections, we will study each stage in detail.

1. SQL Query Lifecycle
2. Parser
3. Semantic Analyzer
4. Query Optimizer
5. Execution Engine
6. Storage Engine
7. Complete Query Execution Flow
8. End-to-End Example

By the end of this chapter, you will understand exactly what happens inside a database every time a SQL query is executed.

## 20.2 SQL Query Lifecycle

Every SQL query follows a series of internal steps before returning a result.

This sequence is called the **SQL Query Lifecycle**.

Think of it as the journey a query takes from the moment you execute it until the final output appears on your screen.

Regardless of whether you use MySQL, PostgreSQL, SQL Server, or Oracle, every relational database follows a similar lifecycle.

Although the internal implementation differs, the overall stages remain almost the same.

***

### SQL Query Lifecycle

```
User Writes Query

↓

Parser

↓

Semantic Analyzer

↓

Query Optimizer

↓

Execution Engine

↓

Storage Engine

↓

Result Returned
```

Each stage has a specific responsibility.

The output of one stage becomes the input for the next stage.

If any stage encounters an error, query execution stops immediately.

***

### What Happens at Each Stage?

| Stage             | Responsibility                                                 |
| ----------------- | -------------------------------------------------------------- |
| Parser            | Checks SQL syntax and creates a parse tree                     |
| Semantic Analyzer | Validates tables, columns, functions, aliases, and permissions |
| Query Optimizer   | Finds the most efficient execution plan                        |
| Execution Engine  | Executes the optimized plan                                    |
| Storage Engine    | Reads and writes the actual data from disk or memory           |

***

### Think Like the Database

Suppose you execute

```sql
SELECT employee_name
FROM employee
WHERE salary > 50000;
```

The database does **not** immediately search the employee table.

Instead, it asks several questions.

```
Is the SQL syntax valid?

↓

Does the table exist?

↓

Does the salary column exist?

↓

Does the user have permission?

↓

Should I use an index?

↓

How should I execute the query?

↓

Retrieve Data

↓

Return Result
```

Only after all these questions are answered does the database return the final result.

***

### Why Understanding the Lifecycle Matters

Understanding the query lifecycle helps you:

* Write more efficient SQL queries.
* Read execution plans confidently.
* Understand why some queries are slow.
* Identify optimization opportunities.
* Build better indexes.
* Troubleshoot database performance issues.

***

### Key Takeaways

* Every SQL query follows a defined execution lifecycle.
* Each stage performs a different task.
* The database never executes a query immediately after receiving it.
* The Query Optimizer determines the best execution strategy before any data is retrieved.

## 20.3 Parser

The **Parser** is the first component that receives your SQL query.

Its primary responsibility is to verify that the SQL statement follows the correct syntax.

If the query contains syntax errors, execution stops immediately.

No optimization or data retrieval occurs.

***

### Real-World Analogy

Think of the Parser as a language teacher checking a sentence for grammar.

```
Sentence

↓

Grammar Check

↓

Valid?

↓

Yes → Continue

No → Error
```

Similarly,

```
SQL Query

↓

Parser

↓

Syntax Valid?

↓

Yes → Continue

No → Stop
```

***

### Responsibilities of the Parser

The Parser performs several tasks.

* Checks SQL keywords.
* Verifies brackets and parentheses.
* Validates query structure.
* Identifies tokens.
* Builds a Parse Tree.

***

### Example

Correct Query

```sql
SELECT employee_name
FROM employee;
```

Result

```
Syntax Valid

↓

Next Stage
```

***

Incorrect Query

```sql
SELEC employee_name
FROM employee;
```

Output

```
Syntax Error

↓

Query Stops
```

***

Another Example

```sql
SELECT
FROM employee;
```

The Parser detects that the SELECT list is incomplete.

Again,

execution stops immediately.

***

### Parse Tree

After validating the syntax,

the Parser converts the SQL query into an internal representation called a **Parse Tree** (or Syntax Tree).

Example

```sql
SELECT employee_name
FROM employee
WHERE salary > 50000;
```

Simplified Parse Tree

```
SELECT

├── employee_name

FROM

├── employee

WHERE

└── salary > 50000
```

The Parse Tree is passed to the next stage.

***

### Important Note

The Parser **does not know** whether the table actually exists.

It only checks whether the SQL statement is written correctly.

Object validation happens in the next stage.

***

### Key Takeaways

* The Parser validates SQL syntax.
* It identifies tokens and creates a Parse Tree.
* It does not verify database objects.
* Syntax errors stop query execution immediately.

## 20.4 Semantic Analyzer

Once the SQL query passes the Parser,

it reaches the **Semantic Analyzer**.

At this stage,

the syntax is already correct.

Now the database checks whether the query actually makes sense.

***

### Definition

The Semantic Analyzer verifies that all database objects referenced in the query are valid.

This includes:

* Tables
* Columns
* Views
* Functions
* Data Types
* Aliases
* Permissions

If any object cannot be resolved,

query execution stops.

***

### Real-World Analogy

Imagine filling out a bank cheque.

The cheque format may be correct.

However,

the bank still verifies:

* Does the account exist?
* Is the account active?
* Is the signature valid?

Only then is the cheque processed.

Similarly,

the Semantic Analyzer validates database objects.

***

### Responsibilities

The Semantic Analyzer checks:

* Does the table exist?
* Does the column exist?
* Is the function valid?
* Are aliases correctly referenced?
* Does the user have permission?
* Are data types compatible?

***

### Example

Valid Query

```sql
SELECT employee_name
FROM employee;
```

Result

```
Table Exists

↓

Column Exists

↓

Permission Granted

↓

Continue
```

***

Invalid Table

```sql
SELECT *
FROM employees;
```

Suppose the table is actually named

```
employee
```

Output

```
Table Does Not Exist

↓

Query Stops
```

***

Invalid Column

```sql
SELECT salary_amount
FROM employee;
```

Suppose

```
salary_amount
```

does not exist.

Result

```
Unknown Column

↓

Stop
```

***

Permission Example

Even if the table exists,

the user may not have permission.

```
Permission Denied

↓

Query Stops
```

***

### Key Takeaways

* The Semantic Analyzer validates database objects.
* It checks tables, columns, functions, aliases, and permissions.
* Syntax is already validated before this stage.
* Invalid objects stop query execution before optimization.

## 20.5 Query Optimizer

After the SQL query passes both the **Parser** and the **Semantic Analyzer**, it reaches one of the most intelligent components of the database.

The **Query Optimizer**.

The Query Optimizer is often called the **brain of the database** because it decides **how the query should be executed**.

Its goal is simple:

> **Find the fastest and most efficient way to execute the query.**

The optimizer does **not** change the result of the query.

Instead,

it changes **how the query is executed**.

***

### Real-World Analogy

Imagine you want to travel from Chennai to Bangalore.

There are multiple routes.

```
Route A

↓

500 km

↓

7 Hours
```

```
Route B

↓

520 km

↓

6 Hours
```

```
Route C

↓

480 km

↓

Heavy Traffic

↓

9 Hours
```

Google Maps chooses the best route.

Similarly,

the Query Optimizer chooses the best execution plan.

***

### Responsibilities

The Query Optimizer decides

* Which table should be read first?
* Should a Full Table Scan be used?
* Can an Index Scan be used?
* Which JOIN algorithm is faster?
* Can filters be applied earlier?
* What is the estimated execution cost?

***

### Example

```sql
SELECT *
FROM employee
WHERE employee_id = 101;
```

Suppose an index exists on `employee_id`.

The optimizer has two choices.

#### Option 1

```
Read Entire Table

↓

Find Row 101
```

***

#### Option 2

```
Use Index

↓

Jump Directly

↓

Read Row 101
```

The optimizer chooses the second option because it is much faster.

***

### Cost-Based Optimization

Modern databases use a **Cost-Based Optimizer (CBO)**.

Instead of guessing,

the optimizer estimates the cost of different execution plans.

Example

| Execution Plan  | Estimated Cost |
| --------------- | -------------: |
| Full Table Scan |            150 |
| Index Scan      |             12 |

The optimizer chooses

```
Cost = 12
```

because it is cheaper.

***

### Execution Plan

After choosing the best strategy,

the optimizer generates an **Execution Plan**.

This execution plan tells the database

exactly

how the query should be executed.

Later,

in the next chapter,

we'll learn how to view this plan using

```sql
EXPLAIN
```

***

### Key Takeaways

* The Query Optimizer chooses the most efficient execution strategy.
* It compares multiple execution plans.
* Modern databases use Cost-Based Optimization.
* The optimizer creates the Execution Plan used by the Execution Engine.

## 20.6 Execution Engine

Once the Query Optimizer creates the execution plan,

the plan is sent to the **Execution Engine**.

The Execution Engine is responsible for actually executing the optimized plan.

Think of it as the worker that follows the optimizer's instructions.

***

### Definition

The Execution Engine executes the execution plan generated by the Query Optimizer.

It coordinates with the Storage Engine to retrieve or modify data.

***

### Real-World Analogy

Imagine a construction project.

```
Architect

↓

Creates Blueprint

↓

Construction Workers

↓

Build House
```

The architect designs the plan.

The workers execute it.

Similarly,

```
Query Optimizer

↓

Creates Execution Plan

↓

Execution Engine

↓

Executes Plan
```

***

### Responsibilities

The Execution Engine

* Executes the execution plan.
* Calls the Storage Engine when data is required.
* Performs joins.
* Applies filters.
* Calculates aggregates.
* Sorts data.
* Produces the final result.

***

### Example

```sql
SELECT employee_name
FROM employee
WHERE salary > 50000;
```

The optimizer decides

```
Use Salary Index
```

The Execution Engine now performs

```
Read Index

↓

Locate Matching Rows

↓

Retrieve Employee Names

↓

Return Result
```

***

### Think Like SQL

```
Execution Plan

↓

Read Rows

↓

Apply WHERE

↓

Perform JOIN

↓

Sort

↓

Aggregate

↓

Return Result
```

The Execution Engine follows this sequence exactly.

***

### Key Takeaways

* The Execution Engine executes the optimized plan.
* It does not decide the strategy.
* It works closely with the Storage Engine.
* It produces the final query result.

## 20.7 Storage Engine

The **Storage Engine** is responsible for storing and retrieving the actual data.

While the Query Optimizer decides _how_ to execute a query,

and the Execution Engine performs the work,

the Storage Engine is responsible for accessing the physical data.

***

### Definition

The Storage Engine manages how data is stored,

retrieved,

updated,

and deleted.

It communicates directly with

* Disk
* Memory
* Indexes
* Data Pages

***

### Real-World Analogy

Imagine a library.

```
Reader

↓

Librarian

↓

Bookshelf

↓

Book
```

The librarian doesn't own the books.

The librarian simply retrieves them.

Similarly,

```
Execution Engine

↓

Storage Engine

↓

Disk

↓

Data
```

***

### Responsibilities

The Storage Engine

* Reads data pages.
* Writes new records.
* Updates existing rows.
* Deletes rows.
* Uses indexes.
* Manages disk storage.
* Handles caching.

***

### Example

```sql
SELECT *
FROM employee
WHERE employee_id = 101;
```

The Execution Engine requests

```
Employee 101
```

The Storage Engine

```
Search Index

↓

Locate Data Page

↓

Read Row

↓

Return Row
```

***

### Storage Engine Examples

Different databases use different storage engines.

| Database   | Storage Engine          |
| ---------- | ----------------------- |
| MySQL      | InnoDB (default)        |
| PostgreSQL | Built-in Storage Engine |
| SQL Server | Built-in Storage Engine |
| Oracle     | Oracle Storage Engine   |

Although implementations differ,

their responsibilities are similar.

***

### Key Takeaways

* The Storage Engine stores and retrieves physical data.
* It interacts with disk, memory, and indexes.
* The Execution Engine requests data from the Storage Engine.
* The Storage Engine never decides the execution strategy.

## 20.8 Complete Query Execution Flow

Now let's combine everything we've learned.

Suppose we execute

```sql
SELECT employee_name
FROM employee
WHERE salary > 50000;
```

The complete execution flow is

```
User Writes Query

↓

Parser

(Check SQL Syntax)

↓

Semantic Analyzer

(Check Tables, Columns, Permissions)

↓

Query Optimizer

(Create Best Execution Plan)

↓

Execution Engine

(Execute Plan)

↓

Storage Engine

(Read Data)

↓

Execution Engine

(Process Data)

↓

Return Result
```

Every SQL query follows this sequence.

If an error occurs,

execution stops immediately.

***

### Summary of Responsibilities

| Component         | Responsibility                  |
| ----------------- | ------------------------------- |
| Parser            | Checks syntax                   |
| Semantic Analyzer | Validates database objects      |
| Query Optimizer   | Chooses the best execution plan |
| Execution Engine  | Executes the plan               |
| Storage Engine    | Retrieves physical data         |

## 20.9 End-to-End Example

Let's trace a SQL query through every stage.

***

### Query

```sql
SELECT employee_name
FROM employee
WHERE salary > 50000;
```

***

### Step 1 — Parser

Checks

* Is `SELECT` spelled correctly?
* Is the SQL syntax valid?
* Are parentheses balanced?

Result

```
Syntax Valid

↓

Continue
```

***

### Step 2 — Semantic Analyzer

Checks

* Does the `employee` table exist?
* Does the `salary` column exist?
* Does the user have permission?

Result

```
Objects Valid

↓

Continue
```

***

### Step 3 — Query Optimizer

Possible Plans

```
Plan A

↓

Full Table Scan

Cost = 180
```

```
Plan B

↓

Salary Index

Cost = 25
```

Optimizer selects

```
Plan B
```

***

### Step 4 — Execution Engine

The Execution Engine follows the selected plan.

```
Use Salary Index

↓

Retrieve Matching Rows

↓

Fetch Employee Names
```

***

### Step 5 — Storage Engine

Reads the required index pages

↓

Reads the required data pages

↓

Returns matching rows

***

### Final Output

| Employee Name |
| ------------- |
| John          |
| Meena         |
| Priya         |

***

## Complete Journey

```
Write SQL

↓

Parser

↓

Semantic Analyzer

↓

Query Optimizer

↓

Execution Plan

↓

Execution Engine

↓

Storage Engine

↓

Retrieve Data

↓

Return Result
```

***

## Chapter Summary

In this chapter, you learned the complete lifecycle of SQL query execution.

You now understand:

* How a SQL query moves through the database.
* The role of the Parser.
* The purpose of the Semantic Analyzer.
* How the Query Optimizer chooses the best execution plan.
* How the Execution Engine executes that plan.
* How the Storage Engine retrieves physical data.

This knowledge forms the foundation for the next chapter.

***

## Next Chapter

### Chapter 21 — EXPLAIN & EXPLAIN ANALYZE

Now that you understand **how SQL executes a query**, the next step is learning how to **see the execution plan**.

Using:

* `EXPLAIN`
* `EXPLAIN ANALYZE`

you'll inspect the Query Optimizer's decisions, understand why a query is fast or slow, and identify opportunities for optimization using indexes and better query design.

## Chapter 21 — EXPLAIN & EXPLAIN ANALYZE

***

## What is EXPLAIN?

**EXPLAIN** is a SQL command that displays the **execution plan** chosen by the **Query Optimizer** for a SQL query **before the query is executed**.

Instead of returning the actual data, `EXPLAIN` shows **how the database intends to execute the query**. It provides information such as the access method (Table Scan or Index Scan), join order, indexes used, estimated rows to be processed, and the estimated cost of execution.

In simple terms, **EXPLAIN acts as a diagnostic tool** that helps developers understand how the database plans to execute a query and identify potential performance bottlenecks.

***

### Professional Definition

> **EXPLAIN is a SQL statement used to display the execution plan generated by the Query Optimizer. It provides an estimated roadmap of how the database will execute a query, including the access methods, join operations, index usage, estimated row counts, and execution cost, without returning the actual query results.**

***

### Easy-to-Understand Definition

Think of **EXPLAIN** as **Google Maps for a SQL query**.

Before you start driving, Google Maps calculates the best route to your destination.

Similarly, before executing a SQL query, the database calculates the most efficient way to retrieve the required data.

`EXPLAIN` simply shows you that planned route.

***

### Key Characteristics of EXPLAIN

* Displays the **execution plan** of a query.
* Shows the **estimated** execution strategy chosen by the Query Optimizer.
* Helps identify performance issues before running a query.
* Displays information such as table access methods, indexes, join order, estimated rows, and execution cost.
* Does **not** return the actual query result set.

***

### Example

```sql
EXPLAIN
SELECT *
FROM employees
WHERE department = 'IT';
```

Instead of returning employee records, the database returns information describing **how it plans to execute the query**, allowing developers to analyze and optimize performance.

```sql
%%sql
EXPLAIN
SELECT *
FROM employees
WHERE department = 'IT';
```

## Understanding an EXPLAIN Output (With WHERE Clause)

Let's analyze the following execution plan.

### Query

```sql
EXPLAIN
SELECT *
FROM employees
WHERE department = 'IT';
```

This query asks MySQL to display **how it plans to execute** the query.

***

## EXPLAIN Output

| id | select\_type | table     | partitions | type | possible\_keys | key  | key\_len | ref  | rows | filtered | Extra       |
| -- | ------------ | --------- | ---------- | ---- | -------------- | ---- | -------- | ---- | ---: | -------: | ----------- |
| 1  | SIMPLE       | employees | None       | ALL  | None           | None | None     | None |   10 |     10.0 | Using where |

Let's understand each column in simple terms.

***

## 1. id

```
id = 1
```

**Meaning:**

The query contains only one `SELECT` statement.

***

## 2. select\_type

```
SIMPLE
```

**Meaning:**

This is a simple query.

* No Subquery
* No UNION
* No Complex Query

***

## 3. table

```
employees
```

**Meaning:**

MySQL is reading data from the **employees** table.

***

## 4. partitions

```
None
```

**Meaning:**

The table is not partitioned.

***

## 5. type

```
ALL
```

**Meaning:**

MySQL performs a **Full Table Scan**.

It reads every row in the table.

```
Employee 1

↓

Employee 2

↓

Employee 3

↓

...

↓

Employee 10
```

Since there is **no index** on the `department` column, MySQL has no faster way to find IT employees.

***

## 6. possible\_keys

```
None
```

**Meaning:**

There are **no indexes available** that can help this query.

***

## 7. key

```
None
```

**Meaning:**

No index was used.

***

## 8. key\_len

```
None
```

**Meaning:**

Since no index is used, the key length is also `None`.

***

## 9. ref

```
None
```

**Meaning:**

No indexed column is used for comparison.

***

## 10. rows

```
10
```

**Meaning:**

MySQL estimates it needs to read **10 rows**.

```
Read 10 Rows

↓

Check Each Row

↓

Find Matching Rows
```

***

## 11. filtered

```
10.0
```

**Meaning:**

After reading all rows, MySQL estimates that only about **10%** of them satisfy the condition.

```
Read 10 Rows

↓

Apply WHERE department = 'IT'

↓

Return Matching Rows
```

> The `filtered` value is an **estimate** made by the Query Optimizer.

***

## 12. Extra

```
Using where
```

**Meaning:**

MySQL applies the `WHERE` condition while processing the rows.

```
Read Row

↓

Check

department = 'IT' ?

↓

Yes → Return Row

No → Skip Row
```

***

## Think Like the Query Optimizer

When MySQL receives

```sql
SELECT *
FROM employees
WHERE department = 'IT';
```

it thinks like this:

```
Read Employees Table

↓

No Index Available

↓

Scan All Rows

↓

Apply WHERE Condition

↓

Return IT Employees
```

***

## Complete Execution Flow

```
Write SQL Query

↓

Parser

↓

Semantic Analyzer

↓

Query Optimizer

↓

Choose Full Table Scan

↓

Read All 10 Rows

↓

Apply WHERE Filter

↓

Return Matching Rows
```

***

## Summary

From this execution plan, we can conclude that:

* The query is a **simple SELECT** statement.
* Data is read from the **employees** table.
* The table is **not partitioned**.
* MySQL performs a **Full Table Scan** (`type = ALL`).
* No indexes are available (`possible_keys = None`).
* No index is used (`key = None`).
* MySQL reads approximately **10 rows**.
* The `WHERE department = 'IT'` condition is applied while processing the rows (`Using where`).
* Only the rows matching the `WHERE` condition are returned.

> **Optimization Tip:** If the `employees` table becomes very large and you frequently search by the `department` column, creating an index on `department` can improve query performance by allowing MySQL to locate matching rows without scanning the entire table.

## What is EXPLAIN ANALYZE?

**EXPLAIN ANALYZE** is a SQL command that **executes a query and displays the actual execution plan along with real runtime statistics**.

Unlike `EXPLAIN`, which only shows the Query Optimizer's **estimated execution plan**, `EXPLAIN ANALYZE` runs the query and reports **how the query actually performed**.

It provides valuable information such as the actual execution time, actual number of rows processed, number of loops, and the execution plan used during query execution.

For database administrators and developers, `EXPLAIN ANALYZE` is one of the most powerful tools for identifying performance bottlenecks and verifying whether the Query Optimizer's estimates are accurate.

***

## Professional Definition

> **EXPLAIN ANALYZE is a SQL statement that executes a query and displays the actual execution plan together with runtime statistics such as execution time, actual rows processed, loops, and the operations performed by the database during query execution.**

***

## Easy-to-Understand Definition

Think of **EXPLAIN ANALYZE** as a **fitness tracker**.

Imagine you're planning to run 5 kilometers.

Before running, Google Maps estimates:

```
Distance : 5 km

Estimated Time : 30 Minutes
```

After completing the run, your fitness tracker shows:

```
Distance : 5 km

Actual Time : 34 Minutes

Calories Burned : 420

Average Speed : 8.8 km/h
```

Google Maps gave an **estimate**.

The fitness tracker reports **what actually happened**.

Similarly,

* **EXPLAIN** shows the estimated execution plan.
* **EXPLAIN ANALYZE** executes the query and reports the actual performance.

***

## Syntax

```sql
EXPLAIN ANALYZE
SELECT *
FROM employees
WHERE department = 'IT';
```

***

## What Does EXPLAIN ANALYZE Show?

Depending on the database, it may display:

* Actual execution plan
* Actual execution time
* Actual rows processed
* Number of loops
* Table Scan or Index Scan
* Join operations
* Filter operations
* Sort operations

***

## Think Like SQL

```
Write Query

↓

Query Optimizer

↓

Create Execution Plan

↓

Execute Query

↓

Measure Actual Performance

↓

Display Execution Statistics
```

***

## Why Do We Use EXPLAIN ANALYZE?

Developers use `EXPLAIN ANALYZE` to:

* Measure the actual execution time of a query.
* Compare estimated rows with actual rows.
* Verify whether indexes are being used effectively.
* Identify slow operations such as table scans, sorting, or expensive joins.
* Optimize queries using real execution statistics instead of estimates.

***

## Example

```sql
EXPLAIN ANALYZE
SELECT *
FROM employees
WHERE department = 'IT';
```

Instead of only showing the execution plan, the database executes the query and returns runtime information, such as:

* How many rows were actually read.
* How long the query took to execute.
* Which execution operators were used.
* Whether the optimizer's estimates matched the actual execution.

***

## EXPLAIN vs EXPLAIN ANALYZE

| Feature                          | EXPLAIN | EXPLAIN ANALYZE |
| -------------------------------- | ------- | --------------- |
| Shows Execution Plan             | ✅       | ✅               |
| Executes the Query               | ❌       | ✅               |
| Shows Estimated Cost             | ✅       | ✅               |
| Shows Actual Execution Time      | ❌       | ✅               |
| Shows Actual Rows Processed      | ❌       | ✅               |
| Used for Performance Measurement | ❌       | ✅               |

***

## Key Points

* `EXPLAIN` shows **what the database plans to do**.
* `EXPLAIN ANALYZE` shows **what the database actually did**.
* `EXPLAIN ANALYZE` executes the query before displaying the results.
* It is one of the most important tools for SQL performance tuning and query optimization.

***

## Interview Tip

A common interview question is:

**What is the difference between EXPLAIN and EXPLAIN ANALYZE?**

A simple way to remember it is:

* **EXPLAIN = Estimated Execution Plan**
* **EXPLAIN ANALYZE = Actual Execution Plan + Runtime Statistics**

```sql
%%sql
EXPLAIN ANALYZE
SELECT *
FROM employees
WHERE department = 'IT';
```

## Understanding an EXPLAIN ANALYZE Output

Let's analyze the following `EXPLAIN ANALYZE` output step by step.

### Query

```sql
EXPLAIN ANALYZE
SELECT *
FROM employees
WHERE department = 'IT';
```

Unlike `EXPLAIN`, **EXPLAIN ANALYZE actually executes the query** and shows the **actual execution statistics**.

***

## Output

```
-> Filter: (employees.department = 'IT')
   (cost=1.25 rows=1)
   (actual time=0.0755..0.0819 rows=3 loops=1)

    -> Table scan on employees
       (cost=1.25 rows=10)
       (actual time=0.0669..0.0724 rows=10 loops=1)
```

Let's understand every part of the output.

***

## Execution Flow

```
Table Scan

↓

Read All Employee Records

↓

Apply WHERE department = 'IT'

↓

Return Matching Rows
```

The execution starts from the bottom and moves upward.

***

## Step 1 — Table Scan

```
Table scan on employees
```

### Meaning

MySQL scans the **entire employees table**.

Since there is **no index** on the `department` column, MySQL has to check every row.

```
Employee 1

↓

Employee 2

↓

Employee 3

↓

...

↓

Employee 10
```

***

## Cost

```
cost = 1.25
```

### Meaning

This is the **estimated cost** calculated by the Query Optimizer.

The cost is **not measured in seconds**.

It is simply a number used internally by MySQL to compare different execution plans.

A lower cost generally means a more efficient plan.

***

## Estimated Rows

```
rows = 10
```

### Meaning

Before executing the query,

the optimizer estimated that it would need to read

```
10 rows
```

This estimate is correct because the table contains 10 rows.

***

## Actual Time

```
actual time = 0.0669 .. 0.0724
```

### Meaning

This shows the **real execution time**.

```
0.0669 ms

↓

Started Reading Rows

↓

0.0724 ms

↓

Finished Reading Rows
```

The query completed very quickly because the table is small.

***

## Actual Rows

```
rows = 10
```

### Meaning

During execution,

MySQL actually read

```
10 rows
```

This matches the optimizer's estimate.

***

## Loops

```
loops = 1
```

### Meaning

The table was scanned only **once**.

```
Read Table

↓

Completed

↓

No Repeated Scan
```

***

## Step 2 — Filter

```
Filter:
employees.department = 'IT'
```

### Meaning

After reading all rows,

MySQL applies the

```sql
WHERE department = 'IT'
```

condition.

```
Read Employee

↓

Department = IT ?

↓

Yes → Return

↓

No → Skip
```

***

## Estimated Rows

```
rows = 1
```

### Meaning

Before execution,

the optimizer estimated that

only **1 row** would match.

This is only an estimate.

***

## Actual Rows

```
rows = 3
```

### Meaning

After executing the query,

MySQL found

```
3 Employees
```

belonging to the IT department.

This shows that the optimizer's estimate was different from reality.

***

## Actual Time

```
0.0755 .. 0.0819
```

### Meaning

This is the actual time taken to apply the filter and return the matching rows.

***

## Loops

```
loops = 1
```

### Meaning

The filter operation was performed only once.

***

## Think Like the Query Optimizer

When MySQL receives

```sql
SELECT *
FROM employees
WHERE department = 'IT';
```

it thinks like this:

```
No Index Available

↓

Read Entire Employees Table

↓

Read 10 Rows

↓

Apply WHERE department = 'IT'

↓

Return 3 Matching Rows
```

***

## Estimated vs Actual

| Operation     | Estimated | Actual |
| ------------- | --------: | -----: |
| Rows Read     |        10 |     10 |
| Rows Returned |         1 |      3 |
| Table Scan    |       Yes |    Yes |
| Loops         |         1 |      1 |

Notice that the optimizer estimated **1 matching row**, but the query actually returned **3 rows**.

This difference is completely normal because the optimizer works with **statistics**, not exact values.

***

## Why Did MySQL Use a Table Scan?

The optimizer chose a **Table Scan** because:

* There is **no index** on the `department` column.
* Every row must be checked to find employees in the IT department.

If an index existed on `department`, the execution plan might change to an **Index Scan** or **Index Lookup**, reducing the number of rows that need to be read.

***

## Complete Execution Flow

```
Write SQL Query

↓

Parser

↓

Semantic Analyzer

↓

Query Optimizer

↓

Choose Table Scan

↓

Execution Engine

↓

Read 10 Rows

↓

Apply WHERE Filter

↓

Return 3 Rows
```

***

## Summary

From this execution plan, we can conclude that:

* The query **actually executed** because `EXPLAIN ANALYZE` was used.
* MySQL performed a **Full Table Scan** on the `employees` table.
* Approximately **10 rows** were read from the table.
* The `WHERE department = 'IT'` condition was applied after reading the rows.
* The optimizer estimated that **1 row** would match, but **3 rows** actually matched.
* The table was scanned only **once** (`loops = 1`).
* The actual execution time was very small because the table contains only a few rows.

> **Optimization Tip:** If the `employees` table grows large and queries frequently filter by the `department` column, creating an index on `department` can significantly reduce the number of rows MySQL needs to scan, improving query performance.

## Chapter 22 — Table Scan vs Index Scan

### Introduction

Imagine you're looking for one specific word—say, **"Photosynthesis"**—inside a **900-page textbook**.

You have two options.

#### Option A

Start from **Page 1** and read every page until you find the word.

```
Page 1

↓

Page 2

↓

Page 3

↓

...

↓

Page 900
```

#### Option B

Open the **Index** at the back of the book.

Find **Photosynthesis**.

It tells you:

```
Photosynthesis → Page 214
```

You immediately turn to Page 214.

```
Index

↓

Page 214

↓

Found!
```

Both methods will eventually find the correct page.

However, if the index is available, **no one would choose to read all 900 pages**.

The same idea applies to databases.

Whenever SQL needs to find data, it has two common choices:

* Read every row (**Table Scan**)
* Use an index to jump directly to the required rows (**Index Scan**)

Choosing the correct method can improve query performance by hundreds or even thousands of times.

***

## Why Do We Need This?

In the previous chapter, we learned how to read execution plans using **EXPLAIN**.

One of the most important things you will see in an execution plan is **how the database accesses the table**.

For example,

```sql
EXPLAIN
SELECT *
FROM employees
WHERE department = 'IT';
```

The execution plan may show:

```
type = ALL
```

or

```
type = ref
```

What do these values mean?

* Why did SQL scan the entire table?
* Why didn't it use an index?
* When does SQL choose an Index Scan?
* Can SQL ignore an available index?

This chapter answers all of these questions.

***

## Definition

### Professional Definition

A **Table Scan** (also called a **Full Table Scan**) is a data access method in which the database reads **every row** of a table to evaluate the query conditions.

An **Index Scan** is a data access method in which the database reads an **index** to locate only the required rows instead of scanning the entire table.

***

### Beginner Definition

* **Table Scan** means **check every row one by one**.
* **Index Scan** means **use an index to jump directly to the required rows**.

***

### One-Line Definition

> **Table Scan = Read Everything**\
> **Index Scan = Read Only What You Need**

***

## Visual Explanation

### Table Scan (Full Table Scan)

Suppose the `employees` table contains 10 rows.

```
employees

↓

Row 1  → Check department

↓

Row 2  → Check department

↓

Row 3  → Check department

↓

...

↓

Row 10 → Check department
```

Every row is read.

No shortcuts are used.

***

### Index Scan

Suppose an index exists on the `department` column.

```
Department Index

Finance

HR

IT  ← Jump Directly Here

Sales
```

SQL immediately locates **IT** inside the index.

```
Department Index

↓

IT

↓

Matching Rows

↓

Return Result
```

Only the required rows are read.

***

## Think Like SQL

Whenever SQL receives a query containing a `WHERE` condition, the Query Optimizer thinks like this:

```
Receive Query

↓

Is there an index on the filtered column?

        │
   ┌────┴────┐
   │         │
  No        Yes
   │         │
   ▼         ▼

Table Scan   How many rows will match?

             │
      ┌──────┴──────┐
      │             │

 Few Rows      Most Rows

      │             │
      ▼             ▼

 Index Scan    Table Scan
```

The optimizer always chooses the option with the **lowest estimated cost**.

> **Important**
>
> Having an index **does not guarantee** that SQL will use it.

***

## Syntax

There is **no special SQL syntax** to request a Table Scan or an Index Scan.

You simply write a normal SQL query.

The Query Optimizer automatically decides the best access method.

Example:

```sql
EXPLAIN
SELECT emp_name, salary
FROM employees
WHERE department = 'IT';
```

After execution,

check the **type** column in the execution plan.

For example,

```
type = ALL
```

means

```
Full Table Scan
```

whereas

```
type = ref
```

indicates that MySQL used an index lookup on a non-unique index.

***

## Example 1 — Full Table Scan

Suppose there is **no index** on `department`.

```sql
EXPLAIN
SELECT emp_name
FROM employees
WHERE department = 'IT';
```

Output

| type | possible\_keys | key  | rows | Extra       |
| ---- | -------------- | ---- | ---: | ----------- |
| ALL  | NULL           | NULL |   10 | Using where |

#### Explanation

* `type = ALL` means MySQL scans the entire table.
* `possible_keys = NULL` means no suitable index exists.
* `key = NULL` means no index is used.
* Every row is checked before returning the matching employees.

***

## Example 2 — Index Scan

Create an index.

```sql
CREATE INDEX idx_department
ON employees(department);
```

Now execute the same query.

```sql
EXPLAIN
SELECT emp_name
FROM employees
WHERE department = 'Finance';
```

Output

| type | possible\_keys  | key             | rows | Extra                 |
| ---- | --------------- | --------------- | ---: | --------------------- |
| ref  | idx\_department | idx\_department |    2 | Using index condition |

#### Explanation

* `type = ref` means MySQL uses the index.
* SQL jumps directly to the **Finance** entries.
* Only two rows are read instead of scanning the entire table.

This is much faster for large tables.

***

## Example 3 — Index Exists but SQL Doesn't Use It

Suppose **80% of employees belong to IT**.

```sql
EXPLAIN
SELECT emp_name
FROM employees
WHERE department = 'IT';
```

Output

| type | possible\_keys  | key  | rows | Extra       |
| ---- | --------------- | ---- | ---: | ----------- |
| ALL  | idx\_department | NULL |   10 | Using where |

Notice something interesting.

The index **exists**.

```
possible_keys = idx_department
```

But SQL still chooses

```
type = ALL
```

Why?

Because most of the table matches the condition.

Using the index would require reading almost every row anyway.

Reading the table sequentially is actually cheaper.

> **Remember**
>
> An index is **an option**, not a command.
>
> The Query Optimizer decides whether to use it.

***

## Table Scan vs Index Scan

| Feature    | Table Scan                | Index Scan                       |
| ---------- | ------------------------- | -------------------------------- |
| Reads      | Entire Table              | Matching Rows                    |
| Uses Index | ❌ No                      | ✅ Yes                            |
| Rows Read  | All Rows                  | Required Rows Only               |
| Speed      | Slower on Large Tables    | Faster for Selective Queries     |
| Best For   | Small tables or most rows | Searching a small number of rows |

***

## Business Examples

#### E-Commerce

Finding **one customer's orders** from a table containing millions of orders.

✅ Index Scan

***

#### Banking

Finding **one account** using an account number.

✅ Index Scan

***

#### HR Report

Generating a report for **every employee** in the company.

✅ Table Scan

***

## Interview Tip

One of the most common SQL interview questions is:

> **If an index exists, will MySQL always use it?**

**Answer:**

No.

The Query Optimizer always chooses the execution plan with the **lowest estimated cost**.

Sometimes, reading the entire table is actually faster than using the index.

***

## Key Takeaways

* A **Table Scan** reads every row in a table.
* An **Index Scan** uses an index to locate matching rows.
* The Query Optimizer automatically chooses the cheaper access method.
* Having an index does **not** guarantee it will be used.
* Use `EXPLAIN` to see whether SQL performs a Table Scan or an Index Scan.

## Chapter 23 — Indexes

### Introduction

Think back to the 900-page textbook from Chapter 22. We established that flipping to the index at the back of the book and jumping straight to "page 214" is much faster than reading from page 1 onward.

But here's a question we skipped over: how does the index at the back of the book actually work? It isn't a magic teleporter. Someone had to sit down, read the entire book once, and carefully write down — in alphabetical order — every important word and exactly which page it appears on. That work happens once, up front, so that every future reader benefits from instant lookups forever after.

This chapter is about that back-of-the-book structure itself: what a database index actually is, how it's built, how you create and remove one, and — just as importantly — when adding one can actually make things worse.

### Why Do We Need This?

Chapter 22 told you that index scans are often dramatically faster than full table scans. But an index scan is only possible if an index already exists. This chapter answers the natural next questions: What exactly is an index? How do I create one? And how do I know it's actually helping?

**What problem do indexes solve?**

Without an index, every single query that filters, sorts, or joins on a column forces the database into Chapter 22's full-scan strategy — no matter how selective the condition is, no matter how large the table grows. Indexes solve this by pre-organizing data so the database can jump directly to what it needs.

**What happens if indexes did not exist at all?**

Every lookup, on every table, at any size, would degrade to "read everything and check." A banking app looking up one account among 50 million would take the same time as scanning all 50 million rows, every single time. Modern applications — search boxes, login screens, order lookups — would simply not be usable at real-world data volumes. Indexes are, quite literally, one of the reasons large-scale databases are practical at all.

> **Real business example:** an e-commerce login page checks `WHERE email = 'user@example.com'` against a `users` table. Without an index on `email`, this single check — performed on every login attempt, for every user, all day — would force a full scan of the entire users table every time. With an index, it's a near-instant, direct lookup.

### Definition

#### Professional / Interview Definition

An index is an auxiliary, ordered data structure — most commonly a B-Tree — maintained by the database alongside a table, that maps values of one or more columns to the physical locations of the rows containing them, enabling the query optimizer to locate matching rows without scanning the entire table.

#### Easy Beginner Definition

An index is a separate, organized "lookup list" the database keeps next to your table, so it can find specific rows quickly instead of checking every row one by one.

#### One-Line Definition

An index is a shortcut structure that trades a small amount of extra storage and write effort for much faster reads.

#### The Difference Between These Three Definitions

| Definition               | Best used when…                       | What it emphasizes                                                           |
| ------------------------ | ------------------------------------- | ---------------------------------------------------------------------------- |
| Professional / Interview | Interviews, system design discussions | The precise mechanism — an ordered structure mapping values to row locations |
| Easy Beginner            | First learning the concept            | The everyday "lookup list" intuition                                         |
| One-Line                 | Fast recall                           | The core trade-off: storage/write cost in exchange for read speed            |

### Real-Life Analogy

Think of a phone contact list. If your contacts were stored in the exact random order you added them over the years, finding "Grandma" would mean scrolling through hundreds of names one at a time. Instead, your phone keeps them alphabetically sorted — an index by name — so you can jump straight to "G" and find her in an instant.

Now imagine you also frequently search contacts by birthday to remember who to call this week. Alphabetical order by name doesn't help with that search at all — you'd need a second, separate sorted list, organized by birthday instead. This is exactly why databases let you create multiple indexes on the same table: one sorted structure per way you commonly search.

### Visual Explanation — B-Tree Fundamentals

The most common index structure in nearly every relational database is called a **B-Tree** (short for "Balanced Tree"). Here is a simplified B-Tree built on the `department` column of `employees`:

```
                         ┌──────────────────┐
                         │ "HR" | "Sales"   │      ← ROOT NODE
                         └─────┬──┬──────┬──┘        (a few signpost values)
                     ┌─────────┘  │      └─────────┐
                     ▼            ▼                ▼
              ┌────────────┐ ┌────────────┐  ┌───────────────┐
              │ Finance    │ │     HR     │  │  IT | Sales   │   ← LEAF NODES
              │ → rows 9,10│ │ → rows 4,5 │  │ → rows 1,2,3  │     (actual sorted
              └────────────┘ └────────────┘  │   6,7,8       │      values + row
                                             └───────────────┘      locations)
```

**How a lookup works:** to find `department = 'IT'`, the engine starts at the root, compares "IT" against the signpost values ("HR", "Sales"), and — because "IT" sorts between "HR" and "Sales" alphabetically — follows the correct branch down to the leaf node containing IT's rows. In a real B-Tree with millions of rows, this tree might be 3–4 levels deep, meaning even a table of a billion rows can be searched in just 3–4 comparisons, instead of a billion.

**Why "balanced" matters:** a B-Tree automatically keeps itself balanced as data is added or removed, so every lookup takes a similarly small, predictable number of steps — it never degrades into a long, slow chain, even after years of inserts, updates, and deletes.

### Think Like SQL

When you run `CREATE INDEX`, here is the database's internal monologue:

```
Receive CREATE INDEX command
        ↓
Read every existing row of the table once
        ↓
Sort the specified column's values
        ↓
Build a B-Tree structure mapping
  each sorted value → its row's physical location
        ↓
Store this structure permanently, alongside the table
        ↓
From now on, KEEP THIS STRUCTURE UPDATED
  every time a row is inserted, updated, or deleted
```

> **IMPORTANT** This last line is the single most important fact in this chapter. An index is not a one-time snapshot — it is a living structure the database must maintain forever after creation. Every `INSERT`, `UPDATE`, or `DELETE` on an indexed column means the database does extra work to keep the index accurate. This is precisely why indexes are not "free," and why more indexes is not automatically better — a theme we return to later in this chapter.

### Syntax

#### Creating an Index

```sql
-- A single-column index
CREATE INDEX idx_employees_department
ON employees (department);
```

| Fragment                                | Meaning                                                                                                                            |
| --------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| `CREATE INDEX idx_employees_department` | Creates a new index and names it — a clear naming convention (`idx_<table>_<column>`) makes indexes easy to find and manage later. |
| `ON employees (department)`             | Specifies which table and which column(s) the index is built on.                                                                   |

#### Dropping an Index

```sql
DROP INDEX idx_employees_department ON employees;
```

| Fragment                              | Meaning                                                                                                     |
| ------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| `DROP INDEX idx_employees_department` | Removes the named index structure entirely.                                                                 |
| `ON employees`                        | (MySQL syntax) specifies which table the index belongs to, since index names are scoped per-table in MySQL. |

**Dialect Note:** MySQL requires `ON <table>` in `DROP INDEX`, as shown above. PostgreSQL and SQL Server use simply `DROP INDEX index_name;`, since index names are unique database-wide (PostgreSQL) or schema-wide (SQL Server) in those systems.

#### Composite (Multi-Column) Index — Introduction

```sql
-- An index on TWO columns together
CREATE INDEX idx_employees_dept_salary
ON employees (department, salary);
```

| Fragment               | Meaning                                                                                                                                                      |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `(department, salary)` | Creates one combined B-Tree, sorted first by `department`, then by `salary` within each department — like a phone book sorted by last name, then first name. |

This composite index efficiently supports queries filtering on `department` alone, or on `department` AND `salary` together — but not efficiently on `salary` alone, since salary is only sorted within each department, not across the whole table. (Column order in a composite index matters enormously — this is a common interview topic, revisited in Key Takeaways.)

### Execution Flow

**What happens first (at creation time)?** The database reads the entire existing table once, to build the initial sorted B-Tree structure. On a large table, this itself can take real time and briefly lock the table, depending on the database and settings.

**What happens next (during normal use)?** From that point forward, every time a query's `WHERE`, `JOIN`, or `ORDER BY` touches an indexed column, the Optimizer (Chapter 20) has the option to use the index — and, as Chapter 22 taught you, will only do so when it's estimated to be cheaper.

**What happens inside the engine (on every write)?**

```
        INSERT / UPDATE / DELETE on employees
                       │
                       ▼
        Update the actual table row
                       │
                       ▼
        For EACH index that includes the changed column(s):
                       │
                       ▼
        Update that index's B-Tree too
        (re-sort, rebalance if needed)
                       │
                       ▼
              Write is complete
```

This is why write-heavy tables with many indexes pay a real, ongoing cost — every index is one more structure that must stay in sync, on every single write.

### Step-by-Step Examples

#### Example 1 — Measuring Performance Before an Index

**Query (before creating any index)**

```sql
EXPLAIN ANALYZE
SELECT emp_name, salary
FROM employees
WHERE department = 'Finance';
```

**Output — Before**

```
-> Filter: (department = 'Finance')  (cost=1.25 rows=2) (actual time=0.021..0.026 rows=2 loops=1)
    -> Table scan on employees  (cost=1.25 rows=10) (actual time=0.015..0.019 rows=10 loops=1)
```

**Explanation:** all 10 rows are scanned to find the 2 matching Finance employees. On our tiny table this costs almost nothing measurable — but the pattern is what matters: work scales with total table size, not with the 2 rows we actually wanted.

#### Example 2 — Creating the Index

```sql
CREATE INDEX idx_employees_department
ON employees (department);
```

#### Example 3 — Measuring Performance After the Index

**Query (identical query, after the index exists)**

```sql
EXPLAIN ANALYZE
SELECT emp_name, salary
FROM employees
WHERE department = 'Finance';
```

**Output — After**

```
-> Index lookup on employees using idx_employees_department
   (department='Finance')  (cost=0.70 rows=2)  (actual time=0.006..0.008 rows=2 loops=1)
```

**Explanation:** the plan changed shape entirely — instead of a `Table scan` feeding into a `Filter`, we now have a single, direct `Index lookup` step. The estimated cost dropped from 1.25 to 0.70, and — more importantly — the plan no longer touches all 10 rows at all; it goes straight to the 2 matching ones. On our 10-row example, the actual time difference is tiny and hard to notice. On a 2-million-row employees table, this exact same before/after change is the difference between a query taking seconds and one taking milliseconds.

#### Example 4 — Verifying Index Usage With EXPLAIN

```sql
EXPLAIN
SELECT emp_name
FROM employees
WHERE department = 'Finance';
```

| type | possible\_keys             | key                        | rows |
| ---- | -------------------------- | -------------------------- | ---- |
| ref  | idx\_employees\_department | idx\_employees\_department | 2    |

**Explanation:** the `key` column is no longer `NULL` — it explicitly names `idx_employees_department`, confirming, in black and white, that the index you created is the one actually being used. This is the standard way to verify, not just hope, that an index is doing its job (directly applying Chapter 21's skills).

### Business Examples

1. **E-commerce:** An index on `orders.customer_id` turns "show me this customer's order history" from a full scan of millions of orders into an instant lookup — used on every single "My Orders" page view.
2. **Banking:** An index on `accounts.account_number` is what makes ATM and mobile banking balance checks return in milliseconds, even across banks with tens of millions of accounts.
3. **Hospital:** An index on `patients.patient_id` ensures that pulling up one patient's chart during an emergency is instant, regardless of how many total patient records the hospital system has accumulated over decades.
4. **HR:** A composite index on `(department, join_date)` speeds up common HR reports like "everyone hired in Sales after March" without needing two separate indexes.
5. **Finance:** An index on `invoices.invoice_number` (typically also a unique constraint) guarantees that looking up any single invoice is instant, no matter how many years of invoices have accumulated.
6. **Inventory:** An index on `products.sku` supports instant "is this exact item in stock" checks at scale, across warehouses with millions of SKU-location combinations.
7. **Logistics:** An index on `shipments.tracking_number` is what allows a customer-facing tracking page to return results instantly, even though the underlying shipments table may hold billions of historical records.

### Performance Explanation

**Measuring Before and After (the professional workflow):**

```
1. Run EXPLAIN ANALYZE on the slow query        → record actual time, scan type
2. CREATE the candidate index
3. Run EXPLAIN ANALYZE on the SAME query again  → record new actual time, scan type
4. Compare the two results directly
```

This before/after discipline — not guesswork — is how real database engineers validate that an index actually helped, rather than assuming it did.

**Clustered vs. Non-Clustered Index (introduction):**

| Aspect              | Clustered Index                                                 | Non-Clustered Index                                                                                           |
| ------------------- | --------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| What it controls    | The physical on-disk order of the table's rows themselves       | A separate structure, pointing back to wherever the rows physically live                                      |
| How many per table? | At most one (the table can only be physically sorted one way)   | Many — you can have several non-clustered indexes on the same table                                           |
| Typical example     | Often built automatically on the `PRIMARY KEY`                  | Any additional `CREATE INDEX` you add yourself                                                                |
| Lookup speed        | Often marginally faster, since matching rows are found directly | Slightly slower — an extra step ("looks up" the actual row after finding it in the index) is sometimes needed |

_(This is an introduction only — the full depth of clustered index design is a topic for a dedicated advanced module.)_

### When Do Indexes Slow Down Queries?

This is the most commonly misunderstood part of indexing. Indexes are not free — and in specific situations, they actively hurt performance:

1. **On every write.** As shown in the Execution Flow section, every `INSERT`/`UPDATE`/`DELETE` must also update every relevant index — more indexes means more work per write, which can measurably slow down write-heavy tables.
2. **On low-selectivity columns.** As Chapter 22 established, an index on a column where most rows share the same value (e.g., a `status` column that's 95% `'ACTIVE'`) is often simply ignored by the Optimizer anyway — but it still costs storage and write overhead to maintain, with little to no read benefit.
3. **When there are simply too many indexes.** Every additional index is more disk space, more memory pressure (indexes need to be cached too), and more maintenance overhead — "index everything" is not a valid strategy.
4. **On very small tables.** As seen throughout this module, on a 10-row table, an index typically provides no measurable benefit at all, since a full scan is already essentially instant.

### Common Mistakes

1. **"Adding more indexes always makes things faster."**
   * _Why it happens:_ since this chapter's whole message is "indexes make reads faster," it's easy to overgeneralize.
   * _How to avoid it:_ remember that every index has a write-time cost and a storage cost — always weigh the read benefit against these, especially on write-heavy tables.
2. **Creating an index and never verifying it's actually used.**
   * _Why it happens:_ creating the index feels like the whole job is done.
   * _How to avoid it:_ always follow up with `EXPLAIN` (Example 4 above) to confirm the `key` column names your new index — don't just assume.
3. **Putting columns in the wrong order in a composite index.**
   * _Why it happens:_ it's not obvious that `(department, salary)` and `(salary, department)` behave completely differently.
   * _How to avoid it:_ remember the phone-book analogy — a composite index is sorted by the first column first; put your most commonly filtered-alone column first.
4. **Indexing a column with very low selectivity (e.g., a boolean `is_active` flag).**
   * _Why it happens:_ it seems like any indexed column should help.
   * _How to avoid it:_ apply Chapter 22's selectivity lesson directly — if most rows share the same value, an index rarely helps and may simply be dead weight.
5. **Forgetting that indexes must be actively maintained and monitored.**
   * _Why it happens:_ once created, an index feels "done forever."
   * _How to avoid it:_ periodically re-check `EXPLAIN` output as data grows and query patterns evolve — an index that helped at 10,000 rows may need reconsideration at 10,000,000.

### Interview Questions

**Q1. What is an index, in your own words?** _Why interviewers ask this:_ a basic but revealing opener — vague answers signal shallow understanding. _Answer:_ An index is a separate, ordered data structure the database maintains alongside a table, mapping column values to the physical location of the rows that contain them, so the engine can find matching rows directly instead of scanning the whole table.

**Q2. Why aren't all columns simply indexed by default?** _Why interviewers ask this:_ tests whether a candidate understands the write-side cost of indexing, not just the read-side benefit. _Answer:_ Because every index must be updated on every `INSERT`, `UPDATE`, and `DELETE` that touches its column, and every index consumes additional storage and memory. Indexing every column would slow down all writes significantly while providing little benefit for low-selectivity or rarely-queried columns.

**Q3. What is a B-Tree, and why is it the standard structure for indexes?** _Why interviewers ask this:_ checks whether the candidate understands the mechanism, not just the vocabulary. _Answer:_ A B-Tree is a self-balancing, sorted tree structure that allows lookups, insertions, and deletions in a small, predictable number of steps, even as the underlying data grows very large — typically just 3–4 levels deep even for tables with millions of rows. Its balanced nature guarantees consistently fast performance over the table's entire lifetime.

**Q4. What is the difference between a clustered and a non-clustered index?** _Why interviewers ask this:_ a very common, practical distinction that separates surface-level from real knowledge. _Answer:_ A clustered index determines the actual physical storage order of the table's rows, and a table can have at most one. A non-clustered index is a separate structure that points back to wherever the rows physically live, and a table can have several.

**Q5. What is a composite index, and why does column order matter?** _Why interviewers ask this:_ a frequent, practical source of real production mistakes. _Answer:_ A composite index is built on two or more columns together, sorted by the first column, then the second within each value of the first, and so on. Column order matters because the index can efficiently support filtering on the first column alone, or the first plus subsequent columns together — but not efficiently on a later column alone, since that column isn't globally sorted.

**Q6. Can adding an index ever make a query slower?** _Why interviewers ask this:_ tests whether a candidate has moved past the oversimplified "indexes are always good" mental model. _Answer:_ An index doesn't directly slow down a `SELECT` that uses it, but it can slow down `INSERT`/`UPDATE`/`DELETE` operations that must maintain it, and an unused index still costs storage and cache memory. In aggregate, too many indexes on a write-heavy table can meaningfully degrade overall system performance.

**Q7. How would you verify that a newly created index is actually being used?** _Why interviewers ask this:_ a hands-on, practical skill question connecting directly to Chapter 21. _Answer:_ Run `EXPLAIN` (or `EXPLAIN ANALYZE`) on the relevant query and check that the plan's access-method column names your index directly — in MySQL, the `key` column should show the index's name rather than `NULL`.

**Q8. When would you choose NOT to create an index, even though a column is frequently filtered on?** _Why interviewers ask this:_ tests judgment, not just mechanical recall of "index frequently-filtered columns." _Answer:_ When the column has low selectivity — most rows share the same value — the Optimizer is unlikely to use the index anyway, so it would add write and storage overhead with little read benefit. I'd also hesitate on a very write-heavy table, where the added maintenance cost per write could outweigh the read benefit.

**Q9. What is the proper before/after workflow for validating that an index actually improved performance?** _Why interviewers ask this:_ checks for a rigorous, evidence-based approach rather than assumption. _Answer:_ Run `EXPLAIN ANALYZE` on the query before creating the index and record the actual execution time and scan type. Create the index. Run the exact same `EXPLAIN ANALYZE` again, and directly compare the actual time, scan type, and rows examined between the two runs.

**Q10. Why might an index that helped performance at a small table size stop helping — or even actively hurt — as the table grows?** _Why interviewers ask this:_ connects indexing to the dynamic, statistics-driven nature of the Optimizer from Chapter 20. _Answer:_ As a table grows, the Optimizer's cost estimates and the actual selectivity of a condition can shift — a column that once had reasonably distinct values might, over time, accumulate many duplicate values, lowering its selectivity and making the Optimizer favor a full scan instead. Conversely, an index that was unnecessary on a small table can become essential once the table crosses a size threshold where full scans are no longer cheap. Because of this, index strategy is not "set once and forget" — it should be revisited as data evolves.

### Comparison Tables

#### Clustered vs. Non-Clustered Index

| Aspect                       | Clustered Index                     | Non-Clustered Index                              |
| ---------------------------- | ----------------------------------- | ------------------------------------------------ |
| Controls physical row order? | Yes                                 | No                                               |
| Maximum per table            | One                                 | Many                                             |
| Typical creation             | Often automatic, on the primary key | Manually created with `CREATE INDEX`             |
| Extra lookup step needed?    | Usually no                          | Sometimes (find in index, then fetch actual row) |

#### Single-Column Index vs. Composite Index

| Aspect                 | Single-Column Index  | Composite Index                                                    |
| ---------------------- | -------------------- | ------------------------------------------------------------------ |
| Built on               | One column           | Two or more columns together                                       |
| Supports filtering on… | That one column      | The first column alone, or the first + subsequent columns together |
| Column order matters?  | Not applicable       | Yes — critically                                                   |
| Example                | `INDEX (department)` | `INDEX (department, salary)`                                       |

#### Before vs. After Adding an Index

| Aspect                | Before                       | After                                     |
| --------------------- | ---------------------------- | ----------------------------------------- |
| EXPLAIN access method | `type: ALL` (full scan)      | `type: ref/range` (index-based)           |
| Rows examined         | Entire table                 | Only matching rows                        |
| Scales with           | Table size                   | Number of matching rows                   |
| Write cost            | Lower (no index to maintain) | Slightly higher (index must stay in sync) |

### Best Practices

* Index columns that are both frequently filtered/joined/sorted on **AND** reasonably selective. Both conditions matter — one without the other rarely pays off.
* Always verify index usage with `EXPLAIN` after creating one — never assume.
* Order composite index columns deliberately: put the column most often used alone, or used in the most queries, first.
* Avoid indexing low-selectivity columns (booleans, status flags with few distinct values) unless combined with other columns in a composite index.
* Periodically audit indexes on write-heavy tables — an index nobody's queries actually use anymore is pure overhead, and should be dropped.
* Re-measure performance before and after any index change, using `EXPLAIN ANALYZE`, rather than assuming the change helped.
* Remember indexes are a trade-off, not a free upgrade: faster reads, in exchange for slightly slower writes and additional storage — make that trade deliberately, not automatically.

### Mental Model

An Index is like the back of a textbook:

```
  Built ONCE (or maintained continuously)
        ↓
  Costs a little extra effort to keep up to date
        ↓
  BUT saves enormous time on every future lookup

  Worth it when:    you look things up often, and specifically
  NOT worth it when: you'd read almost the whole book anyway
```

> **Say it in one breath:** "Pay a little on every write, save a lot on every selective read."

### Key Takeaways

1. An index is a separate, ordered structure — typically a B-Tree — mapping column values to row locations, maintained alongside the table.
2. B-Trees stay balanced automatically, keeping lookups fast and predictable even as a table grows to millions or billions of rows.
3. `CREATE INDEX` builds this structure once, by reading the existing table; from then on, the database keeps it updated on every write.
4. Every `INSERT`, `UPDATE`, or `DELETE` on an indexed column must also update the index — indexes are not free, and this cost is real and ongoing.
5. A clustered index controls the physical storage order of a table and is limited to one per table; non-clustered indexes are separate structures, and a table can have several.
6. A composite index is sorted by its first column, then the next within that, and so on — column order changes which queries it can efficiently support.
7. `DROP INDEX` removes an index structure entirely, syntax varying slightly by database (MySQL requires naming the table).
8. Always verify an index is actually being used with `EXPLAIN` — creating one does not guarantee the Optimizer will choose it (Chapter 22).
9. The proper before/after workflow for validating an index is: measure with `EXPLAIN ANALYZE`, create the index, measure again with the identical query, and compare directly.
10. Indexes can slow queries down indirectly — through write overhead — and provide little benefit on low-selectivity columns, exactly as Chapter 22 predicted.
11. Too many indexes on one table is a real anti-pattern: increased storage, increased memory pressure, and increased per-write cost, often for indexes that are rarely or never used.
12. Index strategy is not "set once" — as data volume and distribution change, previously helpful indexes can become unnecessary, and previously unnecessary ones can become essential.
13. This chapter completes the module's arc: Chapter 20 showed you that SQL plans before executing; Chapter 21 taught you how to observe that plan; Chapter 22 taught you the two core strategies the plan can choose between; and this chapter taught you how to actually build the structure that makes the faster strategy possible in the first place.

```sql
%%sql
-- Here we will be creating an index for the employees table 
-- We will be creating the Indexing for a specific column only in the table 
-- we will be confirming the column by which column will be used more in the where clause

CREATE INDEX idx_employees_department
ON employees (department);
```

```sql
%%sql 
-- now we can analysis the cost

-- For employees we have indexing 
-- so the timing will be 0.0189 - 0.0205 ~ 0.0016

EXPLAIN ANALYZE
SELECT emp_name, salary
FROM employees
WHERE department = 'Finance';
```

```sql
%%sql
-- we are creating anaother table for show the time without index 
CREATE TABLE employees2(
    emp_id      INT PRIMARY KEY,
    emp_name    VARCHAR(50)  NOT NULL,
    department  VARCHAR(30)  NOT NULL,
    city        VARCHAR(30)  NOT NULL,
    salary      DECIMAL(10,2) NOT NULL,
    join_date   DATE          NOT NULL
);
 
INSERT INTO employees2 (emp_id, emp_name, department, city, salary, join_date) VALUES
(1,  'Ram',    'IT',      'Chennai',   30000, '2023-01-10'),
(2,  'Divya',  'IT',      'Bengaluru', 70000, '2021-03-15'),
(3,  'Kumar',  'IT',      'Chennai',   40000, '2022-07-20'),
(4,  'Priya',  'HR',      'Mumbai',    45000, '2022-02-10'),
(5,  'Arun',   'HR',      'Chennai',   55000, '2020-09-05'),
(6,  'John',   'Sales',   'Delhi',     60000, '2021-11-12'),
(7,  'Meena',  'Sales',   'Mumbai',    60000, '2023-05-01'),
(8,  'Ravi',   'Sales',   'Chennai',   35000, '2024-01-15'),
(9,  'Sara',   'Finance', 'Bengaluru', 52000, '2022-10-01'),
(10, 'Vikram', 'Finance', 'Delhi',     48000, '2023-08-19');
```

```sql
%%sql

-- now we can analysis the cost for without indexing
-- Time taken 0.0217 -  0.0234 ~ 0.0017

EXPLAIN ANALYZE
SELECT emp_name, salary
FROM employees
WHERE department = 'Finance';
```
