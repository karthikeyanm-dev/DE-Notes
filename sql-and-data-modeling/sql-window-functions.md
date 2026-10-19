# SQL Window Functions

> Source notebook: [DE SQL Notes/SQL\_Window\_Functions.ipynb](https://github.com/karthikeyanm-dev/DE-Notes/blob/main/DE%20SQL%20Notes/SQL_Window_Functions.ipynb). Notebook outputs are omitted; open the source to run cells.

```sql
%reload_ext sql
%sql mysql+mysqlconnector://root:123456@localhost/test
```

## Window Functions

### What is a Window Function?

A **Window Function** performs calculations across a group of related rows **without collapsing the original rows**.

Unlike `GROUP BY`, a Window Function **preserves every row** and simply adds the calculated result as a new column.

#### Simple Definition

> **A Window Function calculates aggregate or analytical values over a set of rows while keeping every original row in the result.**

***

## Why Were Window Functions Introduced?

Before Window Functions existed, SQL developers had a common problem.

Suppose your manager asks:

> **"Show every employee, their salary, and the average salary of their department."**

Expected Output

| emp\_name | department | salary | dept\_avg\_salary |
| --------- | ---------- | -----: | ----------------: |
| Ram       | IT         |  30000 |          46666.67 |
| Divya     | IT         |  70000 |          46666.67 |
| Kumar     | IT         |  40000 |          46666.67 |
| Priya     | HR         |  45000 |             50000 |
| Arun      | HR         |  55000 |             50000 |

Notice something interesting.

The report contains **two different levels of information** in the same row.

Employee-level data

```
Employee Name

Salary
```

Department-level data

```
Department Average Salary
```

These two granularities are displayed together.

This is exactly the kind of problem Window Functions were designed to solve.

***

## Can GROUP BY Solve This?

Let's first calculate the department average.

```sql
SELECT
    department,
    AVG(salary) AS dept_avg_salary
FROM employee
GROUP BY department;
```

Output

| department | dept\_avg\_salary |
| ---------- | ----------------: |
| IT         |          46666.67 |
| HR         |          50000.00 |
| Sales      |          51666.67 |

The averages are correct.

But where are the employees?

They disappeared.

***

## What Happened?

Before GROUP BY

| emp\_name | department | salary |
| --------- | ---------- | -----: |
| Ram       | IT         |  30000 |
| Divya     | IT         |  70000 |
| Kumar     | IT         |  40000 |

↓

After GROUP BY

| department | avg\_salary |
| ---------- | ----------: |
| IT         |    46666.67 |

Three employee rows became one summary row.

***

## Why?

Because:

### GROUP BY collapses rows.

When SQL groups rows,

the original employee-level information no longer exists in the result.

Everything becomes one summary row.

***

## The Business Problem

Our manager didn't ask for

```
One row per department
```

The manager asked for

```
One row per employee

+

Department Average
```

GROUP BY cannot directly produce this result.

***

## Possible Solutions Before Window Functions

Before Window Functions,

developers used:

* GROUP BY + JOIN
* Correlated Subquery

These approaches work,

but they are longer,

harder to read,

and often less efficient.

***

## Window Function Solution

```sql
SELECT
    emp_name,
    department,
    salary,
    AVG(salary) OVER (
        PARTITION BY department
    ) AS dept_avg_salary
FROM employee;
```

Output

| emp\_name | department | salary | dept\_avg\_salary |
| --------- | ---------- | -----: | ----------------: |
| Ram       | IT         |  30000 |          46666.67 |
| Divya     | IT         |  70000 |          46666.67 |
| Kumar     | IT         |  40000 |          46666.67 |
| Priya     | HR         |  45000 |             50000 |
| Arun      | HR         |  55000 |             50000 |
| John      | Sales      |  60000 |          51666.67 |
| Meena     | Sales      |  60000 |          51666.67 |
| Ravi      | Sales      |  35000 |          51666.67 |

Notice:

Every employee row is still present.

SQL simply adds another column.

No rows disappear.

***

## Verify the IT Department

IT Salaries

```
30000

70000

40000
```

Total

```
140000
```

Average

```
140000 / 3

=

46666.67
```

Every IT employee receives the same department average.

***

## Mental Model

Think of a Window Function like this:

```
Original Rows

↓

Create a Window

↓

Perform Calculation

↓

Attach Result

↓

Return Every Original Row
```

Nothing is removed.

Nothing is grouped into one row.

The calculation is simply attached to each row.

***

## GROUP BY vs Window Function

### GROUP BY

```
Rows

↓

Group

↓

Aggregate

↓

Return One Row Per Group
```

Result

```
Rows are collapsed.
```

***

### Window Function

```
Rows

↓

Create Window

↓

Aggregate

↓

Attach Result

↓

Return Every Row
```

Result

```
Rows are preserved.
```

***

## Comparison

| GROUP BY                                | Window Function                      |
| --------------------------------------- | ------------------------------------ |
| Collapses rows                          | Preserves rows                       |
| One output row per group                | One output row per input row         |
| Detail rows disappear                   | Detail rows remain                   |
| Used for summaries                      | Used for analytics                   |
| Cannot show detail + aggregate together | Can show detail + aggregate together |

***

## Real-World Examples

#### Employee Analytics

Employee Salary

Department Average

Department Maximum Salary

Department Minimum Salary

***

#### Banking

Transaction

Account Balance

Running Balance

***

#### E-commerce

Order

Customer Total Spend

Customer Average Order Value

***

#### Swiggy

Order

Restaurant Average Order Amount

Restaurant Total Revenue

Customer Running Spend

***

## Key Takeaways

* Window Functions perform calculations across related rows.
* They do **not** collapse rows.
* Every original row remains in the output.
* They are designed for analytical queries.
* They solve problems that `GROUP BY` cannot solve cleanly.

***

## Next Topic

### Window Function Syntax

```
Function()

OVER(
    PARTITION BY ...

    ORDER BY ...

)
```

This syntax is the foundation of every Window Function.

Once you understand `OVER()`, `PARTITION BY`, and `ORDER BY`, functions like:

* `ROW_NUMBER()`
* `RANK()`
* `DENSE_RANK()`
* `LAG()`
* `LEAD()`
* Running Total
* Moving Average

become much easier to understand.

## What is a Window?

### Before Learning the Syntax

Forget SQL syntax for a moment.

Imagine SQL is processing your table **one row at a time**.

For **every current row**, SQL looks at a set of related rows.

That set of related rows is called the **Window**.

The Window Function performs its calculation on that set and writes the result back to the current row.

***

## Simple Definition

> A **Window** is the collection of rows that SQL uses to perform a calculation for the current row.

***

## Think Like SQL

Suppose we have this table.

| emp\_name | department | salary |
| --------- | ---------- | -----: |
| Ram       | IT         |  30000 |
| Divya     | IT         |  70000 |
| Kumar     | IT         |  40000 |
| Priya     | HR         |  45000 |
| Arun      | HR         |  55000 |

SQL does **not** calculate everything at once.

Instead, it processes one employee at a time.

***

## Step 1

Current Row

| emp\_name | department | salary |
| --------- | ---------- | -----: |
| Ram       | IT         |  30000 |

SQL asks:

> **Which rows belong to Ram's window?**

The rule is:

```
PARTITION BY department
```

So SQL looks for every employee whose department is **IT**.

***

## Ram's Window

| emp\_name | department | salary |
| --------- | ---------- | -----: |
| Ram       | IT         |  30000 |
| Divya     | IT         |  70000 |
| Kumar     | IT         |  40000 |

Now SQL calculates

```
AVG(salary)

↓

(30000 + 70000 + 40000)

↓

140000

↓

140000 / 3

↓

46666.67
```

Finally SQL writes the answer back to **Ram's row**.

| emp\_name | department | salary | dept\_avg\_salary |
| --------- | ---------- | -----: | ----------------: |
| Ram       | IT         |  30000 |          46666.67 |

Notice

Ram's row is **not replaced**.

The average is simply added as another column.

***

## Step 2

Current Row

| emp\_name | department | salary |
| --------- | ---------- | -----: |
| Divya     | IT         |  70000 |

SQL again asks

> Which rows belong to Divya's window?

Answer

Exactly the same three IT employees.

| emp\_name | department | salary |
| --------- | ---------- | -----: |
| Ram       | IT         |  30000 |
| Divya     | IT         |  70000 |
| Kumar     | IT         |  40000 |

Average

```
46666.67
```

SQL attaches that value to Divya's row.

| emp\_name | department | salary | dept\_avg\_salary |
| --------- | ---------- | -----: | ----------------: |
| Divya     | IT         |  70000 |          46666.67 |

***

## Step 3

Current Row

| emp\_name | department | salary |
| --------- | ---------- | -----: |
| Priya     | HR         |  45000 |

Now SQL asks

> Which rows belong to Priya's window?

Answer

Only HR employees.

| emp\_name | department | salary |
| --------- | ---------- | -----: |
| Priya     | HR         |  45000 |
| Arun      | HR         |  55000 |

Average

```
(45000 + 55000)

↓

100000

↓

100000 / 2

↓

50000
```

SQL writes

```
50000
```

onto Priya's row.

***

## Visual Representation

```
Current Row

        Ram

         │

         ▼

Find Related Rows

Ram

Divya

Kumar

         │

         ▼

Perform Calculation

AVG(salary)

         │

         ▼

Attach Result

Ram 30000 46666.67
```

***

## Important Observation

Notice something.

When SQL moves from

```
Ram
```

to

```
Divya
```

the **Current Row changes**.

But the Window may remain the same.

When SQL moves to

```
Priya
```

the Window changes.

This is because Priya belongs to another department.

***

## What Can a Window Be?

A Window can represent different sets of rows depending on how you define it.

### 1. Entire Table

```sql
OVER()
```

Every row sees **every row**.

```
Employee 1

↓

Entire Table

------------------

Employee 2

↓

Entire Table
```

***

### 2. Partition

```sql
OVER(
PARTITION BY department
)
```

Each employee sees only their department.

```
Ram

↓

IT Employees

--------------------

Priya

↓

HR Employees
```

***

### 3. Ordered Window

```sql
OVER(
ORDER BY hire_date
)
```

Rows are processed in order.

Used for:

* Running Total
* Running Average
* Previous Row
* Next Row

***

### 4. Sliding Window (Frame)

```sql
OVER(
ORDER BY sale_date
ROWS BETWEEN
2 PRECEDING
AND CURRENT ROW
)
```

Instead of seeing the whole partition,

the current row sees only nearby rows.

Example

```
Current Row

↓

Previous 2 Rows

+

Current Row
```

Used for

* Moving Average
* Rolling Sum
* Rolling Maximum

***

## Mental Model

Think of every employee carrying their own window.

```
Ram

↓

Looks through

↓

IT Employees

--------------------

Priya

↓

Looks through

↓

HR Employees

--------------------

John

↓

Looks through

↓

Sales Employees
```

Every employee may have a different Window,

but everyone keeps their own row.

***

## Key Takeaways

* SQL processes one row at a time.
* Every current row has its own Window.
* A Window is simply the set of rows used for calculation.
* The Window changes depending on the `OVER()` clause.
* The calculation is attached to the current row instead of replacing it.

***

## Understanding `OVER()`

### What is `OVER()`?

The `OVER()` clause is what transforms a **normal aggregate function** into a **Window Function**.

Without `OVER()`, SQL performs a normal aggregation.

With `OVER()`, SQL performs the same calculation **without reducing the number of rows**.

***

## Simple Definition

> **`OVER()` tells SQL to perform the calculation as a Window Function instead of a normal aggregate function.**

***

## Why is `OVER()` Important?

Look at these two queries.

### Query 1

```sql
SELECT AVG(salary)
FROM employee;
```

Output

| AVG(salary) |
| ----------: |
|    49375.00 |

Only **one row** is returned.

***

### Query 2

```sql
SELECT
    emp_name,
    department,
    salary,
    AVG(salary) OVER() AS company_avg_salary
FROM employee;
```

Output

| emp\_name | department | salary | company\_avg\_salary |
| --------- | ---------- | -----: | -------------------: |
| Ram       | IT         |  30000 |                49375 |
| Divya     | IT         |  70000 |                49375 |
| Kumar     | IT         |  40000 |                49375 |
| Priya     | HR         |  45000 |                49375 |
| Arun      | HR         |  55000 |                49375 |
| John      | Sales      |  60000 |                49375 |
| Meena     | Sales      |  60000 |                49375 |
| Ravi      | Sales      |  35000 |                49375 |

Notice something amazing.

The average is exactly the same,

but **all employee rows remain**.

That is the power of `OVER()`.

***

## What Does `OVER()` Mean?

Think of it like this.

```
AVG(salary)

↓

Normal Aggregate

↓

Collapse Rows

-------------------------

AVG(salary)

OVER()

↓

Window Function

↓

Keep Every Row
```

***

## The Simplest Window

```sql
AVG(salary) OVER()
```

Nothing is written inside the parentheses.

```
OVER()
```

means

> **Use the entire result set as the Window.**

Every employee sees every employee.

***

## Visual Representation

Current Row

```
Ram
```

Window

```
Ram

Divya

Kumar

Priya

Arun

John

Meena

Ravi
```

Average

```
395000

/

8

=

49375
```

SQL writes

```
49375
```

onto Ram's row.

***

Now SQL moves to

```
Divya
```

Current Row

```
Divya
```

Window

```
Ram

Divya

Kumar

Priya

Arun

John

Meena

Ravi
```

Average

```
49375
```

Again.

The Window never changes because

```
OVER()
```

contains no partition.

***

## Company Average Calculation

```
30000

+

70000

+

40000

+

45000

+

55000

+

60000

+

60000

+

35000

=

395000
```

Average

```
395000

/

8

=

49375
```

Every employee receives

```
49375
```

***

## Adding PARTITION BY

Now suppose we change the query.

```sql
SELECT
    emp_name,
    department,
    salary,
    AVG(salary) OVER(
        PARTITION BY department
    ) AS dept_avg_salary
FROM employee;
```

Now the Window changes.

Instead of looking at

```
Entire Company
```

SQL now looks at

```
Only employees in the same department.
```

***

## Ram's Window

```
Ram

Divya

Kumar
```

Average

```
46666.67
```

***

## Priya's Window

```
Priya

Arun
```

Average

```
50000
```

***

## John's Window

```
John

Meena

Ravi
```

Average

```
51666.67
```

Every employee still keeps their row.

Only the Window changes.

***

## Reading the Syntax

Consider this query.

```sql
AVG(salary)
OVER(
    PARTITION BY department
)
```

Read it like a sentence.

```
AVG(salary)

↓

Calculate the average salary

↓

OVER

↓

As a Window Function

↓

PARTITION BY department

↓

Using employees from the same department
```

If you can read it like English,

you understand Window Functions.

***

## Three Layers of a Window Function

```
AVG(salary)

↓

What to Calculate

-------------------------

OVER()

↓

Perform it as a Window Function

-------------------------

PARTITION BY department

↓

Which rows belong to the Window
```

Every Window Function follows these three layers.

***

## Comparing the Two Queries

### Without OVER()

```sql
SELECT AVG(salary)
FROM employee;
```

Output

```
49375
```

One row.

***

### With OVER()

```sql
SELECT
    emp_name,
    AVG(salary) OVER()
FROM employee;
```

Output

```
Ram      49375

Divya    49375

Kumar    49375

...
```

Eight rows.

***

## Why?

Without

```
OVER()
```

SQL thinks

```
Aggregate Query
```

and collapses rows.

With

```
OVER()
```

SQL thinks

```
Window Function
```

and preserves rows.

That is the only difference,

yet it completely changes the execution model.

***

## Comparison

| AVG(salary)        | AVG(salary) OVER() |
| ------------------ | ------------------ |
| Normal Aggregate   | Window Function    |
| Returns one row    | Returns all rows   |
| Collapses rows     | Preserves rows     |
| Used for summaries | Used for analytics |

***

## Mental Model

```
No OVER()

↓

Aggregate Query

↓

One Result

------------------------

OVER()

↓

Window Function

↓

One Result

For Every Row
```

***

## Key Takeaways

* `OVER()` is the switch that converts an aggregate into a Window Function.
* `OVER()` by itself uses the **entire result set** as the Window.
* `PARTITION BY` divides the Window into groups without collapsing rows.
* Window Functions always preserve the original number of rows.
* The calculation is attached to each row instead of replacing the rows.

***

## Next Topic

Now that you understand `OVER()`, we can study the first real Window Function used in interviews:

### `ROW_NUMBER()`

This is the foundation for:

* Top-N per Group
* Duplicate Removal
* Pagination
* Latest Record per Customer
* Ranking Problems

```sql
%%sql
CREATE TABLE employee (
    emp_id     INT PRIMARY KEY,
    emp_name   VARCHAR(50)  NOT NULL,
    department VARCHAR(30)  NOT NULL,
    salary     DECIMAL(10,2) NOT NULL,
    hire_date  DATE         NOT NULL
);
```

```sql
%%sql
INSERT INTO employee (emp_id, emp_name, department, salary, hire_date) VALUES
(1, 'Karthikeyan',   'IT',    30000, '2023-01-10'),
(2, 'Divya', 'IT',    70000, '2021-03-15'),
(3, 'Kumar', 'IT',    40000, '2022-07-20'),
(4, 'Priya', 'HR',    45000, '2022-02-10'),
(5, 'Arun',  'HR',    55000, '2020-09-05'),
(6, 'John',  'Sales', 60000, '2021-11-12'),
(7, 'Meena', 'Sales', 60000, '2023-05-01'),
(8, 'Ravi',  'Sales', 35000, '2024-01-15');
```

```sql
%%sql
CREATE TABLE sales (
    sale_id     INT PRIMARY KEY,
    customer_id INT           NOT NULL,
    sale_date   DATE          NOT NULL,
    amount      DECIMAL(10,2) NOT NULL
);
```

```sql
%%sql
INSERT INTO sales (sale_id, customer_id, sale_date, amount) VALUES
(1,  101, '2024-01-01', 100),
(2,  101, '2024-01-02', 150),
(3,  101, '2024-01-03', 120),
(4,  101, '2024-01-04', 200),
(5,  101, '2024-01-05', 180),
(6,  102, '2024-01-01', 500),
(7,  102, '2024-01-02', 450),
(8,  102, '2024-01-03', 700),
(9,  102, '2024-01-05', 650),
(10, 102, '2024-01-08', 800),
(11, 103, '2024-01-02', 300),
(12, 103, '2024-01-06', 300);
```

```sql
%%sql
CREATE TABLE customer_history (
    customer_id   INT          NOT NULL,
    customer_name VARCHAR(50)  NOT NULL,
    email         VARCHAR(100) NOT NULL,
    updated_at    DATETIME     NOT NULL
);
```

```sql
%%sql
INSERT INTO customer_history (customer_id, customer_name, email, updated_at) VALUES
(101, 'Anitha',   'anitha@old.com',     '2024-01-01 09:00:00'),
(101, 'Anitha',   'anitha@gmail.com',   '2024-03-01 10:00:00'),
(101, 'Anitha R', 'anitha.r@gmail.com', '2024-05-01 11:30:00'),
(102, 'Suresh',   'suresh@yahoo.com',   '2024-02-10 08:15:00'),
(102, 'Suresh',   'suresh@gmail.com',   '2024-04-12 14:00:00'),
(103, 'Lakshmi',  'lakshmi@gmail.com',  '2024-01-20 16:45:00');
```

#### We have Just create some tables and inserted some datas to experiment Window function

***

```sql
%%sql 
-- Normal Aggregation
select avg(salary)
from employee;
```

```sql
%%sql 
-- Normal Aggregation with group by
select department,avg(salary)
from employee
group by department;
```

***

**We can't able to see the indidual employee details**

***

```sql
%%sql 
-- OVER()
-- in this we got all employees data with the average of all the salary
select emp_name,avg(salary) OVER()
from employee;
```

```sql
%%sql 
-- OVER() 
-- PARTITION BY -> Similar to Group by
-- in this we got all employees data with the average of all the salary

select emp_name,department,
       avg(salary) OVER(
           PARTITION BY department
       ) as dept_average
from employee;
```

```sql
%%sql
-- We can add More aggregates over partition
SELECT emp_name,department,salary,
    AVG(salary) OVER (PARTITION BY department) AS dept_avg,
    SUM(salary) OVER (PARTITION BY department) AS dept_total,
    COUNT(salary) OVER (PARTITION BY department) AS dept_head_count,
    MIN(salary) OVER (PARTITION BY department) AS dept_min_sal,
    MAX(salary) OVER (PARTITION BY department) AS dept_max_sal
FROM employee;
```

## ORDER BY Inside `OVER()`

### Two Completely Different `ORDER BY`s

One of the biggest mistakes beginners make is thinking that every `ORDER BY` does the same thing.

It doesn't.

There are **two completely different ORDER BY clauses in SQL.**

***

## 1. Final Query ORDER BY

```sql
SELECT
    emp_name,
    salary
FROM employee
ORDER BY salary;
```

Output

| emp\_name | salary |
| --------- | -----: |
| Ram       |  30000 |
| Kumar     |  40000 |
| Priya     |  45000 |
| Arun      |  55000 |
| John      |  60000 |
| Meena     |  60000 |
| Divya     |  70000 |

Notice something.

Only the **display order** changed.

Nothing else changed.

No calculations changed.

***

## 2. ORDER BY Inside `OVER()`

```sql
SELECT
    emp_name,
    salary,
    SUM(salary) OVER(
        ORDER BY salary
    ) AS running_total
FROM employee;
```

This ORDER BY is **not for displaying rows.**

It tells SQL

> **In what sequence should I process the rows?**

***

## Simple Definition

> `ORDER BY` inside `OVER()` determines the sequence in which the Window Function performs its calculation.

It is **not** responsible for displaying the result.

***

## Think Like SQL

Suppose salaries are

| Employee | Salary |
| -------- | -----: |
| Ram      |  30000 |
| Kumar    |  40000 |
| Priya    |  45000 |
| Arun     |  55000 |

SQL first sorts them internally.

```
30000

↓

40000

↓

45000

↓

55000
```

Now SQL processes one row at a time.

***

## Current Row 1

Window

```
30000
```

SUM

```
30000
```

Running Total

```
30000
```

***

## Current Row 2

Window

```
30000

40000
```

SUM

```
70000
```

Running Total

```
70000
```

***

## Current Row 3

Window

```
30000

40000

45000
```

SUM

```
115000
```

Running Total

```
115000
```

***

## Current Row 4

Window

```
30000

40000

45000

55000
```

SUM

```
170000
```

Running Total

```
170000
```

***

## Visual Representation

```
Current Row

↓

Window grows

↓

Calculation

↓

Attach Result
```

```
30000

↓

30000

-----------------------

30000

40000

↓

70000

-----------------------

30000

40000

45000

↓

115000

-----------------------

30000

40000

45000

55000

↓

170000
```

Notice something.

The Window becomes larger for every row.

This is why the total keeps increasing.

***

## Real Example

```sql
SELECT
    sale_date,
    amount,
    SUM(amount)
    OVER(
        ORDER BY sale_date
    ) AS running_total
FROM sales
WHERE customer_id = 101;
```

Output

| sale\_date | amount | running\_total |
| ---------- | -----: | -------------: |
| 2024-01-01 |    100 |            100 |
| 2024-01-02 |    150 |            250 |
| 2024-01-03 |    120 |            370 |
| 2024-01-04 |    200 |            570 |
| 2024-01-05 |    180 |            750 |

Verification

```
100

↓

100 + 150 = 250

↓

250 + 120 = 370

↓

370 + 200 = 570

↓

570 + 180 = 750
```

***

## Important Observation

Notice

```sql
ORDER BY
```

inside

```sql
OVER()
```

does **not**

necessarily control the output order.

For example

```sql
SELECT
    emp_name,
    salary,
    SUM(salary)
    OVER(
        ORDER BY salary
    ) AS running_total
FROM employee
ORDER BY emp_name;
```

Now

The Window Function

calculates

```
By Salary
```

But the final report is displayed

```
By Employee Name
```

These are two completely different operations.

***

## Visual Comparison

```
ORDER BY at the End

↓

Display Order

----------------------------

ORDER BY inside OVER()

↓

Calculation Order
```

***

## Comparison

| Final ORDER BY         | ORDER BY Inside OVER()                         |
| ---------------------- | ---------------------------------------------- |
| Controls display order | Controls calculation order                     |
| Does not change values | Changes calculated values                      |
| Executed after SELECT  | Used during Window Function calculation        |
| One per query block    | Each Window Function can have its own ORDER BY |

***

## Mental Model

```
Final ORDER BY

↓

How should I SHOW the rows?

----------------------------

Window ORDER BY

↓

How should I PROCESS the rows?
```

***

## Key Takeaways

* SQL has two different `ORDER BY` clauses.
* Final `ORDER BY` controls the output order.
* `ORDER BY` inside `OVER()` controls the calculation sequence.
* Running totals depend on the calculation order.
* The display order and calculation order can be completely different.

***

## Next Topic

Now that you understand

* `OVER()`
* `PARTITION BY`
* `ORDER BY`

we can finally learn the first real interview Window Function:

### `ROW_NUMBER()`

Everything you've learned so far is the foundation for:

* `ROW_NUMBER()`
* `RANK()`
* `DENSE_RANK()`
* `LAG()`
* `LEAD()`
* Running Totals
* Moving Averages

```sql
%%sql
-- we gona print sales report in order and gona calculte the running total for the sales
-- running total is sumation of the amout of the previous col amount
select 
sale_id,
sale_date,
amount,
sum(amount) OVER (
    ORDER BY amount
) as running_total
from sales;
```

## 7.1 `SUM()` OVER()

### Business Scenario

The finance manager asks:

> **"For every transaction made by a customer, show me how much that customer has spent so far."**

This is called a **Running Total** (or Running Sum).

***

## Sample Sales Table

| sale\_id | customer\_id | sale\_date | amount |
| -------: | -----------: | ---------- | -----: |
|        1 |          101 | 2024-01-01 |    100 |
|        2 |          101 | 2024-01-02 |    150 |
|        3 |          101 | 2024-01-03 |    120 |
|        4 |          101 | 2024-01-04 |    200 |
|        5 |          101 | 2024-01-05 |    180 |

The manager doesn't want only today's amount.

He wants to know

```
How much has the customer spent

up to this transaction?
```

***

## Can GROUP BY Solve This?

Let's try.

```sql
SELECT
    customer_id,
    SUM(amount)
FROM sales
GROUP BY customer_id;
```

Output

| customer\_id | total\_amount |
| ------------ | ------------: |
| 101          |           750 |

This tells us

```
Customer 101 spent 750.
```

Correct.

But...

Where are the individual transactions?

They disappeared.

***

## What We Actually Need

| sale\_date | amount | running\_total |
| ---------- | -----: | -------------: |
| 2024-01-01 |    100 |            100 |
| 2024-01-02 |    150 |            250 |
| 2024-01-03 |    120 |            370 |
| 2024-01-04 |    200 |            570 |
| 2024-01-05 |    180 |            750 |

Every transaction remains.

The running total keeps increasing.

***

## Window Function Solution

```sql
SELECT
    sale_date,
    amount,
    SUM(amount)
    OVER(
        ORDER BY sale_date
    ) AS running_total
FROM sales
WHERE customer_id = 101;
```

***

## How SQL Thinks

Current Row

```
2024-01-01

100
```

Window

```
100
```

SUM

```
100
```

Running Total

```
100
```

***

Current Row

```
2024-01-02

150
```

Window

```
100

150
```

SUM

```
250
```

Running Total

```
250
```

***

Current Row

```
2024-01-03

120
```

Window

```
100

150

120
```

SUM

```
370
```

Running Total

```
370
```

***

Current Row

```
2024-01-04

200
```

Window

```
100

150

120

200
```

SUM

```
570
```

Running Total

```
570
```

***

Current Row

```
2024-01-05

180
```

Window

```
100

150

120

200

180
```

SUM

```
750
```

Running Total

```
750
```

***

## Visual Representation

```
100

↓

100

-----------------------

100

150

↓

250

-----------------------

100

150

120

↓

370

-----------------------

100

150

120

200

↓

570

-----------------------

100

150

120

200

180

↓

750
```

Notice

The Window keeps growing.

Therefore,

the SUM keeps growing.

***

## Multiple Customers

Now suppose the table contains

| customer\_id | sale\_date | amount |
| ------------ | ---------- | -----: |
| 101          | 2024-01-01 |    100 |
| 101          | 2024-01-02 |    150 |
| 102          | 2024-01-01 |    300 |
| 102          | 2024-01-02 |    200 |

If we use

```sql
SUM(amount)
OVER(
    PARTITION BY customer_id
    ORDER BY sale_date
)
```

each customer gets a separate running total.

Customer 101

```
100

↓

250
```

Customer 102

```
300

↓

500
```

The running total restarts for every customer.

***

## Complete Query

```sql
SELECT
    customer_id,
    sale_date,
    amount,
    SUM(amount)
    OVER(
        PARTITION BY customer_id
        ORDER BY sale_date
    ) AS running_total
FROM sales;
```

***

## Execution Flow

```
Partition

↓

Customer 101

↓

Sort by Date

↓

Running SUM

------------------------

Partition

↓

Customer 102

↓

Sort by Date

↓

Running SUM
```

***

## Real-World Uses

#### Banking

Running Account Balance

***

#### Swiggy

Customer Lifetime Spend

***

#### Amazon

Total Amount Purchased

***

#### Finance

Daily Cash Flow

***

#### Inventory

Running Stock Quantity

***

## Interview Questions

#### What happens if PARTITION BY is removed?

The running total continues across the entire table.

***

#### What happens if ORDER BY is removed?

Every row gets the total sum of the partition.

Example

```
750

750

750

750

750
```

instead of

```
100

250

370

570

750
```

***

## Mental Model

```
SUM()

↓

What to Calculate

----------------------

PARTITION BY

↓

Where to Restart

----------------------

ORDER BY

↓

In What Sequence
```

***

## Key Takeaways

* `SUM() OVER()` preserves every row.
* `ORDER BY` creates a running total.
* `PARTITION BY` restarts the running total for each group.
* Without `ORDER BY`, every row receives the same total.
* Running totals are one of the most common Window Function interview questions.

```sql
%%sql
-- Here we have calculated each customers running total
SELECT
    customer_id,
    sale_date,
    amount,
    SUM(amount)
    OVER(
        PARTITION BY customer_id
        ORDER BY sale_date
    ) AS running_total
FROM sales;
```

```sql
%%sql
-- here we have calculate the each customer total
select customer_id, sale_date, amount, 
        sum(amount) over(
            PARTITION BY customer_id
        ) as customer_total
from sales;
```

```sql
%%sql
-- We are merging both the query
SELECT
    customer_id,
    sale_date,
    amount,
    SUM(amount)
    OVER(
        PARTITION BY customer_id
        ORDER BY sale_date
    ) AS running_total,
        sum(amount) over(
            PARTITION BY customer_id
        ) as customer_total
FROM sales;
```

## 7.2 AVG() OVER()

### Business Scenario

The sales manager asks:

> **"For every sale, show the average purchase amount of that customer."**

The manager wants to compare:

* Current Sale Amount
* Customer's Average Purchase Amount

in the same row.

***

## Sample Sales Table

| sale\_id | customer\_id | sale\_date | amount |
| -------: | -----------: | ---------- | -----: |
|        1 |          101 | 2024-01-01 |    100 |
|        2 |          101 | 2024-01-02 |    150 |
|        3 |          101 | 2024-01-03 |    120 |
|        4 |          101 | 2024-01-04 |    200 |
|        5 |          101 | 2024-01-05 |    180 |

***

## Can GROUP BY Solve This?

```sql
SELECT
    customer_id,
    AVG(amount) AS avg_amount
FROM sales
GROUP BY customer_id;
```

Output

| customer\_id | avg\_amount |
| ------------ | ----------: |
| 101          |         150 |

Correct.

But all sales disappeared.

***

## What We Actually Need

| sale\_date | amount | customer\_avg |
| ---------- | -----: | ------------: |
| 2024-01-01 |    100 |           150 |
| 2024-01-02 |    150 |           150 |
| 2024-01-03 |    120 |           150 |
| 2024-01-04 |    200 |           150 |
| 2024-01-05 |    180 |           150 |

Every sale remains.

The average is attached to every row.

***

## Window Function Solution

```sql
SELECT
    customer_id,
    sale_date,
    amount,
    AVG(amount)
    OVER(
        PARTITION BY customer_id
    ) AS customer_avg
FROM sales;
```

***

## How SQL Thinks

Current Row

```
Sale = 100
```

Window

```
100

150

120

200

180
```

Average

```
750

/

5

=

150
```

Attach

```
150
```

to the current row.

***

Current Row

```
Sale = 150
```

Window

```
100

150

120

200

180
```

Average

```
150
```

Again.

Notice

The Window never changes.

Therefore,

the average never changes.

***

## Output

| sale\_date | amount | customer\_avg |
| ---------- | -----: | ------------: |
| 2024-01-01 |    100 |           150 |
| 2024-01-02 |    150 |           150 |
| 2024-01-03 |    120 |           150 |
| 2024-01-04 |    200 |           150 |
| 2024-01-05 |    180 |           150 |

***

## Running Average

Now let's add

```sql
ORDER BY sale_date
```

```sql
SELECT
    customer_id,
    sale_date,
    amount,
    AVG(amount)
    OVER(
        PARTITION BY customer_id
        ORDER BY sale_date
    ) AS running_avg
FROM sales;
```

***

## Output

| sale\_date | amount | running\_avg |
| ---------- | -----: | -----------: |
| 2024-01-01 |    100 |       100.00 |
| 2024-01-02 |    150 |       125.00 |
| 2024-01-03 |    120 |       123.33 |
| 2024-01-04 |    200 |       142.50 |
| 2024-01-05 |    180 |       150.00 |

***

## How SQL Calculates

#### Row 1

Window

```
100
```

Average

```
100
```

***

#### Row 2

Window

```
100

150
```

Average

```
250 / 2

=

125
```

***

#### Row 3

Window

```
100

150

120
```

Average

```
370 / 3

=

123.33
```

***

#### Row 4

Window

```
100

150

120

200
```

Average

```
570 / 4

=

142.50
```

***

#### Row 5

Window

```
100

150

120

200

180
```

Average

```
750 / 5

=

150
```

***

## Visual Representation

```
100

↓

100

------------------

100

150

↓

125

------------------

100

150

120

↓

123.33

------------------

100

150

120

200

↓

142.50

------------------

100

150

120

200

180

↓

150
```

The Window keeps growing,

so the average keeps changing.

***

## Without ORDER BY

```sql
AVG(amount)
OVER(
PARTITION BY customer_id
)
```

Every row gets

```
150
```

***

## With ORDER BY

```sql
AVG(amount)
OVER(
PARTITION BY customer_id
ORDER BY sale_date
)
```

Every row gets

the average

**up to that row**.

***

## Comparison

| Query                                                     | Result                    |
| --------------------------------------------------------- | ------------------------- |
| `AVG() OVER(PARTITION BY customer_id)`                    | Same average on every row |
| `AVG() OVER(PARTITION BY customer_id ORDER BY sale_date)` | Running average           |

***

## Real-World Uses

#### Banking

Average daily balance

***

#### Swiggy

Average order value per customer

***

#### Amazon

Average purchase amount

***

#### Stock Market

Moving average price

***

#### Sales Dashboard

Average revenue trend

***

## Mental Model

```
AVG()

↓

Calculate Average

+

PARTITION BY

↓

Restart Average

+

ORDER BY

↓

Running Average
```

***

## Key Takeaways

* `AVG() OVER()` preserves every row.
* Without `ORDER BY`, each row gets the same group average.
* With `ORDER BY`, SQL calculates a running average.
* `PARTITION BY` restarts the calculation for each group.

```sql
%%sql
-- Customers average
select customer_id,sale_date,amount,
    AVG(amount) OVER(
        PARTITION BY customer_id
    ) as customer_avg
from sales;
```

```sql
%%sql
-- Customers running average
select customer_id,sale_date,amount,
    AVG(amount) OVER(
        PARTITION BY customer_id
        ORDER BY sale_date
    ) as customer_avg
from sales;
```

```sql
%%sql
-- Customers running average and total average
select customer_id,sale_date,amount,
    AVG(amount) OVER(
        PARTITION BY customer_id
        ORDER BY sale_date
    ) as customer_avg,
    AVG(amount) OVER(
        PARTITION BY customer_id
    ) as customer_total_average
from sales;
```

## 7.3 COUNT() OVER()

### Business Scenario

The sales manager asks:

> **"For every sale, show how many orders the customer has placed."**

The report should display:

* Current Order
* Order Amount
* Total Number of Orders by that Customer

***

## Sample Sales Table

| sale\_id | customer\_id | sale\_date | amount |
| -------: | -----------: | ---------- | -----: |
|        1 |          101 | 2024-01-01 |    100 |
|        2 |          101 | 2024-01-02 |    150 |
|        3 |          101 | 2024-01-03 |    120 |
|        4 |          101 | 2024-01-04 |    200 |
|        5 |          101 | 2024-01-05 |    180 |

***

## Can GROUP BY Solve This?

```sql
SELECT
    customer_id,
    COUNT(*) AS total_orders
FROM sales
GROUP BY customer_id;
```

Output

| customer\_id | total\_orders |
| ------------ | ------------: |
| 101          |             5 |

Correct.

But once again,

the individual sales disappear.

***

## What We Actually Need

| sale\_date | amount | total\_orders |
| ---------- | -----: | ------------: |
| 2024-01-01 |    100 |             5 |
| 2024-01-02 |    150 |             5 |
| 2024-01-03 |    120 |             5 |
| 2024-01-04 |    200 |             5 |
| 2024-01-05 |    180 |             5 |

Every sale remains.

The total number of orders is attached to every row.

***

## Window Function Solution

```sql
SELECT
    customer_id,
    sale_date,
    amount,
    COUNT(*)
    OVER(
        PARTITION BY customer_id
    ) AS total_orders
FROM sales;
```

***

## How SQL Thinks

Current Row

```
Sale = 100
```

Window

```
100

150

120

200

180
```

Count

```
5
```

Attach

```
5
```

to the current row.

***

Current Row

```
Sale = 150
```

Window

```
100

150

120

200

180
```

Count

```
5
```

Again.

Since the Window never changes,

the count never changes.

***

## Output

| sale\_date | amount | total\_orders |
| ---------- | -----: | ------------: |
| 2024-01-01 |    100 |             5 |
| 2024-01-02 |    150 |             5 |
| 2024-01-03 |    120 |             5 |
| 2024-01-04 |    200 |             5 |
| 2024-01-05 |    180 |             5 |

***

## Running Count

Now let's add

```sql
ORDER BY sale_date
```

```sql
SELECT
    customer_id,
    sale_date,
    amount,
    COUNT(*)
    OVER(
        PARTITION BY customer_id
        ORDER BY sale_date
    ) AS running_count
FROM sales;
```

***

## Output

| sale\_date | amount | running\_count |
| ---------- | -----: | -------------: |
| 2024-01-01 |    100 |              1 |
| 2024-01-02 |    150 |              2 |
| 2024-01-03 |    120 |              3 |
| 2024-01-04 |    200 |              4 |
| 2024-01-05 |    180 |              5 |

***

## How SQL Calculates

#### Row 1

Window

```
100
```

Count

```
1
```

***

#### Row 2

Window

```
100

150
```

Count

```
2
```

***

#### Row 3

Window

```
100

150

120
```

Count

```
3
```

***

#### Row 4

Window

```
100

150

120

200
```

Count

```
4
```

***

#### Row 5

Window

```
100

150

120

200

180
```

Count

```
5
```

***

## Visual Representation

```
100

↓

Count = 1

------------------

100

150

↓

Count = 2

------------------

100

150

120

↓

Count = 3

------------------

100

150

120

200

↓

Count = 4

------------------

100

150

120

200

180

↓

Count = 5
```

The Window keeps growing,

so the count keeps increasing.

***

## Without ORDER BY

```sql
COUNT(*)
OVER(
PARTITION BY customer_id
)
```

Every row gets

```
5
```

***

## With ORDER BY

```sql
COUNT(*)
OVER(
PARTITION BY customer_id
ORDER BY sale_date
)
```

Every row gets

the count

**up to that row**.

***

## Comparison

| Query                                                        | Result                  |
| ------------------------------------------------------------ | ----------------------- |
| `COUNT(*) OVER(PARTITION BY customer_id)`                    | Same count on every row |
| `COUNT(*) OVER(PARTITION BY customer_id ORDER BY sale_date)` | Running count           |

***

## Real-World Uses

#### Banking

Number of transactions completed.

***

#### Swiggy

Orders placed so far by a customer.

***

#### Amazon

Purchases made by a customer.

***

#### HR

Employees hired so far.

***

#### Analytics

Cumulative event count.

***

## Mental Model

```
COUNT()

↓

Count Rows

+

PARTITION BY

↓

Restart Count

+

ORDER BY

↓

Running Count
```

***

## Key Takeaways

* `COUNT() OVER()` preserves every row.
* Without `ORDER BY`, every row gets the same total count.
* With `ORDER BY`, SQL calculates a running count.
* `PARTITION BY` restarts the count for each group.

```sql
%%sql
-- Total transcation of each customer
SELECT customer_id,sale_date,count(*) over(
    PARTITION BY customer_id
) as total_txn
from sales;
```

```sql
%%sql
-- Running Total of total transaction count
SELECT customer_id,sale_date,count(*) over(
    PARTITION BY customer_id
    ORDER BY sale_date
) as total_txn
from sales;
```

## 7.4 MIN() OVER() and MAX() OVER()

### Business Scenario

The sales manager asks:

> **"For every sale, show the lowest and highest purchase amount of the customer."**

The report should display:

* Current Sale
* Lowest Purchase
* Highest Purchase

without removing any rows.

***

## Sample Sales Table

| sale\_id | customer\_id | sale\_date | amount |
| -------: | -----------: | ---------- | -----: |
|        1 |          101 | 2024-01-01 |    100 |
|        2 |          101 | 2024-01-02 |    150 |
|        3 |          101 | 2024-01-03 |    120 |
|        4 |          101 | 2024-01-04 |    200 |
|        5 |          101 | 2024-01-05 |    180 |

***

## Can GROUP BY Solve This?

```sql
SELECT
    customer_id,
    MIN(amount) AS minimum_amount,
    MAX(amount) AS maximum_amount
FROM sales
GROUP BY customer_id;
```

Output

| customer\_id | minimum\_amount | maximum\_amount |
| ------------ | --------------: | --------------: |
| 101          |             100 |             200 |

Correct.

But all the sales disappeared.

***

## What We Actually Need

| sale\_date | amount | minimum\_amount | maximum\_amount |
| ---------- | -----: | --------------: | --------------: |
| 2024-01-01 |    100 |             100 |             200 |
| 2024-01-02 |    150 |             100 |             200 |
| 2024-01-03 |    120 |             100 |             200 |
| 2024-01-04 |    200 |             100 |             200 |
| 2024-01-05 |    180 |             100 |             200 |

Every sale remains.

The minimum and maximum values are attached to every row.

***

## Window Function Solution

```sql
SELECT
    customer_id,
    sale_date,
    amount,
    MIN(amount)
    OVER(
        PARTITION BY customer_id
    ) AS minimum_amount,
    MAX(amount)
    OVER(
        PARTITION BY customer_id
    ) AS maximum_amount
FROM sales;
```

***

## Output

| sale\_date | amount | minimum\_amount | maximum\_amount |
| ---------- | -----: | --------------: | --------------: |
| 2024-01-01 |    100 |             100 |             200 |
| 2024-01-02 |    150 |             100 |             200 |
| 2024-01-03 |    120 |             100 |             200 |
| 2024-01-04 |    200 |             100 |             200 |
| 2024-01-05 |    180 |             100 |             200 |

Notice

The Window contains all rows.

Therefore,

the minimum and maximum never change.

***

## Running Minimum

Now let's add

```sql
ORDER BY sale_date
```

```sql
SELECT
    customer_id,
    sale_date,
    amount,
    MIN(amount)
    OVER(
        PARTITION BY customer_id
        ORDER BY sale_date
    ) AS running_minimum
FROM sales;
```

***

## Output

| sale\_date | amount | running\_minimum |
| ---------- | -----: | ---------------: |
| 2024-01-01 |    100 |              100 |
| 2024-01-02 |    150 |              100 |
| 2024-01-03 |    120 |              100 |
| 2024-01-04 |    200 |              100 |
| 2024-01-05 |    180 |              100 |

***

## How SQL Calculates

#### Row 1

Window

```
100
```

Minimum

```
100
```

***

#### Row 2

Window

```
100

150
```

Minimum

```
100
```

***

#### Row 3

Window

```
100

150

120
```

Minimum

```
100
```

The smallest value seen so far is still **100**.

***

## Running Maximum

```sql
SELECT
    customer_id,
    sale_date,
    amount,
    MAX(amount)
    OVER(
        PARTITION BY customer_id
        ORDER BY sale_date
    ) AS running_maximum
FROM sales;
```

***

## Output

| sale\_date | amount | running\_maximum |
| ---------- | -----: | ---------------: |
| 2024-01-01 |    100 |              100 |
| 2024-01-02 |    150 |              150 |
| 2024-01-03 |    120 |              150 |
| 2024-01-04 |    200 |              200 |
| 2024-01-05 |    180 |              200 |

***

## How SQL Calculates

#### Row 1

Window

```
100
```

Maximum

```
100
```

***

#### Row 2

Window

```
100

150
```

Maximum

```
150
```

***

#### Row 3

Window

```
100

150

120
```

Maximum

```
150
```

***

#### Row 4

Window

```
100

150

120

200
```

Maximum

```
200
```

***

#### Row 5

Window

```
100

150

120

200

180
```

Maximum

```
200
```

***

## Visual Representation

#### Running Minimum

```
100

↓

100

----------------

100

150

↓

100

----------------

100

150

120

↓

100

----------------

100

150

120

200

↓

100
```

The smallest value never changes because no value smaller than **100** appears.

***

#### Running Maximum

```
100

↓

100

----------------

100

150

↓

150

----------------

100

150

120

↓

150

----------------

100

150

120

200

↓

200

----------------

100

150

120

200

180

↓

200
```

The maximum changes only when a larger value appears.

***

## Without ORDER BY

```sql
MIN(amount)
OVER(
PARTITION BY customer_id
)
```

Every row gets

```
100
```

***

```sql
MAX(amount)
OVER(
PARTITION BY customer_id
)
```

Every row gets

```
200
```

***

## With ORDER BY

```sql
MIN(amount)
OVER(
PARTITION BY customer_id
ORDER BY sale_date
)
```

Returns the **lowest amount seen so far**.

***

```sql
MAX(amount)
OVER(
PARTITION BY customer_id
ORDER BY sale_date
)
```

Returns the **highest amount seen so far**.

***

## Comparison

| Function | Without ORDER BY | With ORDER BY                    |
| -------- | ---------------- | -------------------------------- |
| `MIN()`  | Overall minimum  | Running minimum (lowest so far)  |
| `MAX()`  | Overall maximum  | Running maximum (highest so far) |

***

## Real-World Uses

#### Banking

* Lowest account balance
* Highest account balance

***

#### Sales

* Lowest sale amount
* Highest sale amount

***

#### Stock Market

* Lowest stock price so far
* Highest stock price so far

***

#### Inventory

* Lowest stock level
* Highest stock level

***

## Mental Model

```
MIN()

↓

Smallest Value

+

PARTITION BY

↓

Restart Minimum

+

ORDER BY

↓

Lowest So Far
```

```
MAX()

↓

Largest Value

+

PARTITION BY

↓

Restart Maximum

+

ORDER BY

↓

Highest So Far
```

***

## Key Takeaways

* `MIN() OVER()` preserves every row while finding the smallest value.
* `MAX() OVER()` preserves every row while finding the largest value.
* Without `ORDER BY`, every row gets the overall minimum or maximum of the partition.
* With `ORDER BY`, SQL returns the lowest or highest value **seen up to the current row**.
* `PARTITION BY` restarts the calculation for each group.

```sql
%%sql
-- For every sale, show the lowest and highest purchase amount of the customer.
select customer_id,
sale_date, amount,
MIN(amount) OVER (
    PARTITION BY customer_id
) as min_order_amount,

MAX(amount) OVER(
    PARTITION BY customer_id
) as max_order_amount

from sales;
```

## Chapter 8 — Ranking Window Functions

Until now, we have learned **Aggregate Window Functions**.

They perform calculations such as:

* SUM()
* AVG()
* COUNT()
* MIN()
* MAX()

Now we are moving to another category of Window Functions called **Ranking Functions**.

Unlike aggregate functions, ranking functions do **not** perform mathematical calculations.

Instead, they assign a **position** or **rank** to each row within a window.

Some common ranking functions are:

* ROW\_NUMBER()
* RANK()
* DENSE\_RANK()
* NTILE()

In this chapter, we will start with the most fundamental ranking function:

### `ROW_NUMBER()`

***

## 8.1 What is `ROW_NUMBER()`?

### Definition

`ROW_NUMBER()` assigns a **unique sequential number** to every row within a window.

The numbering starts from **1** and increases by **1** for every subsequent row.

If `PARTITION BY` is used, the numbering **restarts from 1** for each partition.

Unlike `RANK()` or `DENSE_RANK()`, `ROW_NUMBER()` **never assigns the same number to two rows**.

Even if two rows have the same value, each row receives a different number.

***

## Syntax

```sql
ROW_NUMBER()
OVER(
    PARTITION BY column_name
    ORDER BY column_name
)
```

***

## Syntax Breakdown

### `ROW_NUMBER()`

This is the Window Function.

Unlike `SUM()` or `AVG()`, it does **not** accept any arguments.

```sql
ROW_NUMBER()
```

It simply generates sequential numbers.

***

### `OVER()`

Defines the window in which the numbering will happen.

***

### `PARTITION BY`

Creates separate groups.

The numbering starts again from **1** inside every partition.

If omitted, the entire table is treated as a single partition.

***

### `ORDER BY`

Determines the order in which the rows receive their numbers.

This is the **most important clause**.

Without it, the numbering has no meaningful sequence.

***

## Think Like SQL

Suppose we have the following salaries.

| Employee | Salary |
| -------- | -----: |
| Ram      |  70000 |
| Priya    |  60000 |
| Kumar    |  55000 |
| Divya    |  50000 |

SQL first sorts the rows.

```
70000

↓

60000

↓

55000

↓

50000
```

Then SQL assigns numbers.

```
70000

↓

1

----------------

60000

↓

2

----------------

55000

↓

3

----------------

50000

↓

4
```

Notice

SQL is **not calculating anything**.

It is simply assigning positions.

***

## Important Difference

Aggregate Functions

```
Calculate Values
```

Ranking Functions

```
Assign Positions
```

***

## Important Note About Ties

Suppose two employees earn the same salary.

| Employee | Salary |
| -------- | -----: |
| Ram      |  70000 |
| Priya    |  70000 |
| Kumar    |  60000 |

Using

```sql
ROW_NUMBER()
```

The output could be

| Employee | Salary | Row Number |
| -------- | -----: | ---------: |
| Ram      |  70000 |          1 |
| Priya    |  70000 |          2 |
| Kumar    |  60000 |          3 |

Notice

Even though Ram and Priya have the same salary,

they receive different row numbers.

`ROW_NUMBER()` **never shares numbers**.

***

## Mental Model

```
ORDER BY

↓

Arrange Rows

↓

ROW_NUMBER()

↓

Assign Position

↓

1

2

3

4

5
```

***

## Key Takeaways

* `ROW_NUMBER()` assigns a unique sequential number to every row.
* The numbering starts from **1**.
* `PARTITION BY` restarts the numbering for each group.
* `ORDER BY` decides the numbering sequence.
* `ROW_NUMBER()` never gives the same number to two rows, even if values are equal.
* `ROW_NUMBER()` assigns positions—it does **not** perform calculations.

***

## Next Topic

### 8.2 Example 1 — Numbering Employees by Salary

In the next section, we'll use `ROW_NUMBER()` to rank employees based on their salaries and see how `PARTITION BY` changes the numbering.

```sql
%%sql

select customer_id,sale_date,amount,
ROW_NUMBER() OVER (
    PARTITION BY customer_id
) 
from sales;
```

```sql
%%sql
-- Display the employee with row number based on Department and sorted by employee name 
select emp_id,emp_name,department,
ROW_NUMBER() OVER(
    PARTITION BY department
    ORDER BY emp_name
) as number
from employee;
```

## 8.3 Example 2 — Finding the Latest Version of Each Customer

### Business Scenario

In many real-world applications, records are **not updated in place**.

Instead, every time a customer updates their information, a **new version** of the record is inserted.

This allows us to maintain the complete history of changes.

For example,

Customer **101** changes their email address three times.

Instead of deleting the old email,

the database stores all three versions.

***

## CUSTOMER\_HISTORY

| customer\_id | customer\_name | email              | updated\_at         |
| ------------ | -------------- | ------------------ | ------------------- |
| 101          | Anitha         | anitha@old.com     | 2024-01-01 09:00:00 |
| 101          | Anitha         | anitha@gmail.com   | 2024-03-01 10:00:00 |
| 101          | Anitha R       | anitha.r@gmail.com | 2024-05-01 11:30:00 |
| 102          | Suresh         | suresh@yahoo.com   | 2024-02-10 08:15:00 |
| 102          | Suresh         | suresh@gmail.com   | 2024-04-12 14:00:00 |
| 103          | Lakshmi        | lakshmi@gmail.com  | 2024-01-20 16:45:00 |

***

## Requirement

The business asks:

> **"Show me the latest version of every customer."**

To do that,

we first need to identify

which record is the newest.

***

## Solution

```sql
SELECT
    customer_id,
    customer_name,
    email,
    updated_at,
    ROW_NUMBER()
    OVER(
        PARTITION BY customer_id
        ORDER BY updated_at DESC
    ) AS rn
FROM customer_history;
```

***

## Output

| customer\_id | customer\_name | email              | updated\_at         | rn |
| ------------ | -------------- | ------------------ | ------------------- | -: |
| 101          | Anitha R       | anitha.r@gmail.com | 2024-05-01 11:30:00 |  1 |
| 101          | Anitha         | anitha@gmail.com   | 2024-03-01 10:00:00 |  2 |
| 101          | Anitha         | anitha@old.com     | 2024-01-01 09:00:00 |  3 |
| 102          | Suresh         | suresh@gmail.com   | 2024-04-12 14:00:00 |  1 |
| 102          | Suresh         | suresh@yahoo.com   | 2024-02-10 08:15:00 |  2 |
| 103          | Lakshmi        | lakshmi@gmail.com  | 2024-01-20 16:45:00 |  1 |

***

## How SQL Thinks

### Step 1

Partition the data by

```sql
customer_id
```

Customer **101**

```
Anitha

↓

Anitha

↓

Anitha R
```

Customer **102**

```
Suresh

↓

Suresh
```

Customer **103**

```
Lakshmi
```

Each customer is processed separately.

***

### Step 2

Sort each partition

```sql
ORDER BY updated_at DESC
```

Customer **101**

```
2024-05-01

↓

2024-03-01

↓

2024-01-01
```

Newest record comes first.

***

### Step 3

Assign Row Numbers

Customer **101**

```
2024-05-01

↓

rn = 1

------------------

2024-03-01

↓

rn = 2

------------------

2024-01-01

↓

rn = 3
```

Customer **102**

```
2024-04-12

↓

rn = 1

------------------

2024-02-10

↓

rn = 2
```

Customer **103**

```
2024-01-20

↓

rn = 1
```

Notice

The numbering restarts for every customer because of

```sql
PARTITION BY customer_id
```

***

## Why DESC?

We want

the **latest record**.

Latest means

```
Highest Date

↓

Newest Version
```

Therefore

```sql
ORDER BY updated_at DESC
```

places the newest record first,

which receives

```
rn = 1
```

If we used

```sql
ORDER BY updated_at ASC
```

the oldest record would receive

```
rn = 1
```

which is usually **not** what we want.

***

## Finding Only the Latest Record

Once every row has a row number,

finding the latest version becomes easy.

```sql
SELECT *
FROM
(
    SELECT
        customer_id,
        customer_name,
        email,
        updated_at,
        ROW_NUMBER()
        OVER(
            PARTITION BY customer_id
            ORDER BY updated_at DESC
        ) AS rn
    FROM customer_history
) latest_customer
WHERE rn = 1;
```

***

## Final Output

| customer\_id | customer\_name | email              | updated\_at         |
| ------------ | -------------- | ------------------ | ------------------- |
| 101          | Anitha R       | anitha.r@gmail.com | 2024-05-01 11:30:00 |
| 102          | Suresh         | suresh@gmail.com   | 2024-04-12 14:00:00 |
| 103          | Lakshmi        | lakshmi@gmail.com  | 2024-01-20 16:45:00 |

Now we have only the latest version of each customer.

***

## Real-World Uses

#### Data Warehousing

Find the current version of dimension records.

***

#### Banking

Latest customer profile.

***

#### HR

Most recent employee information.

***

#### E-Commerce

Latest customer address.

***

#### CRM Systems

Current customer details after multiple updates.

***

## Interview Tip

This is one of the most frequently asked SQL interview questions:

> **How do you retrieve the latest record for each customer?**

The standard solution is:

1. Partition by the unique identifier.
2. Order by the timestamp in descending order.
3. Assign `ROW_NUMBER()`.
4. Filter where `rn = 1`.

***

## Mental Model

```
PARTITION BY customer_id

↓

Create One Group Per Customer

↓

ORDER BY updated_at DESC

↓

Newest Record First

↓

ROW_NUMBER()

↓

1

2

3

↓

Keep Only

rn = 1
```

***

## Key Takeaways

* `PARTITION BY` creates one window for each customer.
* `ORDER BY updated_at DESC` places the newest record first.
* `ROW_NUMBER()` assigns sequential numbers within each customer's history.
* `rn = 1` always represents the latest record for that customer.
* This pattern is widely used to retrieve the current version from versioned or historical tables.

```sql
%%sql
select customer_id,customer_name,email,
ROW_NUMBER() OVER(
    PARTITION BY customer_id
    ORDER BY updated_at desc
) rn
from customer_history;
```

***

#### This is fine for less record

* Suppose your history table has 10 million records.
* You only want the latest version of every customer.
* Instead of writing complicated SQL, you simply do:

```sql
%%sql
SELECT * 
FROM(
    SELECT *,
    ROW_NUMBER() OVER(
        PARTITION BY customer_id
    ) as rn
    FROM customer_history
) t
where rn =1;
```

## Chapter 9 — `RANK()`

Until now, we have learned:

* `ROW_NUMBER()` → Every row gets a unique number.

Now we will learn another ranking function:

### `RANK()`

Unlike `ROW_NUMBER()`, `RANK()` allows **ties**.

If two or more rows have the same value, they receive the **same rank**.

The next rank is **skipped**.

This is known as **Competition Ranking** or **Olympic Ranking**.

***

## 9.1 What is `RANK()`?

### Definition

`RANK()` assigns a position to each row based on the `ORDER BY` clause.

* Equal values receive the **same rank**.
* After a tie, SQL skips the next rank.
* The numbering restarts for every partition if `PARTITION BY` is used.

***

## Syntax

```sql
RANK()
OVER(
    PARTITION BY column_name
    ORDER BY column_name
)
```

***

## Think About a Running Race

Suppose four students finish a race.

```
🥇 Student A

🥈 Student B

🥈 Student C

🥉 Student D
```

Can Student D be **3rd**?

No.

Two students already occupied **2nd place**.

Therefore,

Student D becomes

```
4th
```

This is exactly how `RANK()` works.

***

## Example

Suppose the salaries are

| Employee | Salary |
| -------- | -----: |
| Divya    |  70000 |
| John     |  60000 |
| Meena    |  60000 |
| Arun     |  55000 |

***

SQL first sorts the salaries.

```
70000

↓

60000

↓

60000

↓

55000
```

***

Then SQL assigns ranks.

| Salary | Rank |
| -----: | ---: |
|  70000 |    1 |
|  60000 |    2 |
|  60000 |    2 |
|  55000 |    4 |

Notice

```
1

2

2

4
```

Rank **3** does not exist.

***

## Why is Rank 3 Missing?

Imagine employees standing in a queue.

```
Position

1 → Divya

2 → John

3 → Meena

4 → Arun
```

John and Meena have the same salary.

Both deserve

```
Rank = 2
```

Arun is physically standing in

```
Position 4
```

Therefore

```
Rank = 4
```

The skipped rank shows that **two employees shared Rank 2**.

***

## Employee Table

| Employee | Salary |
| -------- | -----: |
| Divya    |  70000 |
| John     |  60000 |
| Meena    |  60000 |
| Arun     |  55000 |
| Priya    |  45000 |
| Kumar    |  40000 |
| Ravi     |  35000 |
| Ram      |  30000 |

***

## Query

```sql
SELECT
    emp_name,
    department,
    salary,
    RANK()
    OVER(
        ORDER BY salary DESC
    ) AS company_rank
FROM employee;
```

***

## Output

| Employee | Salary | Rank |
| -------- | -----: | ---: |
| Divya    |  70000 |    1 |
| John     |  60000 |    2 |
| Meena    |  60000 |    2 |
| Arun     |  55000 |    4 |
| Priya    |  45000 |    5 |
| Kumar    |  40000 |    6 |
| Ravi     |  35000 |    7 |
| Ram      |  30000 |    8 |

***

## How SQL Thinks

#### Step 1

Sort by salary.

```
70000

↓

60000

↓

60000

↓

55000

↓

45000

↓

40000

↓

35000

↓

30000
```

***

#### Step 2

Assign ranks.

```
70000

↓

Rank 1

----------------

60000

↓

Rank 2

----------------

60000

↓

Rank 2

----------------

55000

↓

Rank 4

----------------

45000

↓

Rank 5

----------------

40000

↓

Rank 6

----------------

35000

↓

Rank 7

----------------

30000

↓

Rank 8
```

***

## Important Difference from ROW\_NUMBER()

#### ROW\_NUMBER()

```
70000

↓

1

60000

↓

2

60000

↓

3

55000

↓

4
```

Every row gets a unique number.

***

#### RANK()

```
70000

↓

1

60000

↓

2

60000

↓

2

55000

↓

4
```

Equal values share the same rank.

The next rank is skipped.

***

## Real-World Uses

#### Sales Leaderboard

If two salespersons achieve the same sales amount, both receive the same rank.

***

#### Sports Competition

Athletes with equal scores receive the same position.

***

#### Exam Results

Students with identical marks receive the same rank.

***

#### Employee Performance

Employees with equal ratings receive the same rank.

***

## Mental Model

```
ORDER BY

↓

Sort Rows

↓

Same Values?

↓

Yes

↓

Share the Same Rank

↓

Skip the Next Rank
```

***

## Key Takeaways

* `RANK()` assigns positions based on the `ORDER BY` clause.
* Equal values receive the same rank.
* The next rank is skipped after a tie.
* `PARTITION BY` restarts the ranking for each group.
* `RANK()` is ideal for competition-style rankings.

```sql
%%sql
-- Here we have displayed the rank of all employees
select emp_id,emp_name,department,salary,
RANK() OVER(
    order by salary desc
) as company_rank
from employee;
```

```sql
%%sql
-- Here we have displayed the rank of all employees with their department
select emp_id,emp_name,department,salary,
RANK() OVER(
    partition by department
    order by salary desc
) as company_rank
from employee;
```

## Chapter 10 — `DENSE_RANK()`

In the previous chapter, we learned `RANK()`.

Remember this result.

| Salary | Rank |
| -----: | ---: |
|  70000 |    1 |
|  60000 |    2 |
|  60000 |    2 |
|  55000 |    4 |

Notice something strange.

```
1

2

2

4
```

Where is Rank **3**?

It was skipped because two employees shared Rank 2.

Sometimes this is exactly what the business wants.

But sometimes the business asks a different question.

> **"Ignore duplicate salaries. Just tell me the 1st, 2nd, 3rd highest salary."**

That's where `DENSE_RANK()` is used.

***

## 10.1 What is `DENSE_RANK()`?

### Definition

`DENSE_RANK()` assigns a rank based on the `ORDER BY` clause.

* Equal values receive the same rank.
* The next distinct value receives the next consecutive rank.
* No ranks are skipped.

Unlike `RANK()`, `DENSE_RANK()` ranks **distinct values**, not row positions.

***

## Syntax

```sql
DENSE_RANK()
OVER(
    PARTITION BY column_name
    ORDER BY column_name
)
```

***

## Example

Suppose the salaries are

| Employee | Salary |
| -------- | -----: |
| Divya    |  70000 |
| John     |  60000 |
| Meena    |  60000 |
| Arun     |  55000 |

***

SQL sorts the salaries.

```
70000

↓

60000

↓

60000

↓

55000
```

Then SQL assigns dense ranks.

| Salary | Dense Rank |
| -----: | ---------: |
|  70000 |          1 |
|  60000 |          2 |
|  60000 |          2 |
|  55000 |          3 |

Notice

```
1

2

2

3
```

No numbers are skipped.

***

## Why?

Think about **distinct salary values**.

```
70000

↓

60000

↓

55000
```

There are only

```
3
```

different salaries.

Therefore

```
70000 → Rank 1

60000 → Rank 2

55000 → Rank 3
```

The duplicate 60000 does not create another rank.

***

## Employee Table

| Employee | Salary |
| -------- | -----: |
| Divya    |  70000 |
| John     |  60000 |
| Meena    |  60000 |
| Arun     |  55000 |
| Priya    |  45000 |
| Kumar    |  40000 |
| Ravi     |  35000 |
| Ram      |  30000 |

***

## Query

```sql
SELECT
    emp_name,
    salary,
    DENSE_RANK()
    OVER(
        ORDER BY salary DESC
    ) AS dense_rank
FROM employee;
```

***

## Output

| Employee | Salary | Dense Rank |
| -------- | -----: | ---------: |
| Divya    |  70000 |          1 |
| John     |  60000 |          2 |
| Meena    |  60000 |          2 |
| Arun     |  55000 |          3 |
| Priya    |  45000 |          4 |
| Kumar    |  40000 |          5 |
| Ravi     |  35000 |          6 |
| Ram      |  30000 |          7 |

***

## How SQL Thinks

#### Step 1

Sort the salaries.

```
70000

↓

60000

↓

60000

↓

55000

↓

45000

↓

40000

↓

35000

↓

30000
```

***

#### Step 2

Assign dense ranks.

```
70000

↓

Rank 1

----------------

60000

↓

Rank 2

----------------

60000

↓

Rank 2

----------------

55000

↓

Rank 3

----------------

45000

↓

Rank 4

----------------

40000

↓

Rank 5

----------------

35000

↓

Rank 6

----------------

30000

↓

Rank 7
```

Notice

The next distinct salary gets the next rank.

No numbers are skipped.

***

## Comparison of All Three Ranking Functions

```sql
SELECT
    emp_name,
    salary,
    ROW_NUMBER() OVER(
        ORDER BY salary DESC, emp_id
    ) AS row_num,
    RANK() OVER(
        ORDER BY salary DESC
    ) AS rnk,
    DENSE_RANK() OVER(
        ORDER BY salary DESC
    ) AS dense_rnk
FROM employee;
```

***

## Output

| Employee | Salary | ROW\_NUMBER | RANK | DENSE\_RANK |
| -------- | -----: | ----------: | ---: | ----------: |
| Divya    |  70000 |           1 |    1 |           1 |
| John     |  60000 |           2 |    2 |           2 |
| Meena    |  60000 |           3 |    2 |           2 |
| Arun     |  55000 |           4 |    4 |           3 |
| Priya    |  45000 |           5 |    5 |           4 |
| Kumar    |  40000 |           6 |    6 |           5 |
| Ravi     |  35000 |           7 |    7 |           6 |
| Ram      |  30000 |           8 |    8 |           7 |

***

## Understanding the Difference

#### `ROW_NUMBER()`

Every row gets a unique number.

```
1

2

3

4
```

***

#### `RANK()`

Equal values share the same rank.

The next rank is skipped.

```
1

2

2

4
```

***

#### `DENSE_RANK()`

Equal values share the same rank.

The next distinct value gets the next rank.

```
1

2

2

3
```

***

## Comparison Table

| Aspect           | `ROW_NUMBER()`              | `RANK()`            | `DENSE_RANK()`         |
| ---------------- | --------------------------- | ------------------- | ---------------------- |
| Duplicate Values | Unique number for every row | Same rank           | Same rank              |
| Skips Ranks      | No                          | Yes                 | No                     |
| Sequence         | 1,2,3,4                     | 1,2,2,4             | 1,2,2,3                |
| Best For         | Pagination, Deduplication   | Competition Ranking | Distinct Value Ranking |

***

## Real-World Uses

#### Employee Salary

Find the 3rd highest distinct salary.

***

#### Product Pricing

Rank unique price levels.

***

#### Student Marks

Rank distinct score groups.

***

#### Sales Analysis

Identify revenue tiers.

***

## Interview Question

#### Find the 3rd Highest Salary

```sql
SELECT *
FROM
(
    SELECT
        emp_name,
        salary,
        DENSE_RANK() OVER(
            ORDER BY salary DESC
        ) AS dr
    FROM employee
) x
WHERE dr = 3;
```

Output

| Employee | Salary |
| -------- | -----: |
| Arun     |  55000 |

This works because

```
70000 → Rank 1

60000 → Rank 2

55000 → Rank 3
```

***

## Mental Model

```
ROW_NUMBER()

↓

Every Row Gets a Number

--------------------------

RANK()

↓

Equal Values Share a Rank

↓

Skip Next Rank

--------------------------

DENSE_RANK()

↓

Equal Values Share a Rank

↓

No Skipped Ranks
```

***

## Key Takeaways

* `DENSE_RANK()` assigns the same rank to equal values.
* The next distinct value receives the next consecutive rank.
* No rank numbers are skipped.
* It is ideal for finding the **Nth highest distinct value**.
* It is one of the most frequently asked SQL interview functions.

```sql
%%sql
-- Here we have displayed the rank of all employees with their department group
-- Example 1
select emp_id,emp_name,salary,
DENSE_RANK() OVER(
    ORDER BY salary
)
from employee;
```

```sql
%%sql
-- Example 2
-- Here we have displayed the rank of all employees with their department group

select emp_id,emp_name,salary,department,
DENSE_RANK() OVER(
    PARTITION BY department
    ORDER BY salary
) as dense_rnk
from employee;
```

```sql
%%sql
-- All 3 Ranking in one 

select emp_id,emp_name,salary, 
ROW_NUMBER() OVER( ORDER BY salary desc ) as row_num,
RANK() OVER( ORDER BY salary desc ) as rnk,
DENSE_RANK() OVER(
    ORDER BY salary desc
) as dense_rnk
from employee;
```

## Chapter 11 — `PERCENT_RANK()`

So far, we have learned three ranking functions.

| Function       | Example    |
| -------------- | ---------- |
| `ROW_NUMBER()` | 1, 2, 3, 4 |
| `RANK()`       | 1, 2, 2, 4 |
| `DENSE_RANK()` | 1, 2, 2, 3 |

These functions tell us **the position** of a row.

But sometimes businesses don't want a position.

They want a **percentage**.

For example,

Instead of asking

> "What is the employee's rank?"

they ask

> "Is this employee in the Top 10%?"

This is where `PERCENT_RANK()` is useful.

***

## 11.1 What is `PERCENT_RANK()`?

### Definition

`PERCENT_RANK()` returns the relative position of a row within its partition as a value between **0 and 1**.

* The first row always has a percent rank of **0**.
* The last row always has a percent rank of **1**.
* All other rows fall somewhere between **0 and 1**.

Unlike `RANK()`, which returns an integer, `PERCENT_RANK()` returns a decimal.

***

## Syntax

```sql
PERCENT_RANK()
OVER(
    PARTITION BY column_name
    ORDER BY column_name
)
```

***

## How is it Calculated?

Internally SQL uses the formula

```
(RANK - 1)

--------------------

(Total Rows - 1)
```

Notice

It uses **RANK()**

not

```
ROW_NUMBER()
```

or

```
DENSE_RANK()
```

***

## Sample Employee Table

| Employee | Salary |
| -------- | -----: |
| Divya    |  70000 |
| John     |  60000 |
| Meena    |  60000 |
| Arun     |  55000 |
| Priya    |  45000 |
| Kumar    |  40000 |
| Ravi     |  35000 |
| Ram      |  30000 |

There are

```
8 Employees
```

Therefore

```
Total Rows = 8
```

***

## Query

```sql
SELECT
    emp_name,
    salary,
    RANK()
    OVER(
        ORDER BY salary DESC
    ) AS rnk,

    PERCENT_RANK()
    OVER(
        ORDER BY salary DESC
    ) AS percent_rank
FROM employee;
```

***

## Output

| Employee | Salary | Rank | Percent Rank |
| -------- | -----: | ---: | -----------: |
| Divya    |  70000 |    1 |        0.000 |
| John     |  60000 |    2 |        0.143 |
| Meena    |  60000 |    2 |        0.143 |
| Arun     |  55000 |    4 |        0.429 |
| Priya    |  45000 |    5 |        0.571 |
| Kumar    |  40000 |    6 |        0.714 |
| Ravi     |  35000 |    7 |        0.857 |
| Ram      |  30000 |    8 |        1.000 |

***

## How SQL Calculates

Since

```
Total Rows = 8
```

The denominator is

```
8 - 1 = 7
```

***

#### Divya

```
Rank = 1

(1-1)/7

=

0
```

***

#### John

```
Rank = 2

(2-1)/7

=

1/7

=

0.143
```

***

#### Meena

```
Rank = 2

(2-1)/7

=

0.143
```

Notice

John and Meena share the same percentage because they share the same rank.

***

#### Arun

```
Rank = 4

(4-1)/7

=

3/7

=

0.429
```

***

#### Ram

```
Rank = 8

(8-1)/7

=

1
```

The last row always gets

```
1.000
```

***

## Visual Representation

```
Top Employee

↓

0%

----------------

14%

----------------

14%

----------------

43%

----------------

57%

----------------

71%

----------------

86%

----------------

100%

Bottom Employee
```

***

## Difference Between RANK() and PERCENT\_RANK()

| `RANK()`           | `PERCENT_RANK()`        |
| ------------------ | ----------------------- |
| Returns an integer | Returns a decimal       |
| Shows position     | Shows relative position |
| Example: 4         | Example: 0.429          |

***

## Real-World Uses

#### Employee Performance

Top performers by percentage.

***

#### Student Rankings

Top 5% of students.

***

#### Sales Dashboard

Identify the top-performing salespeople.

***

#### Data Analysis

Understand where a value lies within a distribution.

***

## Interview Notes

#### Formula

```
(RANK - 1)

/

(Total Rows - 1)
```

#### Important Points

* First row always returns **0**.
* Last row always returns **1**.
* Equal ranks receive the same percentage.
* Uses `RANK()`, not `DENSE_RANK()`.

***

## Mental Model

```
RANK()

↓

Convert Rank

↓

Percentage

↓

0

↓

1
```

***

## Key Takeaways

* `PERCENT_RANK()` returns a value between **0 and 1**.
* It shows the relative position of a row within its partition.
* It is calculated using the row's `RANK()`.
* Tied rows receive the same percentage.
* It is useful for percentile-based reporting and analytics.

```sql
%%sql

select 
emp_name,salary,
RANK() OVER(
	ORDER BY salary DESC
)as rnk,
PERCENT_RANK() OVER(
	ORDER BY salary DESC
) AS percent_rnk2
FROM employee;
```

## Chapter 12 — `NTILE()`

So far, we have learned ranking functions that answer questions like:

* What is my position?
* What is my rank?
* What percentage is my rank?

Now imagine the business asks a completely different question.

> **"Divide all employees into 4 performance groups."**

or

> **"Split all customers into 10 equal groups."**

or

> **"Show the Top 25% of products."**

This is exactly what `NTILE()` does.

***

## 12.1 What is `NTILE()`?

### Definition

`NTILE()` divides the rows in each window into a specified number of nearly equal-sized groups called **tiles** or **buckets**.

Each row is assigned a bucket number starting from **1**.

Unlike `RANK()`, `NTILE()` does **not** care about duplicate values.

Its goal is to divide the rows into groups of approximately equal size.

***

## Syntax

```sql
NTILE(number_of_groups)
OVER(
    PARTITION BY column_name
    ORDER BY column_name
)
```

***

## Syntax Breakdown

### `NTILE(4)`

Means

```
Divide the rows into

4

equal groups.
```

***

### `OVER()`

Defines the window.

***

### `PARTITION BY`

Optional.

If used,

each partition is divided separately.

***

### `ORDER BY`

Very important.

Rows are divided **after sorting**.

***

## Think Like SQL

Suppose we have

```
8 Employees
```

and we write

```sql
NTILE(4)
```

SQL first sorts the employees.

```
Employee 1

↓

Employee 2

↓

Employee 3

↓

Employee 4

↓

Employee 5

↓

Employee 6

↓

Employee 7

↓

Employee 8
```

Then SQL divides them.

```
Group 1

1

2

----------------

Group 2

3

4

----------------

Group 3

5

6

----------------

Group 4

7

8
```

Each group contains

```
2 Employees
```

***

## Example

Employee salaries

| Employee | Salary |
| -------- | -----: |
| Divya    |  70000 |
| John     |  60000 |
| Meena    |  60000 |
| Arun     |  55000 |
| Priya    |  45000 |
| Kumar    |  40000 |
| Ravi     |  35000 |
| Ram      |  30000 |

***

## Query

```sql
SELECT
    emp_name,
    salary,
    NTILE(4)
    OVER(
        ORDER BY salary DESC
    ) AS performance_group
FROM employee;
```

***

## Output

| Employee | Salary | Group |
| -------- | -----: | ----: |
| Divya    |  70000 |     1 |
| John     |  60000 |     1 |
| Meena    |  60000 |     2 |
| Arun     |  55000 |     2 |
| Priya    |  45000 |     3 |
| Kumar    |  40000 |     3 |
| Ravi     |  35000 |     4 |
| Ram      |  30000 |     4 |

***

## How SQL Thinks

#### Step 1

Sort salaries.

```
70000

↓

60000

↓

60000

↓

55000

↓

45000

↓

40000

↓

35000

↓

30000
```

***

#### Step 2

Split into

```
4

groups
```

```
70000

60000

↓

Group 1

----------------

60000

55000

↓

Group 2

----------------

45000

40000

↓

Group 3

----------------

35000

30000

↓

Group 4
```

Notice

SQL is **not ranking**.

SQL is **distributing rows**.

***

## Uneven Distribution

Suppose there are

```
10 Employees
```

but we divide into

```sql
NTILE(4)
```

Can every group contain the same number?

No.

```
10 ÷ 4

=

2 remainder 2
```

SQL distributes the extra rows to the earlier groups.

```
Group 1 → 3 rows

Group 2 → 3 rows

Group 3 → 2 rows

Group 4 → 2 rows
```

The difference between groups is never more than **one row**.

***

## Another Example

12 students

```sql
NTILE(3)
```

Output

```
Group 1

4 Students

----------------

Group 2

4 Students

----------------

Group 3

4 Students
```

Perfectly balanced.

***

## Difference Between Ranking Functions

| Function         | Purpose                 |
| ---------------- | ----------------------- |
| `ROW_NUMBER()`   | Number every row        |
| `RANK()`         | Competition ranking     |
| `DENSE_RANK()`   | Rank distinct values    |
| `PERCENT_RANK()` | Relative position       |
| `NTILE()`        | Divide rows into groups |

***

## Real-World Uses

#### Customer Segmentation

Divide customers into Gold, Silver, Bronze groups.

***

#### Salary Bands

Create salary quartiles.

***

#### Student Performance

Split students into Top 10%, Middle, Bottom groups.

***

#### Marketing

Target the highest-value customer groups.

***

#### Data Analysis

Create quartiles, quintiles and deciles.

***

## Interview Examples

#### Quartiles

```sql
NTILE(4)
```

Creates

```
4 Groups
```

***

#### Quintiles

```sql
NTILE(5)
```

Creates

```
5 Groups
```

***

#### Deciles

```sql
NTILE(10)
```

Creates

```
10 Groups
```

***

## Mental Model

```
ORDER BY

↓

Sort Rows

↓

NTILE(4)

↓

Divide Into

↓

Group 1

Group 2

Group 3

Group 4
```

***

## Key Takeaways

* `NTILE()` divides rows into nearly equal-sized groups.
* Bucket numbers always start from **1**.
* `ORDER BY` determines how rows are distributed.
* Duplicate values do **not** receive special treatment.
* If rows cannot be divided equally, the earlier groups receive one extra row.
* `NTILE()` is commonly used for quartiles, quintiles, deciles, and customer segmentation.

```sql
%%sql

select emp_id,emp_name,
salary,
NTILE(4) OVER(
    ORDER BY salary desc
) as spend_qartile
FROM employee;
```

## Chapter 13 — Value Window Functions

So far, we have learned two major categories of Window Functions.

### Aggregate Window Functions

These calculate values.

* SUM()
* AVG()
* COUNT()
* MIN()
* MAX()

***

### Ranking Window Functions

These assign positions.

* ROW\_NUMBER()
* RANK()
* DENSE\_RANK()
* PERCENT\_RANK()
* NTILE()

***

Now we are entering the third category.

## Value Window Functions

Unlike aggregate functions,

they do **not calculate** totals.

Unlike ranking functions,

they do **not assign positions**.

Instead,

they allow us to access values from **other rows** inside the same window.

Some common Value Window Functions are:

* LAG()
* LEAD()
* FIRST\_VALUE()
* LAST\_VALUE()
* NTH\_VALUE()

These functions are extremely common in

* Sales Analysis
* Financial Reporting
* Stock Market Analysis
* Time-Series Data
* ETL Pipelines
* Data Warehousing

***

## Chapter 13.1 — `LAG()`

### Business Problem

Suppose your manager asks

> "Show today's sales along with yesterday's sales."

The table already contains today's sales.

But how do we access the previous row?

Without Window Functions,

this usually requires

* Self Join
* Correlated Subquery
* Complex SQL

With `LAG()`,

it becomes a single line of SQL.

***

## What is `LAG()`?

### Definition

`LAG()` returns the value from a previous row within the same window.

Think of it as

> **"Look behind the current row."**

It does **not** calculate anything.

It simply retrieves a value from an earlier row.

***

## Syntax

```sql
LAG(expression,
    offset,
    default_value)
OVER(
    PARTITION BY column_name
    ORDER BY column_name
)
```

***

## Syntax Breakdown

### expression

The column whose previous value you want.

Example

```sql
salary
```

***

### offset

How many rows back should SQL look?

```
1

↓

Previous Row
```

```
2

↓

Two Rows Back
```

```
3

↓

Three Rows Back
```

Default is

```
1
```

***

### default\_value

What should SQL return

if no previous row exists?

Example

```sql
0
```

or

```sql
NULL
```

If omitted,

SQL returns

```
NULL
```

***

### OVER()

Defines the window.

***

### ORDER BY

Determines

what "previous"

actually means.

Without ORDER BY,

there is no logical previous row.

***

## Think Like SQL

Suppose we have monthly sales.

| Month | Sales |
| ----- | ----: |
| Jan   |   100 |
| Feb   |   150 |
| Mar   |   200 |
| Apr   |   180 |

SQL first sorts the rows.

```
Jan

↓

Feb

↓

Mar

↓

Apr
```

Then SQL asks

For every row

```
Look

1 Row Behind
```

***

Result

| Month | Sales | Previous Sales |
| ----- | ----: | -------------: |
| Jan   |   100 |           NULL |
| Feb   |   150 |            100 |
| Mar   |   200 |            150 |
| Apr   |   180 |            200 |

Notice

The first row has no previous row.

Therefore

```
NULL
```

is returned.

***

## Query

```sql
SELECT
    month,
    sales,
    LAG(sales)
    OVER(
        ORDER BY month
    ) AS previous_sales
FROM monthly_sales;
```

***

## How SQL Thinks

Current Row

```
Jan

↓

Look Back

↓

Nothing Found

↓

NULL
```

***

Current Row

```
Feb

↓

Look Back

↓

Jan

↓

100
```

***

Current Row

```
Mar

↓

Look Back

↓

Feb

↓

150
```

***

Current Row

```
Apr

↓

Look Back

↓

Mar

↓

200
```

***

## Example Using Offset

```sql
SELECT
    month,
    sales,
    LAG(sales,2)
    OVER(
        ORDER BY month
    ) AS two_months_ago
FROM monthly_sales;
```

Output

| Month | Sales | Two Months Ago |
| ----- | ----: | -------------: |
| Jan   |   100 |           NULL |
| Feb   |   150 |           NULL |
| Mar   |   200 |            100 |
| Apr   |   180 |            150 |

SQL now looks

```
2 Rows Back
```

instead of one.

***

## Example Using Default Value

```sql
SELECT
    month,
    sales,
    LAG(sales,1,0)
    OVER(
        ORDER BY month
    ) AS previous_sales
FROM monthly_sales;
```

Output

| Month | Sales | Previous Sales |
| ----- | ----: | -------------: |
| Jan   |   100 |              0 |
| Feb   |   150 |            100 |
| Mar   |   200 |            150 |
| Apr   |   180 |            200 |

Instead of

```
NULL
```

SQL returns

```
0
```

***

## Real-World Uses

#### Month-over-Month Sales

Compare this month with last month.

***

#### Stock Market

Compare today's price with yesterday's price.

***

#### Employee Salary Changes

Compare current salary with previous salary.

***

#### Website Analytics

Compare today's visitors with yesterday's visitors.

***

#### IoT Sensor Data

Compare the latest reading with the previous reading.

***

## Mental Model

```
Current Row

↓

Look Back

↓

Previous Row

↓

Return Value
```

***

## Key Takeaways

* `LAG()` retrieves data from a previous row.
* It does not perform calculations.
* `ORDER BY` defines what "previous" means.
* The default offset is 1.
* If no previous row exists, `NULL` is returned unless a default value is specified.
* `LAG()` is widely used for trend analysis, comparisons, and time-series reporting.

```sql
%%sql
select sale_date,amount,
LAG(amount) over(
    order by sale_date
) as prev_amount,

amount - LAG(amount) over(
    order by sale_date
) as change_vs_prev

from sales
where customer_id = 101;
```

```sql
%%sql

-- grouped by customer id and displaying there row with the prev data
select row_number() OVER(
  partition by customer_id  
) as s_no, customer_id,sale_id,sale_date,amount,
LAG(amount) OVER (
    partition by customer_id
) as prev_data
from sales;
```

## Chapter 14 — `LEAD()`

In the previous chapter, we learned `LAG()`.

`LAG()` looks **backward**.

It retrieves a value from a **previous row**.

Now we'll learn its opposite.

### `LEAD()`

`LEAD()` looks **forward**.

Instead of retrieving the previous row,

it retrieves the value from a **future row**.

***

## 14.1 What is `LEAD()`?

### Definition

`LEAD()` returns the value from a subsequent (next) row within the same window.

Think of it as

> **"Look ahead of the current row."**

It does **not** perform any calculations.

It simply retrieves data from a future row.

***

## Syntax

```sql
LEAD(
    expression,
    offset,
    default_value
)
OVER(
    PARTITION BY column_name
    ORDER BY column_name
)
```

***

## Syntax Breakdown

### expression

The column whose future value you want.

Example

```sql
sales
```

***

### offset

How many rows ahead should SQL look?

```
1

↓

Next Row
```

```
2

↓

Two Rows Ahead
```

```
3

↓

Three Rows Ahead
```

Default

```
1
```

***

### default\_value

What should SQL return

if no future row exists?

Examples

```sql
NULL
```

or

```sql
0
```

If omitted,

SQL returns

```
NULL
```

***

### OVER()

Defines the window.

***

### ORDER BY

Determines

what "next"

actually means.

Without ORDER BY,

there is no logical next row.

***

## Think Like SQL

Suppose monthly sales are

| Month | Sales |
| ----- | ----: |
| Jan   |   100 |
| Feb   |   150 |
| Mar   |   200 |
| Apr   |   180 |

SQL first sorts the rows.

```
Jan

↓

Feb

↓

Mar

↓

Apr
```

Now SQL asks

For every row

```
Look

1 Row Ahead
```

***

## Result

| Month | Sales | Next Month Sales |
| ----- | ----: | ---------------: |
| Jan   |   100 |              150 |
| Feb   |   150 |              200 |
| Mar   |   200 |              180 |
| Apr   |   180 |             NULL |

Notice

The last row has

no next row.

Therefore

```
NULL
```

is returned.

***

## Query

```sql
SELECT
    month,
    sales,
    LEAD(sales)
    OVER(
        ORDER BY month
    ) AS next_month_sales
FROM monthly_sales;
```

***

## How SQL Thinks

Current Row

```
Jan

↓

Look Ahead

↓

Feb

↓

150
```

***

Current Row

```
Feb

↓

Look Ahead

↓

Mar

↓

200
```

***

Current Row

```
Mar

↓

Look Ahead

↓

Apr

↓

180
```

***

Current Row

```
Apr

↓

Look Ahead

↓

Nothing Found

↓

NULL
```

***

## Example Using Offset

```sql
SELECT
    month,
    sales,
    LEAD(sales,2)
    OVER(
        ORDER BY month
    ) AS two_months_later
FROM monthly_sales;
```

***

## Output

| Month | Sales | Two Months Later |
| ----- | ----: | ---------------: |
| Jan   |   100 |              200 |
| Feb   |   150 |              180 |
| Mar   |   200 |             NULL |
| Apr   |   180 |             NULL |

SQL now looks

```
2 Rows Ahead
```

instead of one.

***

## Example Using Default Value

```sql
SELECT
    month,
    sales,
    LEAD(sales,1,0)
    OVER(
        ORDER BY month
    ) AS next_month_sales
FROM monthly_sales;
```

***

## Output

| Month | Sales | Next Month Sales |
| ----- | ----: | ---------------: |
| Jan   |   100 |              150 |
| Feb   |   150 |              200 |
| Mar   |   200 |              180 |
| Apr   |   180 |                0 |

Instead of

```
NULL
```

SQL returns

```
0
```

for the final row.

***

## Real Business Example

Suppose a company wants to know

how much sales will change next month.

```sql
SELECT
    month,
    sales,
    LEAD(sales)
        OVER(
            ORDER BY month
        ) AS next_month_sales,
    LEAD(sales)
        OVER(
            ORDER BY month
        ) - sales AS expected_change
FROM monthly_sales;
```

***

## Output

| Month | Sales | Next Month | Expected Change |
| ----- | ----: | ---------: | --------------: |
| Jan   |   100 |        150 |              50 |
| Feb   |   150 |        200 |              50 |
| Mar   |   200 |        180 |             -20 |
| Apr   |   180 |       NULL |            NULL |

***

## Difference Between LAG() and LEAD()

| `LAG()`                | `LEAD()`           |
| ---------------------- | ------------------ |
| Looks backward         | Looks forward      |
| Retrieves previous row | Retrieves next row |
| Previous month's sales | Next month's sales |
| Yesterday's value      | Tomorrow's value   |

***

## Visual Comparison

#### LAG()

```
Previous

←

Current
```

***

#### LEAD()

```
Current

→

Next
```

***

## Real-World Uses

#### Sales Forecasting

Compare this month's sales with next month's sales.

***

#### Project Management

Show the next milestone date.

***

#### Employee Scheduling

View the next shift.

***

#### Delivery Tracking

Identify the next delivery status.

***

#### Manufacturing

Compare the next production cycle.

***

## Mental Model

```
Current Row

↓

Look Forward

↓

Next Row

↓

Return Value
```

***

## Key Takeaways

* `LEAD()` retrieves data from a future row.
* It does not perform calculations.
* `ORDER BY` defines what "next" means.
* The default offset is 1.
* If there is no next row, `NULL` is returned unless a default value is provided.
* `LEAD()` is commonly used for forecasting, planning, and future-row comparisons.

```sql
%%sql
-- If we havent add any args in OVER() by default it will take the next row data
select customer_id,sale_date,amount, 
LEAD(sale_date) over() as next_data
from sales;
```

```sql
%%sql
-- with this we can find how frequent the customers are ordering 

select customer_id,sale_date,amount,
LEAD(sale_date) over(
    partition by customer_id
    order by sale_date
) as nextd_date,
datediff(
    LEAD(sale_date) over(
    partition by customer_id
    order by sale_date
), sale_date
)as day_until_next
from sales;
```

```sql
%%sql

select customer_id,sale_date,amount,
LEAD(amount,1,'No Further Order') OVER(
    partition by customer_id
) next_order_amount
from sales;
```

## Chapter 15 — `FIRST_VALUE()`

So far, we have learned two Value Window Functions.

* `LAG()` → Look backward.
* `LEAD()` → Look forward.

Now we'll learn another Value Window Function.

### `FIRST_VALUE()`

Instead of looking backward or forward,

`FIRST_VALUE()` always returns

the **value from the first row** in the window.

***

## 15.1 What is `FIRST_VALUE()`?

### Definition

`FIRST_VALUE()` returns the value from the **first row** of the current window.

The "first row" is determined by the `ORDER BY` clause inside the window.

It does **not** calculate anything.

It simply copies the value from the first row to every row in the window.

***

## Syntax

```sql
FIRST_VALUE(expression)
OVER(
    PARTITION BY column_name
    ORDER BY column_name
)
```

***

## Syntax Breakdown

### expression

The column whose first value you want.

Example

```sql
salary
```

***

### PARTITION BY

Optional.

Creates separate windows.

Each partition gets its own first value.

***

### ORDER BY

Very important.

It decides

which row becomes

```
First
```

Changing the sort order changes the result.

***

## Think Like SQL

Suppose monthly sales are

| Month | Sales |
| ----- | ----: |
| Jan   |   100 |
| Feb   |   150 |
| Mar   |   200 |
| Apr   |   180 |

SQL sorts the rows.

```
Jan

↓

Feb

↓

Mar

↓

Apr
```

The first row is

```
Jan

↓

100
```

SQL copies

```
100
```

to every row.

***

## Result

| Month | Sales | First Sales |
| ----- | ----: | ----------: |
| Jan   |   100 |         100 |
| Feb   |   150 |         100 |
| Mar   |   200 |         100 |
| Apr   |   180 |         100 |

***

## Query

```sql
SELECT
    month,
    sales,
    FIRST_VALUE(sales)
    OVER(
        ORDER BY month
    ) AS first_sales
FROM monthly_sales;
```

***

## How SQL Thinks

Current Window

```
Jan

↓

Feb

↓

Mar

↓

Apr
```

First row

```
Jan

↓

100
```

Return

```
100

↓

100

↓

100

↓

100
```

***

## Example Using DESC

Now change the order.

```sql
SELECT
    month,
    sales,
    FIRST_VALUE(sales)
    OVER(
        ORDER BY sales DESC
    ) AS highest_sales
FROM monthly_sales;
```

***

## Output

| Month | Sales | Highest Sales |
| ----- | ----: | ------------: |
| Mar   |   200 |           200 |
| Apr   |   180 |           200 |
| Feb   |   150 |           200 |
| Jan   |   100 |           200 |

SQL sorts

```
200

↓

180

↓

150

↓

100
```

The first row becomes

```
200
```

Therefore every row receives

```
200
```

***

## Example Using PARTITION BY

Suppose sales belong to different departments.

| Department | Month | Sales |
| ---------- | ----- | ----: |
| IT         | Jan   |   100 |
| IT         | Feb   |   150 |
| IT         | Mar   |   200 |
| HR         | Jan   |    80 |
| HR         | Feb   |   120 |
| HR         | Mar   |   140 |

***

```sql
SELECT
    department,
    month,
    sales,
    FIRST_VALUE(sales)
    OVER(
        PARTITION BY department
        ORDER BY month
    ) AS first_month_sales
FROM department_sales;
```

***

## Output

| Department | Month | Sales | First Month Sales |
| ---------- | ----- | ----: | ----------------: |
| IT         | Jan   |   100 |               100 |
| IT         | Feb   |   150 |               100 |
| IT         | Mar   |   200 |               100 |
| HR         | Jan   |    80 |                80 |
| HR         | Feb   |   120 |                80 |
| HR         | Mar   |   140 |                80 |

Notice

The first value is calculated separately

for every department.

***

## Difference Between MIN() and FIRST\_VALUE()

Many beginners think

```
FIRST_VALUE()

=

MIN()
```

This is **not always true**.

#### MIN()

Returns

```
Smallest Value
```

***

#### FIRST\_VALUE()

Returns

```
Value From First Row

After ORDER BY
```

***

Example

Suppose products are sorted

by

```
Launch Date
```

| Product | Launch Date | Price |
| ------- | ----------- | ----: |
| A       | Jan         |   500 |
| B       | Feb         |   400 |
| C       | Mar         |   300 |

```sql
FIRST_VALUE(price)
OVER(
    ORDER BY launch_date
)
```

Returns

```
500
```

because Product A was launched first.

But

```sql
MIN(price)
```

returns

```
300
```

because it is the smallest price.

These are different business questions.

***

## Real-World Uses

#### Baseline Sales

Compare every month with the first month's sales.

***

#### Stock Market

Compare every price with the opening price.

***

#### Employee Salary

Compare current salary with initial salary.

***

#### Product Analysis

Compare current price with launch price.

***

#### Project Management

Compare current progress with the first milestone.

***

## Mental Model

```
Sort Rows

↓

Find First Row

↓

Take Value

↓

Repeat For

Every Row
```

***

## Key Takeaways

* `FIRST_VALUE()` returns the value from the first row in the window.
* The first row is determined by `ORDER BY`.
* `PARTITION BY` creates independent windows.
* It does not calculate the minimum or maximum value.
* It is commonly used to compare current values with the initial value.

```sql
%%sql
-- Partition by customer with there first value
select sale_id,customer_id,amount,
FIRST_VALUE(amount) OVER(
    partition by  customer_id
) as fst_val
from sales;
```

## Chapter 16 — `LAST_VALUE()`

In the previous chapter, we learned

* `FIRST_VALUE()` → Returns the value from the first row.

Now we'll learn its opposite.

### `LAST_VALUE()`

At first glance,

you might think

> "It returns the last value."

This is **partially true**.

There is an important concept that every SQL developer must understand.

By default,

`LAST_VALUE()` usually returns the **current row's value**, not the final row of the partition.

Later in this chapter, we'll learn why.

***

## 16.1 What is `LAST_VALUE()`?

### Definition

`LAST_VALUE()` returns the value from the **last row of the current window frame**.

The last row is determined by

* `ORDER BY`
* The Window Frame

Unlike `FIRST_VALUE()`,

`LAST_VALUE()` depends on the window frame.

***

## Syntax

```sql
LAST_VALUE(expression)
OVER(
    PARTITION BY column_name
    ORDER BY column_name
)
```

***

## First Example

Monthly sales

| Month | Sales |
| ----- | ----: |
| Jan   |   100 |
| Feb   |   150 |
| Mar   |   200 |
| Apr   |   180 |

***

Query

```sql
SELECT
    month,
    sales,
    LAST_VALUE(sales)
    OVER(
        ORDER BY month
    ) AS last_sales
FROM monthly_sales;
```

***

## Output

| Month | Sales | LAST\_VALUE |
| ----- | ----: | ----------: |
| Jan   |   100 |         100 |
| Feb   |   150 |         150 |
| Mar   |   200 |         200 |
| Apr   |   180 |         180 |

Many beginners expect

```
180

180

180

180
```

But SQL returns

```
100

150

200

180
```

Why?

***

## Understanding the Window Frame

When an `ORDER BY` clause is present,

most SQL databases use the default window frame:

```sql
RANGE BETWEEN UNBOUNDED PRECEDING
AND CURRENT ROW
```

This means

```
Start

↓

First Row

↓

Current Row
```

Notice

The window **ends at the current row**.

Therefore,

for every row,

the current row becomes

the last row of its frame.

***

## How SQL Thinks

Current Row

```
Jan
```

Window

```
Jan
```

Last row

```
Jan
```

Return

```
100
```

***

Current Row

```
Feb
```

Window

```
Jan

↓

Feb
```

Last row

```
Feb
```

Return

```
150
```

***

Current Row

```
Mar
```

Window

```
Jan

↓

Feb

↓

Mar
```

Last row

```
Mar
```

Return

```
200
```

***

Current Row

```
Apr
```

Window

```
Jan

↓

Feb

↓

Mar

↓

Apr
```

Last row

```
Apr
```

Return

```
180
```

***

## How to Get the Actual Last Value

To include the **entire partition**,

we must explicitly define the window frame.

```sql
SELECT
    month,
    sales,
    LAST_VALUE(sales)
    OVER(
        ORDER BY month
        ROWS BETWEEN UNBOUNDED PRECEDING
        AND UNBOUNDED FOLLOWING
    ) AS final_sales
FROM monthly_sales;
```

***

## Output

| Month | Sales | Final Sales |
| ----- | ----: | ----------: |
| Jan   |   100 |         180 |
| Feb   |   150 |         180 |
| Mar   |   200 |         180 |
| Apr   |   180 |         180 |

Now SQL can see

the complete partition.

The last row is

```
Apr

↓

180
```

Therefore every row receives

```
180
```

***

## Example Using DESC

```sql
SELECT
    month,
    sales,
    LAST_VALUE(sales)
    OVER(
        ORDER BY sales DESC
        ROWS BETWEEN UNBOUNDED PRECEDING
        AND UNBOUNDED FOLLOWING
    ) AS lowest_sales
FROM monthly_sales;
```

***

Output

| Month | Sales | Lowest Sales |
| ----- | ----: | -----------: |
| Mar   |   200 |          100 |
| Apr   |   180 |          100 |
| Feb   |   150 |          100 |
| Jan   |   100 |          100 |

Why?

SQL sorts

```
200

↓

180

↓

150

↓

100
```

The last row is

```
100
```

Therefore every row receives

```
100
```

***

## Example Using PARTITION BY

Department sales

| Department | Month | Sales |
| ---------- | ----- | ----: |
| IT         | Jan   |   100 |
| IT         | Feb   |   150 |
| IT         | Mar   |   200 |
| HR         | Jan   |    80 |
| HR         | Feb   |   120 |
| HR         | Mar   |   140 |

***

```sql
SELECT
    department,
    month,
    sales,
    LAST_VALUE(sales)
    OVER(
        PARTITION BY department
        ORDER BY month
        ROWS BETWEEN UNBOUNDED PRECEDING
        AND UNBOUNDED FOLLOWING
    ) AS last_sales
FROM department_sales;
```

***

Output

| Department | Month | Sales | Last Sales |
| ---------- | ----- | ----: | ---------: |
| IT         | Jan   |   100 |        200 |
| IT         | Feb   |   150 |        200 |
| IT         | Mar   |   200 |        200 |
| HR         | Jan   |    80 |        140 |
| HR         | Feb   |   120 |        140 |
| HR         | Mar   |   140 |        140 |

Each department gets its own last value.

***

## FIRST\_VALUE() vs LAST\_VALUE()

| Function        | Returns                              |
| --------------- | ------------------------------------ |
| `FIRST_VALUE()` | First row in the window              |
| `LAST_VALUE()`  | Last row in the current window frame |

When using `LAST_VALUE()`, remember that the default window frame often ends at the current row. To retrieve the true last value of the partition, extend the frame with:

```sql
ROWS BETWEEN UNBOUNDED PRECEDING
AND UNBOUNDED FOLLOWING
```

***

## Real-World Uses

#### Compare with Final Sales

Compare each month's sales with the last month's sales.

***

#### Compare with Final Salary

Compare an employee's earlier salary with their latest salary.

***

#### Stock Analysis

Compare daily prices with the closing price.

***

#### Manufacturing

Compare each production stage with the final production output.

***

#### Project Tracking

Compare each milestone with the project's final status.

***

## Mental Model

```
FIRST_VALUE()

↓

Beginning

-----------------------

LAST_VALUE()

↓

End Of Window Frame

↓

(Usually Current Row)

↓

Unless Window Frame

Includes Entire Partition
```

***

## Key Takeaways

* `LAST_VALUE()` returns the value from the last row of the current window frame.
* With the default frame, it often returns the current row's value.
* To retrieve the actual last row of the partition, specify:

```sql
ROWS BETWEEN UNBOUNDED PRECEDING
AND UNBOUNDED FOLLOWING
```

* `ORDER BY` determines the row order.
* `PARTITION BY` creates independent windows.
* Understanding window frames is essential for using `LAST_VALUE()` correctly.

```sql
%%sql
-- Partition by customer with there first value
-- by default it will take the last value. Or we can use order by
select sale_id,customer_id,amount,
LAST_VALUE(amount) OVER(
    partition by  customer_id
) as fst_val
from sales;
```

## Chapter 17 — `NTH_VALUE()`

So far, we have learned

* `FIRST_VALUE()` → First row
* `LAST_VALUE()` → Last row

But what if the business asks

> "Show me the **3rd highest salary**."

or

> "Return the **5th sale**."

or

> "What was the **2nd month's sales**?"

Neither `FIRST_VALUE()` nor `LAST_VALUE()` can answer these questions.

This is where `NTH_VALUE()` is useful.

***

## 17.1 What is `NTH_VALUE()`?

### Definition

`NTH_VALUE()` returns the value from the **Nth row** of the current window frame.

You decide

which row

by specifying

```
N
```

Examples

```
N = 1

↓

First Row
```

```
N = 2

↓

Second Row
```

```
N = 3

↓

Third Row
```

Unlike `FIRST_VALUE()` and `LAST_VALUE()`,

`NTH_VALUE()` can return **any position** inside the window.

***

## Syntax

```sql
NTH_VALUE(
    expression,
    N
)
OVER(
    PARTITION BY column_name
    ORDER BY column_name
    ROWS BETWEEN UNBOUNDED PRECEDING
    AND UNBOUNDED FOLLOWING
)
```

***

## Syntax Breakdown

### expression

The column whose value you want.

Example

```sql
salary
```

***

### N

The row position.

Examples

```
1

↓

First Row
```

```
2

↓

Second Row
```

```
5

↓

Fifth Row
```

***

### ORDER BY

Determines

which row is

1st,

2nd,

3rd,

and so on.

***

### Window Frame

Like `LAST_VALUE()`,

`NTH_VALUE()` depends on the window frame.

To access the complete partition,

we usually write

```sql
ROWS BETWEEN UNBOUNDED PRECEDING
AND UNBOUNDED FOLLOWING
```

***

## Employee Table

| Employee | Salary |
| -------- | -----: |
| Divya    |  70000 |
| John     |  60000 |
| Meena    |  60000 |
| Arun     |  55000 |
| Priya    |  45000 |
| Kumar    |  40000 |
| Ravi     |  35000 |
| Ram      |  30000 |

***

## Example 1

Return

the **2nd highest salary**.

```sql
SELECT
    emp_name,
    salary,
    NTH_VALUE(salary,2)
    OVER(
        ORDER BY salary DESC
        ROWS BETWEEN UNBOUNDED PRECEDING
        AND UNBOUNDED FOLLOWING
    ) AS second_highest
FROM employee;
```

***

## Output

| Employee | Salary | Second Highest |
| -------- | -----: | -------------: |
| Divya    |  70000 |          60000 |
| John     |  60000 |          60000 |
| Meena    |  60000 |          60000 |
| Arun     |  55000 |          60000 |
| Priya    |  45000 |          60000 |
| Kumar    |  40000 |          60000 |
| Ravi     |  35000 |          60000 |
| Ram      |  30000 |          60000 |

***

## How SQL Thinks

Sort the salaries.

```
70000

↓

60000

↓

60000

↓

55000

↓

45000

↓

40000

↓

35000

↓

30000
```

Now SQL asks

```
What is

Row Number

2 ?
```

Answer

```
60000
```

Return

```
60000

↓

60000

↓

60000

↓

...
```

to every row.

***

## Example 2

Return

the **4th highest salary**.

```sql
SELECT
    emp_name,
    salary,
    NTH_VALUE(salary,4)
    OVER(
        ORDER BY salary DESC
        ROWS BETWEEN UNBOUNDED PRECEDING
        AND UNBOUNDED FOLLOWING
    ) AS fourth_highest
FROM employee;
```

***

## Output

| Employee | Salary | Fourth Highest |
| -------- | -----: | -------------: |
| Divya    |  70000 |          55000 |
| John     |  60000 |          55000 |
| Meena    |  60000 |          55000 |
| Arun     |  55000 |          55000 |
| Priya    |  45000 |          55000 |
| Kumar    |  40000 |          55000 |
| Ravi     |  35000 |          55000 |
| Ram      |  30000 |          55000 |

***

## Example Using PARTITION BY

Department salaries

| Department | Employee | Salary |
| ---------- | -------- | -----: |
| IT         | Divya    |  70000 |
| IT         | Kumar    |  40000 |
| IT         | Ram      |  30000 |
| HR         | Arun     |  55000 |
| HR         | Priya    |  45000 |
| HR         | Ravi     |  35000 |

***

```sql
SELECT
    department,
    emp_name,
    salary,
    NTH_VALUE(salary,2)
    OVER(
        PARTITION BY department
        ORDER BY salary DESC
        ROWS BETWEEN UNBOUNDED PRECEDING
        AND UNBOUNDED FOLLOWING
    ) AS second_highest
FROM employee;
```

***

## Output

| Department | Employee | Salary | Second Highest |
| ---------- | -------- | -----: | -------------: |
| IT         | Divya    |  70000 |          40000 |
| IT         | Kumar    |  40000 |          40000 |
| IT         | Ram      |  30000 |          40000 |
| HR         | Arun     |  55000 |          45000 |
| HR         | Priya    |  45000 |          45000 |
| HR         | Ravi     |  35000 |          45000 |

Each department gets

its own

Nth value.

***

## Important Note

Like `LAST_VALUE()`,

`NTH_VALUE()` depends on the **window frame**.

Without

```sql
ROWS BETWEEN UNBOUNDED PRECEDING
AND UNBOUNDED FOLLOWING
```

many databases return

```
NULL
```

or unexpected values

for the earlier rows,

because the Nth row may not yet exist in the current frame.

***

## Difference Between FIRST\_VALUE(), LAST\_VALUE() and NTH\_VALUE()

| Function        | Returns                      |
| --------------- | ---------------------------- |
| `FIRST_VALUE()` | First row                    |
| `LAST_VALUE()`  | Last row of the window frame |
| `NTH_VALUE()`   | Nth row of the window frame  |

Think of them as one family.

```
FIRST_VALUE()

↓

1st Row

---------------------

NTH_VALUE(3)

↓

3rd Row

---------------------

LAST_VALUE()

↓

Last Row
```

***

## Real-World Uses

#### Salary Analysis

Retrieve the 2nd, 3rd or 5th highest salary.

***

#### Sales Reports

Compare every sale with the 10th sale.

***

#### Stock Market

Compare prices with the opening, middle or closing price.

***

#### Manufacturing

Retrieve the value at a specific production stage.

***

#### Time-Series Analysis

Compare today's value with the value from a specific point in the timeline.

***

## Mental Model

```
Sort Rows

↓

Find

Nth Row

↓

Take Value

↓

Repeat

For Every Row
```

***

## Key Takeaways

* `NTH_VALUE()` returns the value from the Nth row of the current window frame.
* The row position is determined by `ORDER BY`.
* Like `LAST_VALUE()`, it depends on the window frame.
* To retrieve the Nth value from the entire partition, specify:

```sql
ROWS BETWEEN UNBOUNDED PRECEDING
AND UNBOUNDED FOLLOWING
```

* `PARTITION BY` creates separate windows.
* It is useful when the business needs a specific positional value rather than the first or last.

```sql
%%sql
-- select only the second row
select *,
NTH_VALUE(salary,2) OVER(
    
) as second_row_salary

from employee;
```
