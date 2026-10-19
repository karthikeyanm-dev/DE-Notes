# CTE

> Source notebook: [DE SQL Notes/CTE.ipynb](https://github.com/karthikeyanm-dev/DE-Notes/blob/main/DE%20SQL%20Notes/CTE.ipynb). Notebook outputs are omitted; open the source to run cells.

```sql
%load_ext sql
%sql mysql+mysqlconnector://root:123456@localhost:3306
```

```sql
%%sql

show databases;
```

```sql
%%sql
use tp_930_de;
```

```sql
%%sql
show tables
```

```sql
%%sql
DROP TABLE IF EXISTS employees;
```

```sql
%%sql
CREATE TABLE employees (
    employee_id      INT PRIMARY KEY,
    employee_name    VARCHAR(100) NOT NULL,
    department       VARCHAR(50) NOT NULL,
    job_title        VARCHAR(100),
    manager_id       INT,
    salary           DECIMAL(12,2),
    joining_date     DATE,
    employment_status VARCHAR(20)
);
```

```sql
%%sql
INSERT INTO employees
(employee_id, employee_name, department, job_title, manager_id, salary, joining_date, employment_status)
VALUES
(1, 'Arun Kumar', 'Executive', 'CEO', NULL, 250000, '2015-01-10', 'Active'),
(2, 'Priya Sharma', 'Technology', 'CTO', 1, 220000, '2016-03-15', 'Active'),
(3, 'Rahul Verma', 'Finance', 'CFO', 1, 210000, '2016-05-20', 'Active'),
(4, 'Sneha Iyer', 'Human Resources', 'CHRO', 1, 200000, '2017-02-11', 'Active'),
(5, 'Vikram Singh', 'Sales', 'VP Sales', 1, 195000, '2017-06-18', 'Active'),
(6, 'Karthik Rao', 'Technology', 'Engineering Manager', 2, 150000, '2018-01-08', 'Active'),
(7, 'Meena Krishnan', 'Technology', 'Engineering Manager', 2, 148000, '2018-04-12', 'Active'),
(8, 'Suresh Babu', 'Technology', 'Data Engineering Manager', 2, 155000, '2018-07-19', 'Active'),
(9, 'Divya Nair', 'Finance', 'Finance Manager', 3, 135000, '2019-01-22', 'Active'),
(10, 'Ramesh Gupta', 'Finance', 'Accounting Manager', 3, 130000, '2019-03-18', 'Active'),
(11, 'Anita Das', 'Human Resources', 'HR Manager', 4, 125000, '2019-05-10', 'Active'),
(12, 'Manoj Patel', 'Sales', 'Sales Manager', 5, 120000, '2019-08-01', 'Active'),
(13, 'Lakshmi Menon', 'Sales', 'Regional Sales Manager', 5, 118000, '2020-01-14', 'Active'),
(14, 'Ajay Kumar', 'Technology', 'Senior Data Engineer', 8, 110000, '2020-02-17', 'Active'),
(15, 'Pooja Shah', 'Technology', 'Senior Data Engineer', 8, 108000, '2020-03-20', 'Active'),
(16, 'Vijay Anand', 'Technology', 'Data Engineer', 8, 95000, '2021-01-05', 'Active'),
(17, 'Neha Reddy', 'Technology', 'Data Engineer', 8, 92000, '2021-02-10', 'Active'),
(18, 'Rohit Jain', 'Technology', 'Data Engineer', 8, 90000, '2021-04-15', 'Active'),
(19, 'Swathi Rao', 'Technology', 'Data Analyst', 7, 85000, '2021-06-11', 'Active'),
(20, 'Gopal Krishna', 'Technology', 'Software Engineer', 6, 88000, '2021-07-19', 'Active'),
(21, 'Harish Kumar', 'Technology', 'Software Engineer', 6, 86000, '2021-08-23', 'Active'),
(22, 'Deepa Mohan', 'Technology', 'Software Engineer', 6, 84000, '2021-09-13', 'Active'),
(23, 'Naveen Raj', 'Technology', 'Software Engineer', 6, 82000, '2022-01-17', 'Active'),
(24, 'Asha Devi', 'Technology', 'Software Engineer', 6, 80000, '2022-02-28', 'Active'),
(25, 'Sanjay Rao', 'Technology', 'Software Engineer', 7, 81000, '2022-03-21', 'Active'),
(26, 'Bhavya Sri', 'Technology', 'Software Engineer', 7, 79000, '2022-04-18', 'Active'),
(27, 'Dinesh Kumar', 'Technology', 'QA Engineer', 6, 76000, '2022-05-16', 'Active'),
(28, 'Keerthi S', 'Technology', 'QA Engineer', 6, 74000, '2022-06-20', 'Active'),
(29, 'Mohan Das', 'Technology', 'DevOps Engineer', 7, 90000, '2022-07-11', 'Active'),
(30, 'Preethi Nair', 'Technology', 'Cloud Engineer', 7, 93000, '2022-08-15', 'Active'),
(31, 'Ravi Shankar', 'Technology', 'Data Engineer', 8, 87000, '2022-09-19', 'Active'),
(32, 'Shalini Rao', 'Technology', 'Data Analyst', 8, 83000, '2022-10-17', 'Active'),
(33, 'Manish Verma', 'Technology', 'Software Engineer', 6, 85000, '2022-11-14', 'Active'),
(34, 'Geetha R', 'Technology', 'Software Engineer', 7, 82000, '2022-12-12', 'Active'),
(35, 'Sathish Kumar', 'Technology', 'Data Engineer', 8, 91000, '2023-01-16', 'Active'),
(36, 'Ishita Roy', 'Technology', 'Data Analyst', 8, 78000, '2023-02-20', 'Active'),
(37, 'Abhishek Singh', 'Technology', 'Software Engineer', 6, 88000, '2023-03-13', 'Active'),
(38, 'Nithya Lakshmi', 'Technology', 'QA Engineer', 7, 75000, '2023-04-17', 'Active'),
(39, 'Varun Kumar', 'Technology', 'DevOps Engineer', 7, 92000, '2023-05-15', 'Active'),
(40, 'Reshma P', 'Technology', 'Data Engineer', 8, 89000, '2023-06-19', 'Active'),
(41, 'Ganesh B', 'Finance', 'Senior Accountant', 10, 90000, '2020-02-03', 'Active'),
(42, 'Anjali Menon', 'Finance', 'Accountant', 10, 72000, '2021-01-11', 'Active'),
(43, 'Surya Prakash', 'Finance', 'Accountant', 10, 70000, '2021-05-17', 'Active'),
(44, 'Swetha R', 'Finance', 'Financial Analyst', 9, 82000, '2021-08-09', 'Active'),
(45, 'Vishal Gupta', 'Finance', 'Financial Analyst', 9, 80000, '2022-01-10', 'Active'),
(46, 'Monika Jain', 'Finance', 'Accountant', 10, 68000, '2022-03-14', 'Active'),
(47, 'Suresh Reddy', 'Finance', 'Accountant', 10, 66000, '2022-05-09', 'Active'),
(48, 'Divakar Rao', 'Finance', 'Financial Analyst', 9, 76000, '2022-07-18', 'Active'),
(49, 'Ritu Sharma', 'Finance', 'Accountant', 10, 69000, '2023-01-23', 'Active'),
(50, 'Nikhil Kumar', 'Finance', 'Financial Analyst', 9, 78000, '2023-04-10', 'Active'),
(51, 'Ramesh B', 'Human Resources', 'HR Executive', 11, 65000, '2020-06-15', 'Active'),
(52, 'Kavya Iyer', 'Human Resources', 'Recruiter', 11, 62000, '2021-02-08', 'Active'),
(53, 'Siva Kumar', 'Human Resources', 'Recruiter', 11, 60000, '2021-07-12', 'Active'),
(54, 'Nandhini S', 'Human Resources', 'HR Analyst', 11, 68000, '2022-01-17', 'Active'),
(55, 'Ashwin Kumar', 'Human Resources', 'HR Executive', 11, 61000, '2022-05-16', 'Active'),
(56, 'Madhavi R', 'Human Resources', 'Recruiter', 11, 63000, '2022-09-19', 'Active'),
(57, 'Prakash M', 'Human Resources', 'HR Executive', 11, 59000, '2023-01-09', 'Active'),
(58, 'Sneha R', 'Human Resources', 'HR Analyst', 11, 67000, '2023-04-17', 'Active'),
(59, 'Kiran Kumar', 'Human Resources', 'Recruiter', 11, 64000, '2023-06-12', 'Active'),
(60, 'Aparna Devi', 'Human Resources', 'HR Executive', 11, 60000, '2023-08-14', 'Active'),
(61, 'Suresh Kumar', 'Sales', 'Sales Executive', 12, 70000, '2020-01-13', 'Active'),
(62, 'Divya Sharma', 'Sales', 'Sales Executive', 12, 68000, '2020-04-20', 'Active'),
(63, 'Aravind R', 'Sales', 'Sales Executive', 12, 66000, '2021-01-18', 'Active'),
(64, 'Pavithra S', 'Sales', 'Sales Executive', 12, 65000, '2021-06-14', 'Active'),
(65, 'Vignesh Kumar', 'Sales', 'Sales Executive', 12, 64000, '2021-09-20', 'Active'),
(66, 'Harini P', 'Sales', 'Sales Executive', 13, 63000, '2022-01-10', 'Active'),
(67, 'Bala Murugan', 'Sales', 'Sales Executive', 13, 62000, '2022-03-14', 'Active'),
(68, 'Ranjith Kumar', 'Sales', 'Sales Executive', 13, 61000, '2022-05-16', 'Active'),
(69, 'Swetha Devi', 'Sales', 'Sales Executive', 13, 60000, '2022-07-18', 'Active'),
(70, 'Karthika R', 'Sales', 'Sales Executive', 13, 59000, '2022-09-19', 'Active'),
(71, 'Lokesh M', 'Sales', 'Sales Executive', 12, 61000, '2023-01-16', 'Active'),
(72, 'Megha S', 'Sales', 'Sales Executive', 12, 60000, '2023-03-20', 'Active'),
(73, 'Suraj Kumar', 'Sales', 'Sales Executive', 13, 58000, '2023-05-15', 'Active'),
(74, 'Aishwarya R', 'Sales', 'Sales Executive', 13, 57000, '2023-07-17', 'Active'),
(75, 'Rahul K', 'Sales', 'Sales Executive', 12, 59000, '2023-09-18', 'Active'),
(76, 'John Mathew', 'Operations', 'Operations Manager', 1, 135000, '2019-02-11', 'Active'),
(77, 'Sonia George', 'Operations', 'Operations Analyst', 76, 78000, '2020-03-16', 'Active'),
(78, 'Amit Joshi', 'Operations', 'Operations Analyst', 76, 76000, '2020-07-20', 'Active'),
(79, 'Farhan Ali', 'Operations', 'Operations Analyst', 76, 75000, '2021-01-11', 'Active'),
(80, 'Maria Thomas', 'Operations', 'Operations Analyst', 76, 74000, '2021-05-17', 'Active'),
(81, 'Rakesh Kumar', 'Operations', 'Operations Analyst', 76, 72000, '2022-02-14', 'Active'),
(82, 'Anu Joseph', 'Operations', 'Operations Analyst', 76, 70000, '2022-04-18', 'Active'),
(83, 'Naveena R', 'Operations', 'Operations Analyst', 76, 68000, '2022-06-20', 'Active'),
(84, 'Tarun Singh', 'Operations', 'Operations Analyst', 76, 69000, '2022-08-15', 'Active'),
(85, 'Irfan Khan', 'Operations', 'Operations Analyst', 76, 71000, '2023-01-16', 'Active'),
(86, 'Meena S', 'Operations', 'Operations Analyst', 76, 67000, '2023-03-20', 'Active'),
(87, 'Rohit S', 'Operations', 'Operations Analyst', 76, 66000, '2023-05-15', 'Active'),
(88, 'Pooja R', 'Operations', 'Operations Analyst', 76, 65000, '2023-07-17', 'Active'),
(89, 'Sanjay P', 'Operations', 'Operations Analyst', 76, 64000, '2023-09-18', 'Active'),
(90, 'Nisha Kumar', 'Operations', 'Operations Analyst', 76, 63000, '2023-10-16', 'Active'),
(91, 'Akhil Raj', 'Technology', 'Intern', 20, 30000, '2024-01-08', 'Active'),
(92, 'Bhargav S', 'Technology', 'Intern', 21, 30000, '2024-01-15', 'Active'),
(93, 'Chandana R', 'Finance', 'Intern', 41, 28000, '2024-02-12', 'Active'),
(94, 'Darshan K', 'Human Resources', 'Intern', 52, 28000, '2024-02-19', 'Active'),
(95, 'Esha P', 'Sales', 'Intern', 61, 27000, '2024-03-11', 'Active'),
(96, 'Faisal M', 'Operations', 'Intern', 77, 27000, '2024-03-18', 'Active'),
(97, 'Gowtham R', 'Technology', 'Data Engineer', 8, 88000, '2024-04-15', 'Active'),
(98, 'Hema L', 'Technology', 'Data Analyst', 8, 82000, '2024-05-20', 'Active'),
(99, 'Imran K', 'Sales', 'Sales Executive', 12, 60000, '2024-06-17', 'Active'),
(100, 'Janani S', 'Finance', 'Accountant', 10, 67000, '2024-07-15', 'Active');
```

## 6. What Is a CTE?

CTE stands for:

> Common Table Expression

A CTE is a temporary named result set that exists only for the duration of a single SQL statement.

Basic syntax:

```sql
WITH cte_name AS (
    SELECT ...
)
SELECT *
FROM cte_name;
```

Think of it as:

```
Complex SQL logic
       ↓
Give the intermediate result a name
       ↓
Reuse that result in the main query
```

Example:

```sql
WITH high_salary_employees AS (
    SELECT
        employee_id,
        employee_name,
        department,
        salary
    FROM employees
    WHERE salary > 100000
)
SELECT *
FROM high_salary_employees;
```

The CTE is called:

```
high_salary_employees
```

The CTE produces an intermediate result.

The final `SELECT` reads from that result.

***

## 7. Why Do We Need CTEs?

Without a CTE:

```sql
SELECT *
FROM (
    SELECT
        employee_id,
        employee_name,
        department,
        salary
    FROM employees
    WHERE salary > 100000
) AS high_salary_employees;
```

With a CTE:

```sql
WITH high_salary_employees AS (
    SELECT
        employee_id,
        employee_name,
        department,
        salary
    FROM employees
    WHERE salary > 100000
)
SELECT *
FROM high_salary_employees;
```

Both can produce the same result.

The CTE is usually easier to read and maintain.

***

## 8. CTE Mental Model

Imagine a data pipeline:

```
employees
    ↓
filter active employees
    ↓
active_employees
    ↓
calculate department statistics
    ↓
department_stats
    ↓
filter high-performing departments
    ↓
final result
```

SQL:

```sql
WITH active_employees AS (
    ...
),
department_stats AS (
    ...
),
final_data AS (
    ...
)
SELECT *
FROM final_data;
```

This is one of the most useful ways for a data engineer to think about CTEs.

***

## 9. Simple CTE Example

Find employees earning more than 100,000.

```sql
WITH high_salary AS (
    SELECT
        employee_id,
        employee_name,
        department,
        salary
    FROM employees
    WHERE salary > 100000
)
SELECT *
FROM high_salary;
```

***

## 10. CTE With WHERE Clause

A `WHERE` clause can be used inside a CTE.

```sql
WITH technology_employees AS (
    SELECT *
    FROM employees
    WHERE department = 'Technology'
)
SELECT *
FROM technology_employees;
```

The filtering happens while creating the CTE result.

***

## 11. WHERE Clause Outside the CTE

You can also filter the CTE in the final query.

```sql
WITH all_employees AS (
    SELECT *
    FROM employees
)
SELECT *
FROM all_employees
WHERE salary > 100000;
```

Conceptually:

```
employees
   ↓
all_employees
   ↓
salary > 100000
   ↓
result
```

***

## 12. WHERE Inside vs Outside CTE

These can often produce the same result:

#### Option 1

```sql
WITH data AS (
    SELECT *
    FROM employees
    WHERE salary > 100000
)
SELECT *
FROM data;
```

#### Option 2

```sql
WITH data AS (
    SELECT *
    FROM employees
)
SELECT *
FROM data
WHERE salary > 100000;
```

The important difference is logical organization.

If the filter is part of the definition of the intermediate dataset, put it inside the CTE.

If the CTE represents a reusable logical dataset and the filter is a final requirement, putting it outside may be clearer.

Do not assume the two forms always have identical execution plans in every database.

***

## 13. Multiple CTEs

One of the biggest advantages of CTEs is that multiple CTEs can be chained.

Syntax:

```sql
WITH cte1 AS (
    ...
),
cte2 AS (
    ...
),
cte3 AS (
    ...
)
SELECT *
FROM cte3;
```

Example:

```sql
WITH active_employees AS (
    SELECT *
    FROM employees
    WHERE employment_status = 'Active'
),
technology_employees AS (
    SELECT *
    FROM active_employees
    WHERE department = 'Technology'
),
high_salary_technology AS (
    SELECT *
    FROM technology_employees
    WHERE salary > 90000
)
SELECT *
FROM high_salary_technology;
```

Logical flow:

```
employees
    ↓
active_employees
    ↓
technology_employees
    ↓
high_salary_technology
    ↓
final SELECT
```

***

## 14. CTE With GROUP BY

Find average salary by department.

```sql
WITH department_salary AS (
    SELECT
        department,
        COUNT(*) AS employee_count,
        AVG(salary) AS avg_salary,
        MAX(salary) AS max_salary,
        MIN(salary) AS min_salary
    FROM employees
    GROUP BY department
)
SELECT *
FROM department_salary
ORDER BY avg_salary DESC;
```

***

## 15. CTE With HAVING

Find departments whose average salary is greater than 80,000.

```sql
WITH department_salary AS (
    SELECT
        department,
        AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department
    HAVING AVG(salary) > 80000
)
SELECT *
FROM department_salary;
```

***

## 16. CTE With JOIN

Suppose we want to display each employee and their manager.

```sql
WITH employee_data AS (
    SELECT
        e.employee_id,
        e.employee_name,
        e.department,
        e.salary,
        e.manager_id
    FROM employees e
)
SELECT
    e.employee_name AS employee,
    m.employee_name AS manager
FROM employee_data e
LEFT JOIN employees m
    ON e.manager_id = m.employee_id;
```

***

## 17. Multiple CTEs + JOIN

```sql
WITH department_stats AS (
    SELECT
        department,
        COUNT(*) AS employee_count,
        AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department
),
high_paid_employees AS (
    SELECT
        employee_id,
        employee_name,
        department,
        salary
    FROM employees
    WHERE salary > 100000
)
SELECT
    h.employee_name,
    h.department,
    h.salary,
    d.employee_count,
    d.avg_salary
FROM high_paid_employees h
JOIN department_stats d
    ON h.department = d.department;
```

This is much easier to understand than putting every transformation into one huge query.

***

## 18. CTE With Window Functions

Find each employee's salary rank within their department.

```sql
WITH ranked_employees AS (
    SELECT
        employee_id,
        employee_name,
        department,
        salary,
        RANK() OVER (
            PARTITION BY department
            ORDER BY salary DESC
        ) AS salary_rank
    FROM employees
)
SELECT *
FROM ranked_employees
ORDER BY department, salary_rank;
```

***

## 19. CTE + Window Function + WHERE

A common real-world pattern is filtering the result of a window function.

You generally cannot use a window function directly in `WHERE` at the same query level.

This is problematic:

```sql
SELECT
    employee_id,
    employee_name,
    department,
    salary,
    RANK() OVER (
        PARTITION BY department
        ORDER BY salary DESC
    ) AS salary_rank
FROM employees
WHERE salary_rank <= 3;
```

Instead:

```sql
WITH ranked_employees AS (
    SELECT
        employee_id,
        employee_name,
        department,
        salary,
        RANK() OVER (
            PARTITION BY department
            ORDER BY salary DESC
        ) AS salary_rank
    FROM employees
)
SELECT *
FROM ranked_employees
WHERE salary_rank <= 3;
```

This is one of the most important practical uses of CTEs.

***

## 20. Top 3 Employees Per Department

```sql
WITH ranked_employees AS (
    SELECT
        employee_id,
        employee_name,
        department,
        salary,
        ROW_NUMBER() OVER (
            PARTITION BY department
            ORDER BY salary DESC
        ) AS rn
    FROM employees
)
SELECT *
FROM ranked_employees
WHERE rn <= 3
ORDER BY department, rn;
```

***

## 21. ROW\_NUMBER vs RANK vs DENSE\_RANK

### ROW\_NUMBER

Every row gets a unique number.

```sql
ROW_NUMBER() OVER (
    PARTITION BY department
    ORDER BY salary DESC
)
```

If salaries are:

```
100000
100000
90000
```

Result:

```
1
2
3
```

### RANK

Ties receive the same rank, with gaps.

```
1
1
3
```

### DENSE\_RANK

Ties receive the same rank, without gaps.

```
1
1
2
```

***

## 22. CTE for Duplicate Detection

Suppose a dataset has duplicate business records.

The same technique can be used to detect duplicates.

```sql
WITH duplicate_check AS (
    SELECT
        employee_name,
        department,
        COUNT(*) AS record_count
    FROM employees
    GROUP BY
        employee_name,
        department
)
SELECT *
FROM duplicate_check
WHERE record_count > 1;
```

***

## 23. CTE for Data Quality Checks

Example:

```sql
WITH invalid_employees AS (
    SELECT *
    FROM employees
    WHERE employee_name IS NULL
       OR department IS NULL
       OR salary <= 0
       OR joining_date IS NULL
)
SELECT *
FROM invalid_employees;
```

A data engineer can use this pattern in data-quality pipelines.

***

## 24. CTE for Transformation

Imagine raw data:

```
employees
```

We want:

```
1. Active employees
2. Salary classification
3. Department statistics
4. Final report
```

SQL:

```sql
WITH active_employees AS (
    SELECT *
    FROM employees
    WHERE employment_status = 'Active'
),
classified_employees AS (
    SELECT
        employee_id,
        employee_name,
        department,
        salary,
        CASE
            WHEN salary >= 100000 THEN 'High'
            WHEN salary >= 70000 THEN 'Medium'
            ELSE 'Low'
        END AS salary_band
    FROM active_employees
),
department_stats AS (
    SELECT
        department,
        COUNT(*) AS employee_count,
        AVG(salary) AS avg_salary
    FROM classified_employees
    GROUP BY department
)
SELECT *
FROM department_stats
ORDER BY avg_salary DESC;
```

This resembles a simple ELT transformation pipeline.

***

## 25. CTE in Data Engineering

Data engineers commonly work with pipelines like:

```
Source
  ↓
Raw Layer
  ↓
Cleaning
  ↓
Transformation
  ↓
Business Rules
  ↓
Aggregation
  ↓
Analytics Table
```

CTEs are useful for expressing the transformation stages.

Example:

```sql
WITH raw_data AS (
    SELECT *
    FROM employees
),
clean_data AS (
    SELECT *
    FROM raw_data
    WHERE employee_id IS NOT NULL
      AND salary > 0
),
business_data AS (
    SELECT
        employee_id,
        employee_name,
        department,
        salary,
        CASE
            WHEN salary >= 100000 THEN 'High'
            ELSE 'Standard'
        END AS salary_category
    FROM clean_data
)
SELECT *
FROM business_data;
```

***

## 26. CTE Is Not a Permanent Table

A CTE normally exists only for the duration of one SQL statement.

Example:

```sql
WITH data AS (
    SELECT *
    FROM employees
)
SELECT *
FROM data;
```

After this statement finishes:

```
data
```

does not remain as a permanent table.

You cannot normally do:

```sql
SELECT *
FROM data;
```

in a separate statement.

***

## 27. CTE vs Temporary Table

### CTE

```sql
WITH data AS (
    SELECT *
    FROM employees
)
SELECT *
FROM data;
```

Characteristics:

* Exists for one statement.
* Excellent for query organization.
* Good for multi-step transformations.
* No permanent database object is created.

### Temporary Table

```sql
CREATE TEMP TABLE temp_employees AS
SELECT *
FROM employees;
```

Characteristics:

* Exists beyond a single statement, depending on database/session behavior.
* Can usually be queried by multiple statements.
* Can often be indexed.
* Useful for intermediate data that needs to be reused across several statements.

***

## 28. CTE vs Subquery

Subquery:

```sql
SELECT *
FROM (
    SELECT *
    FROM employees
    WHERE salary > 100000
) x;
```

CTE:

```sql
WITH high_salary AS (
    SELECT *
    FROM employees
    WHERE salary > 100000
)
SELECT *
FROM high_salary;
```

Both are useful.

A CTE becomes especially valuable when:

* The query has multiple transformation stages.
* The intermediate result needs a meaningful name.
* Several CTEs need to be chained.
* The query contains complex window-function logic.
* Readability and maintainability matter.

***

## 29. CTE vs View

A CTE:

```sql
WITH data AS (...)
SELECT ...
```

is temporary for the statement.

A view:

```sql
CREATE VIEW employee_view AS
SELECT ...
FROM employees;
```

is a persistent database object.

Use a view when a logical query should be available to users or other queries repeatedly.

Use a CTE when the logic is primarily part of one statement.

***

## 30. CTE vs Stored Procedure

A CTE is part of a SQL statement.

A stored procedure is a database program that can contain multiple SQL statements and procedural logic, depending on the database.

Conceptually:

```
CTE
→ query-level transformation

View
→ persistent query abstraction

Temporary table
→ temporary stored intermediate data

Stored procedure
→ reusable database program
```

***

## 31. Recursive CTE

A recursive CTE is a CTE that refers to itself.

Basic structure:

```sql
WITH RECURSIVE cte_name AS (

    -- Anchor query
    SELECT ...

    UNION ALL

    -- Recursive query
    SELECT ...
    FROM cte_name
    WHERE ...

)
SELECT *
FROM cte_name;
```

The important concept is:

```
Anchor
   ↓
Recursive step
   ↓
Recursive step
   ↓
Recursive step
   ↓
...
```

***

## 32. Two Components of a Recursive CTE

A recursive CTE normally contains:

1. Anchor member
2. Recursive member

Example:

```sql
WITH RECURSIVE numbers AS (
    SELECT 1 AS n

    UNION ALL

    SELECT n + 1
    FROM numbers
    WHERE n < 10
)
SELECT *
FROM numbers;
```

***

## 33. Understanding the Anchor

```sql
SELECT 1 AS n
```

This creates the initial row:

```
1
```

This is called the anchor member.

***

## 34. Understanding the Recursive Member

```sql
SELECT n + 1
FROM numbers
WHERE n < 10
```

The recursive member uses the previous result.

Conceptually:

```
1
↓
2
↓
3
↓
4
↓
...
↓
10
```

The `WHERE n < 10` condition stops the recursion.

***

## 35. Why WHERE Is Important in Recursive CTE

Consider:

```sql
WITH RECURSIVE numbers AS (
    SELECT 1 AS n

    UNION ALL

    SELECT n + 1
    FROM numbers
    WHERE n < 10
)
SELECT *
FROM numbers;
```

The condition:

```sql
WHERE n < 10
```

is the termination condition.

Without a proper termination condition, recursion can continue indefinitely or until the database's recursion/resource limit is reached.

This is one of the most important concepts to teach.

***

## 36. Recursive CTE Execution

For:

```sql
WITH RECURSIVE numbers AS (
    SELECT 1 AS n

    UNION ALL

    SELECT n + 1
    FROM numbers
    WHERE n < 5
)
SELECT *
FROM numbers;
```

Think about the execution like this:

#### Step 1

Anchor:

```
1
```

#### Step 2

Recursive query sees:

```
1
```

Produces:

```
2
```

#### Step 3

Recursive query sees:

```
2
```

Produces:

```
3
```

#### Step 4

Produces:

```
4
```

#### Step 5

Produces:

```
5
```

#### Step 6

For `5`:

```sql
WHERE n < 5
```

is false.

Recursion stops.

Final:

```
1
2
3
4
5
```

***

## 37. Recursive CTE for Employee Hierarchy

This is one of the most important real-world examples.

Our table contains:

```
employee_id
manager_id
```

For example:

```
Arun Kumar
    ↓
Priya Sharma
    ↓
Karthik Rao
    ↓
Gopal Krishna
    ↓
Akhil Raj
```

This represents a hierarchy.

***

## 38. Find the Management Chain

Suppose we want to start with employee `91`.

Employee 91:

```
Akhil Raj
```

Manager:

```
20 → Gopal Krishna
```

Manager of Gopal:

```
6 → Karthik Rao
```

Manager of Karthik:

```
8 → Suresh Babu
```

Manager of Suresh:

```
2 → Priya Sharma
```

Manager of Priya:

```
1 → Arun Kumar
```

Recursive CTE:

```sql
WITH RECURSIVE employee_hierarchy AS (

    -- Anchor
    SELECT
        employee_id,
        employee_name,
        manager_id,
        0 AS level
    FROM employees
    WHERE employee_id = 91

    UNION ALL

    -- Recursive member
    SELECT
        e.employee_id,
        e.employee_name,
        e.manager_id,
        eh.level + 1
    FROM employees e
    JOIN employee_hierarchy eh
        ON e.employee_id = eh.manager_id
)
SELECT *
FROM employee_hierarchy
ORDER BY level;
```

***

## 39. Understanding This Recursive Employee Query

Anchor:

```sql
SELECT
    employee_id,
    employee_name,
    manager_id,
    0 AS level
FROM employees
WHERE employee_id = 91
```

This starts with one employee.

Then:

```sql
JOIN employee_hierarchy eh
    ON e.employee_id = eh.manager_id
```

moves upward to the employee's manager.

The level increases:

```sql
eh.level + 1
```

Result conceptually:

```
Level 0 → Akhil Raj
Level 1 → Gopal Krishna
Level 2 → Karthik Rao
Level 3 → Suresh Babu
Level 4 → Priya Sharma
Level 5 → Arun Kumar
```

***

## 40. Find All Employees Under a Manager

Recursive CTEs can also move downward.

Suppose we want everyone under employee `8`.

```sql
WITH RECURSIVE employee_tree AS (

    -- Anchor
    SELECT
        employee_id,
        employee_name,
        manager_id,
        0 AS level
    FROM employees
    WHERE employee_id = 8

    UNION ALL

    -- Recursive member
    SELECT
        e.employee_id,
        e.employee_name,
        e.manager_id,
        et.level + 1
    FROM employees e
    JOIN employee_tree et
        ON e.manager_id = et.employee_id
)
SELECT *
FROM employee_tree
ORDER BY level, employee_id;
```

This moves:

```
Manager
  ↓
Direct reports
  ↓
Reports of reports
  ↓
Reports of reports of reports
  ↓
...
```

***

## 41. Recursive CTE for Organizational Hierarchy

Conceptual hierarchy:

```
Arun Kumar
│
├── Priya Sharma
│   ├── Karthik Rao
│   │   ├── Gopal Krishna
│   │   └── Harish Kumar
│   ├── Meena Krishnan
│   └── Suresh Babu
│       ├── Ajay Kumar
│       ├── Pooja Shah
│       └── Vijay Anand
│
├── Rahul Verma
│   ├── Divya Nair
│   └── Ramesh Gupta
│
├── Sneha Iyer
│   └── Anita Das
│
└── Vikram Singh
    ├── Manoj Patel
    └── Lakshmi Menon
```

Recursive CTEs are designed for this type of parent-child relationship.

***

## 42. Recursive CTE With a Path

Sometimes we want to display the complete hierarchy path.

PostgreSQL example:

```sql
WITH RECURSIVE employee_tree AS (

    SELECT
        employee_id,
        employee_name,
        manager_id,
        employee_name::TEXT AS hierarchy_path,
        0 AS level
    FROM employees
    WHERE employee_id = 8

    UNION ALL

    SELECT
        e.employee_id,
        e.employee_name,
        e.manager_id,
        et.hierarchy_path || ' -> ' || e.employee_name,
        et.level + 1
    FROM employees e
    JOIN employee_tree et
        ON e.manager_id = et.employee_id
)
SELECT *
FROM employee_tree
ORDER BY level, employee_id;
```

The path can look like:

```
Suresh Babu
Suresh Babu -> Ajay Kumar
Suresh Babu -> Pooja Shah
Suresh Babu -> Vijay Anand
```

For employees several levels deep:

```
Suresh Babu
-> Vijay Anand
-> Akhil Raj
```

***

## 43. Recursive CTE for Dates

Recursive CTEs can generate sequences.

Example:

```sql
WITH RECURSIVE dates AS (

    SELECT DATE '2026-01-01' AS dt

    UNION ALL

    SELECT dt + INTERVAL '1 day'
    FROM dates
    WHERE dt < DATE '2026-01-10'
)
SELECT *
FROM dates;
```

Result:

```
2026-01-01
2026-01-02
2026-01-03
...
2026-01-10
```

Note: Many analytical databases provide dedicated date-generation functions that may be preferable for large date ranges.

***

## 44. Recursive CTE for Fibonacci Sequence

```sql
WITH RECURSIVE fibonacci AS (

    SELECT
        0 AS a,
        1 AS b,
        1 AS position

    UNION ALL

    SELECT
        b,
        a + b,
        position + 1
    FROM fibonacci
    WHERE position < 10
)
SELECT *
FROM fibonacci;
```

Concept:

```
0, 1
1, 1
1, 2
2, 3
3, 5
5, 8
...
```

This is mainly useful for understanding recursion, rather than as a typical data-engineering production pattern.

***

## 45. Recursive CTE and WHERE Clause

The `WHERE` clause can control which rows participate in the next recursive iteration.

Example:

```sql
WITH RECURSIVE numbers AS (
    SELECT 1 AS n

    UNION ALL

    SELECT n + 1
    FROM numbers
    WHERE n < 100
)
SELECT *
FROM numbers;
```

Here:

```sql
WHERE n < 100
```

is the stopping condition.

In hierarchical queries, `WHERE` can also restrict which starting nodes are selected or which rows are allowed into the recursive branch.

***

## 46. Important Recursive CTE Rule

The recursive member must eventually stop.

Bad design:

```sql
WITH RECURSIVE numbers AS (
    SELECT 1 AS n

    UNION ALL

    SELECT n + 1
    FROM numbers
)
SELECT *
FROM numbers;
```

There is no termination condition.

Better:

```sql
WITH RECURSIVE numbers AS (
    SELECT 1 AS n

    UNION ALL

    SELECT n + 1
    FROM numbers
    WHERE n < 100
)
SELECT *
FROM numbers;
```

***

## 47. Recursive CTE: UNION ALL

Most recursive CTE examples use:

```sql
UNION ALL
```

because each recursive iteration contributes new rows.

Some databases and use cases permit `UNION` instead, which applies duplicate elimination.

For ordinary recursive hierarchy traversal, `UNION ALL` is commonly used, but duplicate/cycle handling must be considered carefully.

***

## 48. Cycle Problem in Hierarchies

Imagine:

```
A → B
B → C
C → A
```

There is a cycle.

A naive recursive query can keep following:

```
A
B
C
A
B
C
...
```

Therefore, production hierarchical queries may need cycle detection or a visited-node path.

Example conceptual approach:

```
Current node
+
Visited nodes
↓
Before visiting next node
check whether it has already been visited
```

Exact syntax differs by database.

***

## 49. Recursive CTE With Cycle Protection

PostgreSQL-style conceptual example:

```sql
WITH RECURSIVE employee_tree AS (

    SELECT
        employee_id,
        employee_name,
        manager_id,
        ARRAY[employee_id] AS visited_ids
    FROM employees
    WHERE employee_id = 8

    UNION ALL

    SELECT
        e.employee_id,
        e.employee_name,
        e.manager_id,
        et.visited_ids || e.employee_id
    FROM employees e
    JOIN employee_tree et
        ON e.manager_id = et.employee_id
    WHERE NOT e.employee_id = ANY(et.visited_ids)
)
SELECT *
FROM employee_tree;
```

The important idea is not the exact syntax but the algorithm:

```
If node already exists in visited list
        ↓
Do not visit it again
```

***

## 50. Multiple CTEs and Dependency Order

Consider:

```sql
WITH
a AS (...),
b AS (
    SELECT *
    FROM a
),
c AS (
    SELECT *
    FROM b
)
SELECT *
FROM c;
```

Dependency:

```
a
↓
b
↓
c
↓
final query
```

A later CTE can reference an earlier CTE in the same `WITH` clause.

***

## 51. Can One CTE Reference a Later CTE?

Normally:

```sql
WITH
a AS (
    SELECT *
    FROM b
),
b AS (
    SELECT *
    FROM employees
)
SELECT *
FROM a;
```

is not valid in many SQL implementations because `a` references `b` before `b` has been defined.

A normal non-recursive CTE chain should generally follow dependency order:

```sql
WITH
b AS (
    SELECT *
    FROM employees
),
a AS (
    SELECT *
    FROM b
)
SELECT *
FROM a;
```

Recursive CTEs are the special case where self-reference is explicitly supported.

***

## 52. CTE With CASE

```sql
WITH employee_classification AS (
    SELECT
        employee_id,
        employee_name,
        department,
        salary,
        CASE
            WHEN salary >= 100000 THEN 'High'
            WHEN salary >= 70000 THEN 'Medium'
            ELSE 'Low'
        END AS salary_category
    FROM employees
)
SELECT *
FROM employee_classification
ORDER BY salary DESC;
```

***

## 53. CTE With Date Filtering

Find employees who joined after 2023:

```sql
WITH recent_employees AS (
    SELECT *
    FROM employees
    WHERE joining_date >= DATE '2023-01-01'
)
SELECT *
FROM recent_employees
ORDER BY joining_date;
```

***

## 54. CTE With Aggregation and Filtering

Find departments with more than 10 employees.

```sql
WITH department_counts AS (
    SELECT
        department,
        COUNT(*) AS employee_count
    FROM employees
    GROUP BY department
)
SELECT *
FROM department_counts
WHERE employee_count > 10;
```

This demonstrates an important pattern:

```
GROUP BY
   ↓
calculated aggregate
   ↓
outer WHERE
```

***

## 55. Why Not Put COUNT in WHERE?

This is invalid:

```sql
SELECT
    department,
    COUNT(*) AS employee_count
FROM employees
WHERE COUNT(*) > 10
GROUP BY department;
```

Aggregate filtering belongs in `HAVING` at that query level:

```sql
SELECT
    department,
    COUNT(*) AS employee_count
FROM employees
GROUP BY department
HAVING COUNT(*) > 10;
```

Or use a CTE:

```sql
WITH department_counts AS (
    SELECT
        department,
        COUNT(*) AS employee_count
    FROM employees
    GROUP BY department
)
SELECT *
FROM department_counts
WHERE employee_count > 10;
```

***

## 56. CTE With Window Aggregate

Calculate department average salary for every employee.

```sql
WITH employee_salary AS (
    SELECT
        employee_id,
        employee_name,
        department,
        salary,
        AVG(salary) OVER (
            PARTITION BY department
        ) AS department_avg_salary
    FROM employees
)
SELECT *
FROM employee_salary;
```

***

## 57. Find Employees Above Department Average

This is a very common interview and practical problem.

```sql
WITH employee_salary AS (
    SELECT
        employee_id,
        employee_name,
        department,
        salary,
        AVG(salary) OVER (
            PARTITION BY department
        ) AS department_avg_salary
    FROM employees
)
SELECT *
FROM employee_salary
WHERE salary > department_avg_salary;
```

Why CTE?

Because the window calculation creates a derived value, and the outer query can filter it.

***

## 58. Find Highest Paid Employee Per Department

```sql
WITH ranked_employees AS (
    SELECT
        employee_id,
        employee_name,
        department,
        salary,
        ROW_NUMBER() OVER (
            PARTITION BY department
            ORDER BY salary DESC
        ) AS rn
    FROM employees
)
SELECT
    employee_id,
    employee_name,
    department,
    salary
FROM ranked_employees
WHERE rn = 1;
```

***

## 59. Find Second Highest Salary Per Department

```sql
WITH ranked_employees AS (
    SELECT
        employee_id,
        employee_name,
        department,
        salary,
        DENSE_RANK() OVER (
            PARTITION BY department
            ORDER BY salary DESC
        ) AS salary_rank
    FROM employees
)
SELECT *
FROM ranked_employees
WHERE salary_rank = 2;
```

***

## 60. Find Employees Who Earn More Than Their Manager

```sql
WITH employee_manager AS (
    SELECT
        e.employee_id,
        e.employee_name,
        e.salary AS employee_salary,
        m.employee_name AS manager_name,
        m.salary AS manager_salary
    FROM employees e
    JOIN employees m
        ON e.manager_id = m.employee_id
)
SELECT *
FROM employee_manager
WHERE employee_salary > manager_salary;
```

This demonstrates:

```
CTE
+
self join
+
WHERE
```

***

## 61. CTE for Monthly/Yearly Reporting

Suppose we want yearly hiring counts.

```sql
WITH employee_year AS (
    SELECT
        EXTRACT(YEAR FROM joining_date) AS joining_year,
        employee_id
    FROM employees
)
SELECT
    joining_year,
    COUNT(*) AS employees_joined
FROM employee_year
GROUP BY joining_year
ORDER BY joining_year;
```

***

## 62. CTE as a Data Transformation Layer

A useful mental model for data engineers:

```
CTE 1
Raw filtering
    ↓
CTE 2
Cleaning
    ↓
CTE 3
Standardization
    ↓
CTE 4
Business rules
    ↓
CTE 5
Aggregation
    ↓
Final SELECT
```

Example:

```sql
WITH source_data AS (
    SELECT *
    FROM employees
),
cleaned_data AS (
    SELECT *
    FROM source_data
    WHERE employee_id IS NOT NULL
      AND salary IS NOT NULL
      AND salary > 0
),
standardized_data AS (
    SELECT
        employee_id,
        employee_name,
        TRIM(department) AS department,
        salary
    FROM cleaned_data
),
classified_data AS (
    SELECT
        *,
        CASE
            WHEN salary >= 100000 THEN 'High'
            WHEN salary >= 70000 THEN 'Medium'
            ELSE 'Low'
        END AS salary_band
    FROM standardized_data
)
SELECT *
FROM classified_data;
```

***

## 63. CTEs in Modern Data Engineering

CTEs are especially common in:

* ELT pipelines
* Analytics engineering
* Data warehouse transformations
* Data quality logic
* Deduplication
* Slowly changing dimension preparation
* Incremental transformation logic
* Window-function transformations
* Hierarchical data
* Reporting queries
* Feature engineering
* Data marts

A common pattern is:

```
Source table
     ↓
CTE: source
     ↓
CTE: cleaned
     ↓
CTE: transformed
     ↓
CTE: enriched
     ↓
CTE: aggregated
     ↓
Final table/view
```

***

## 64. Important: CTE Does Not Automatically Mean Performance Improvement

A common misconception is:

> "CTE makes the query faster."

Not necessarily.

A CTE is primarily a query-structuring mechanism.

Depending on the database engine and query, a CTE may be:

* Inlined into the surrounding query
* Materialized
* Optimized in another way

Therefore:

```
CTE ≠ automatically faster
```

The main benefit is often:

```
Readability
Maintainability
Logical organization
Debugging
Reusable intermediate logic within the statement
```

Performance depends on the database optimizer and the specific query.

***

## 65. CTE Performance Considerations

When working with large data:

* Filter early when logically appropriate.
* Select only required columns.
* Avoid unnecessary repeated computation.
* Check execution plans.
* Understand whether the database materializes the CTE.
* Consider temporary tables when an intermediate result must be reused across multiple statements or benefits from indexing.
* Use appropriate partitioning/clustering/indexing strategies for the database.
* Do not create many CTE layers just for the sake of creating CTE layers.

***

## 66. Bad CTE Style

Avoid meaningless layers like:

```sql
WITH a AS (
    SELECT *
    FROM employees
),
b AS (
    SELECT *
    FROM a
),
c AS (
    SELECT *
    FROM b
)
SELECT *
FROM c;
```

There is no useful transformation.

Better:

```sql
SELECT *
FROM employees;
```

or create meaningful transformation stages.

***

## 67. Good CTE Naming

Bad:

```sql
WITH a AS (...),
b AS (...),
c AS (...)
```

Better:

```sql
WITH active_employees AS (...),
department_stats AS (...),
ranked_employees AS (...)
```

Good names explain what the intermediate result represents.

***

## 68. Avoid SELECT \* in Production Transformations

Instead of:

```sql
SELECT *
FROM employees;
```

prefer:

```sql
SELECT
    employee_id,
    employee_name,
    department,
    salary
FROM employees;
```

Why?

* Makes dependencies clear.
* Avoids unnecessary columns.
* Reduces accidental schema coupling.
* Makes downstream transformations easier to understand.

***

## 69. Debugging a Complex CTE

Suppose:

```sql
WITH
step1 AS (...),
step2 AS (...),
step3 AS (...)
SELECT *
FROM step3;
```

If the result is wrong, temporarily run:

```sql
WITH
step1 AS (...),
step2 AS (...),
step3 AS (...)
SELECT *
FROM step1;
```

Then:

```sql
SELECT *
FROM step2;
```

Then:

```sql
SELECT *
FROM step3;
```

This lets you inspect each transformation stage.

This is a major practical advantage of CTE-based SQL.

***

## 70. CTE as a SQL Pipeline

Think:

```
CTE #1
  ↓
CTE #2
  ↓
CTE #3
  ↓
CTE #4
  ↓
Final SELECT
```

Each CTE should ideally have one clear responsibility.

For example:

```
raw
→ filter invalid records

cleaned
→ standardize columns

enriched
→ join dimension data

ranked
→ calculate window functions

final
→ apply business filter
```

***

## 71. Complete Practical Example

Requirement:

> Find the top 3 highest-paid active employees in every department, but only include employees earning above their department average.

Solution:

```sql
WITH active_employees AS (
    SELECT
        employee_id,
        employee_name,
        department,
        salary
    FROM employees
    WHERE employment_status = 'Active'
),
employee_stats AS (
    SELECT
        employee_id,
        employee_name,
        department,
        salary,
        AVG(salary) OVER (
            PARTITION BY department
        ) AS department_avg_salary
    FROM active_employees
),
above_average AS (
    SELECT
        employee_id,
        employee_name,
        department,
        salary,
        department_avg_salary
    FROM employee_stats
    WHERE salary > department_avg_salary
),
ranked_employees AS (
    SELECT
        *,
        ROW_NUMBER() OVER (
            PARTITION BY department
            ORDER BY salary DESC
        ) AS rn
    FROM above_average
)
SELECT
    employee_id,
    employee_name,
    department,
    salary,
    department_avg_salary,
    rn
FROM ranked_employees
WHERE rn <= 3
ORDER BY department, rn;
```

Logical pipeline:

```
employees
    ↓
active_employees
    ↓
employee_stats
    ↓
above_average
    ↓
ranked_employees
    ↓
top 3
```

***

## 72. Recursive CTE Practical Example

Requirement:

> Starting from employee 91, show every manager above that employee.

```sql
WITH RECURSIVE management_chain AS (

    -- Anchor
    SELECT
        employee_id,
        employee_name,
        manager_id,
        0 AS level
    FROM employees
    WHERE employee_id = 91

    UNION ALL

    -- Recursive member
    SELECT
        m.employee_id,
        m.employee_name,
        m.manager_id,
        mc.level + 1
    FROM employees m
    JOIN management_chain mc
        ON m.employee_id = mc.manager_id
)
SELECT
    level,
    employee_id,
    employee_name,
    manager_id
FROM management_chain
ORDER BY level;
```

Expected conceptual output:

```
0 | 91 | Akhil Raj       | 20
1 | 20 | Gopal Krishna   | 6
2 | 6  | Karthik Rao     | 2
3 | 2  | Priya Sharma    | 1
4 | 1  | Arun Kumar      | NULL
```

***

## 73. Recursive CTE: Downward Hierarchy

Requirement:

> Starting from employee 8, find all employees under that manager at every level.

```sql
WITH RECURSIVE employee_tree AS (

    -- Anchor
    SELECT
        employee_id,
        employee_name,
        manager_id,
        0 AS level
    FROM employees
    WHERE employee_id = 8

    UNION ALL

    -- Recursive member
    SELECT
        e.employee_id,
        e.employee_name,
        e.manager_id,
        et.level + 1
    FROM employees e
    JOIN employee_tree et
        ON e.manager_id = et.employee_id
)
SELECT *
FROM employee_tree
ORDER BY level, employee_id;
```

***

## 74. Upward vs Downward Recursion

### Upward

Start with an employee.

Find:

```
employee
↓
manager
↓
manager's manager
↓
CEO
```

Join:

```sql
ON e.employee_id = hierarchy.manager_id
```

### Downward

Start with a manager.

Find:

```
manager
↓
employees
↓
employees' employees
↓
...
```

Join:

```sql
ON e.manager_id = hierarchy.employee_id
```

This distinction is extremely important.

***

## 75. Recursive CTE Algorithm

A recursive CTE can be understood as:

```
1. Run anchor query.
2. Store anchor result.
3. Run recursive query using the previous iteration.
4. Add new rows.
5. Repeat.
6. Stop when recursive query produces no more rows
   or when a termination condition prevents further rows.
7. Return the accumulated result.
```

This is the core idea.

***

## 76. CTE Logical Query Processing

For a simple CTE:

```sql
WITH data AS (
    SELECT *
    FROM employees
    WHERE salary > 80000
)
SELECT *
FROM data
WHERE department = 'Technology';
```

Think:

```
employees
    ↓
CTE filter: salary > 80000
    ↓
data
    ↓
outer filter: department = Technology
    ↓
final result
```

***

## 77. CTE Does Not Store Data Permanently

Important distinction:

```
CTE
↓
logical intermediate result
```

not:

```
CTE
↓
permanent physical table
```

Unless a particular database optimizer chooses to materialize it internally, you should not treat a CTE as a permanent stored dataset.

***

## 78. Common CTE Mistakes

### Mistake 1: Missing comma

Wrong:

```sql
WITH a AS (...),
b AS (...)
SELECT ...
```

Correct syntax includes commas between CTE definitions.

***

### Mistake 2: Referencing CTE outside its statement

Wrong:

```sql
WITH data AS (
    SELECT *
    FROM employees
)
SELECT *
FROM data;

SELECT *
FROM data;
```

The second statement normally cannot access `data`.

***

### Mistake 3: Filtering a window function at the same level

Wrong:

```sql
SELECT
    *,
    ROW_NUMBER() OVER (...) AS rn
FROM employees
WHERE rn <= 3;
```

Correct:

```sql
WITH ranked AS (
    SELECT
        *,
        ROW_NUMBER() OVER (...) AS rn
    FROM employees
)
SELECT *
FROM ranked
WHERE rn <= 3;
```

***

### Mistake 4: Infinite recursive query

Wrong:

```sql
WITH RECURSIVE numbers AS (
    SELECT 1 AS n
    UNION ALL
    SELECT n + 1
    FROM numbers
)
SELECT *
FROM numbers;
```

There is no termination condition.

***

### Mistake 5: Creating unnecessary CTEs

Do not make every single `SELECT` a CTE.

Use CTEs when they improve:

* clarity
* transformation structure
* debugging
* maintainability
* reuse within the statement

***

## 79. Interview Questions

### Q1. What is a CTE?

A CTE is a named temporary result set defined using `WITH` and available for the duration of a single SQL statement.

***

### Q2. Why use CTEs?

For:

* readability
* modular query design
* multi-step transformations
* window-function filtering
* recursive queries
* easier debugging

***

### Q3. Is a CTE a physical table?

No. A CTE is a query construct. Whether the database materializes it internally depends on the database engine and optimizer.

***

### Q4. CTE vs subquery?

Both can express intermediate queries. CTEs generally make multi-step logic easier to read and maintain.

***

### Q5. CTE vs temporary table?

A CTE normally exists for one statement. A temporary table can persist for multiple statements within its temporary-table lifetime and can often be indexed.

***

### Q6. What is a recursive CTE?

A recursive CTE is a CTE that references itself and repeatedly executes a recursive member until recursion terminates.

***

### Q7. What are the two parts of a recursive CTE?

```
Anchor member
+
Recursive member
```

***

### Q8. Why is WHERE important in recursive CTE?

It can provide a termination condition or restrict rows participating in recursive iterations.

***

### Q9. Where are recursive CTEs used?

Common examples:

* organizational hierarchies
* employee-manager relationships
* folder structures
* category trees
* bill of materials
* graph-like parent-child relationships
* sequence generation

***

## 80. Practice Exercises

### Exercise 1

Find all employees with salary greater than 90,000 using a CTE.

***

### Exercise 2

Find all Technology employees who earn more than 80,000.

***

### Exercise 3

Find average salary by department using a CTE.

***

### Exercise 4

Find departments with more than 10 employees.

***

### Exercise 5

Find the top 3 employees by salary in every department.

***

### Exercise 6

Find employees whose salary is greater than their department average.

***

### Exercise 7

Find employees whose salary is greater than their manager's salary.

***

### Exercise 8

Find the second-highest-paid employee in every department.

***

### Exercise 9

Starting from employee 91, find the complete management chain.

***

### Exercise 10

Starting from employee 8, find all employees under that manager.

***

### Exercise 11

Generate numbers from 1 to 100 using a recursive CTE.

***

### Exercise 12

Generate dates from January 1, 2026 to January 31, 2026 using a recursive CTE.

***

## 81. Suggested Teaching Sequence

For a classroom session, teach in this order:

```
1. Problem with deeply nested subqueries
        ↓
2. Definition of CTE
        ↓
3. Basic WITH syntax
        ↓
4. Simple CTE
        ↓
5. WHERE inside CTE
        ↓
6. WHERE outside CTE
        ↓
7. Multiple CTEs
        ↓
8. CTE + GROUP BY
        ↓
9. CTE + HAVING
        ↓
10. CTE + JOIN
        ↓
11. CTE + CASE
        ↓
12. CTE + Window Functions
        ↓
13. Filtering window-function results
        ↓
14. Data-engineering use cases
        ↓
15. CTE vs subquery
        ↓
16. CTE vs temporary table
        ↓
17. CTE vs view
        ↓
18. Performance considerations
        ↓
19. Recursive CTE
        ↓
20. Anchor + recursive member
        ↓
21. Recursive WHERE / termination
        ↓
22. Employee hierarchy
        ↓
23. Upward recursion
        ↓
24. Downward recursion
        ↓
25. Cycle detection
        ↓
26. Practice problems
```

***

## 82. One-Line Summary of Every Topic

| Topic                 | Meaning                                                     |
| --------------------- | ----------------------------------------------------------- |
| CTE                   | Named intermediate query result                             |
| `WITH`                | Starts CTE definition                                       |
| Multiple CTEs         | Chain multiple transformation stages                        |
| `WHERE` inside CTE    | Filter while creating intermediate data                     |
| `WHERE` outside CTE   | Filter the CTE result                                       |
| `GROUP BY`            | Aggregate data inside a CTE                                 |
| `HAVING`              | Filter groups                                               |
| JOIN + CTE            | Combine transformed data                                    |
| Window function + CTE | Calculate analytic values and filter them outside           |
| CTE vs subquery       | Mostly organization/readability difference                  |
| CTE vs temp table     | Statement-scoped query construct vs temporary stored object |
| CTE vs view           | Temporary query definition vs persistent database object    |
| Recursive CTE         | CTE that references itself                                  |
| Anchor                | Starting rows                                               |
| Recursive member      | Generates next level                                        |
| Recursive `WHERE`     | Often controls termination/filtering                        |
| Hierarchy             | Parent-child relationships                                  |
| Cycle detection       | Prevent repeated traversal                                  |
| Data engineering      | Multi-step transformation and analytical logic              |

***

## 83. Final Mental Model

Remember this:

```
NORMAL CTE

WITH
    step1 AS (...),
    step2 AS (...),
    step3 AS (...)
SELECT *
FROM step3;
```

Think:

```
Source
  ↓
Step 1
  ↓
Step 2
  ↓
Step 3
  ↓
Final Result
```

Recursive CTE:

```
WITH RECURSIVE
    hierarchy AS (

        ANCHOR

        UNION ALL

        RECURSIVE STEP
    )
SELECT *
FROM hierarchy;
```

Think:

```
Start
  ↓
Next level
  ↓
Next level
  ↓
Next level
  ↓
Stop condition
```

The most important distinction:

```
Normal CTE
→ Defines an intermediate result.

Recursive CTE
→ Defines an intermediate result
  that can reference itself
  to process multiple levels/iterations.
```

***

## 84. Final Data Engineer Perspective

A data engineer should not think of a CTE merely as:

> "Another SQL syntax."

Think of it as a way to break a complicated transformation into understandable stages.

For example:

```
RAW DATA
   ↓
filter invalid records
   ↓
CLEAN DATA
   ↓
standardize values
   ↓
ENRICHED DATA
   ↓
calculate metrics
   ↓
RANKED DATA
   ↓
apply business rules
   ↓
FINAL DATASET
```

SQL can represent that pipeline cleanly:

```sql
WITH raw_data AS (...),
cleaned_data AS (...),
enriched_data AS (...),
calculated_data AS (...),
ranked_data AS (...),
final_data AS (...)
SELECT *
FROM final_data;
```

For hierarchical problems:

```
Parent
  ↓
Child
  ↓
Grandchild
  ↓
Great-grandchild
  ↓
...
```

use:

```sql
WITH RECURSIVE ...
```

That is the fundamental idea behind CTEs and recursive CTEs.
