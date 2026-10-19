# Normalization & SCD

> Source notebook: [DE SQL Notes/Normalization & SCD.ipynb](https://github.com/karthikeyanm-dev/DE-Notes/blob/main/DE%20SQL%20Notes/Normalization%20%26%20SCD.ipynb). Notebook outputs are omitted; open the source to run cells.

```sql
%load_ext sql
%sql mysql+mysqlconnector://root:123456@localhost/test
```

## Introduction to Database Normalization

Normalization is an important process in database design that helps improve the **efficiency**, **consistency**, and **accuracy** of a database.

It organizes data into logical tables, making the database easier to manage and maintain while ensuring that it can adapt to changing business requirements.

***

## Before Normalization

In a non-normalized database, all information is stored in a single table, resulting in duplicated data and unnecessary repetition.

### Employee\_Department

| Emp\_ID | Emp\_Name  | Department | Dept\_Location | Emp\_Skills          |
| ------- | ---------- | ---------- | -------------- | -------------------- |
| 101     | Nick Wise  | HR         | London         | Recruitment, Payroll |
| 102     | John Cader | Finance    | Australia      | Budgeting            |
| 103     | Lily Case  | HR         | London         | Recruitment          |
| 104     | Ford Dawid | IT         | Chicago        | Programming, Testing |

#### Problems Before Normalization

* Department information is repeated for multiple employees.
* Employee skills are stored as multiple values in a single column.
* Data redundancy increases storage requirements.
* Updating department information requires modifying multiple rows.
* Deleting an employee may accidentally remove important department information.
* Inserting new departments without employees becomes difficult.

***

## After Normalization

After normalization, the data is divided into multiple logical tables.

### Employee

| Emp\_ID | Emp\_Name  | Dept\_ID |
| ------- | ---------- | -------- |
| 101     | Nick Wise  | D1       |
| 102     | John Cader | D2       |
| 103     | Lily Case  | D1       |
| 104     | Ford Dawid | D3       |

***

### Department

| Dept\_ID | Department | Dept\_Location |
| -------- | ---------- | -------------- |
| D1       | HR         | London         |
| D2       | Finance    | Australia      |
| D3       | IT         | Chicago        |

***

### Employee Skills

| Emp\_ID | Emp\_Skills |
| ------- | ----------- |
| 101     | Recruitment |
| 101     | Payroll     |
| 102     | Budgeting   |
| 103     | Recruitment |
| 104     | Programming |
| 104     | Testing     |

***

## Before vs After Normalization

| Before Normalization                            | After Normalization                           |
| ----------------------------------------------- | --------------------------------------------- |
| Data is stored in a single table.               | Data is divided into multiple logical tables. |
| Duplicate data is common.                       | Duplicate data is minimized.                  |
| Update, Insert, and Delete anomalies may occur. | Anomalies are greatly reduced.                |
| Data maintenance is difficult.                  | Data maintenance becomes easier.              |
| Database is less efficient.                     | Database is more efficient and reliable.      |

***

## Types of Normalization

Normalization consists of several stages known as **Normal Forms (NF)**.

The most commonly used normal forms are:

* **First Normal Form (1NF)**
* **Second Normal Form (2NF)**
* **Third Normal Form (3NF)**

Although there are additional normal forms such as **BCNF**, **4NF**, and **5NF**, understanding **1NF**, **2NF**, and **3NF** is sufficient for most database design scenarios and forms the foundation of normalization.

## First Normal Form (1NF)

### What is First Normal Form (1NF)?

First Normal Form (1NF) states that:

* Every column must contain **atomic (indivisible) values**.
* Each field should contain only **one value**.
* There should be **no repeating groups** or multiple values in a single column.
* Every row should be uniquely identifiable using a Primary Key.

***

### Purpose of 1NF

The main purpose of First Normal Form is to ensure that each field contains only a **single piece of data**.

This makes the database:

* Easier to search
* Easier to update
* Easier to maintain
* Free from repeating groups

***

## Before First Normal Form

Suppose we have a table that stores student information.

### Students

| Student\_ID | Student\_Name | Phone                  |
| ----------- | ------------- | ---------------------- |
| 101         | John          | 9876543210, 9123456789 |
| 102         | Sara          | 9988776655             |
| 103         | David         | 9876501234, 9012345678 |

#### Problem

The **Phone** column contains multiple phone numbers in a single cell.

This violates the First Normal Form because every column should contain only **one value**.

***

## After First Normal Form

To satisfy 1NF, we separate the phone numbers into another table.

### Students

| Student\_ID | Student\_Name |
| ----------- | ------------- |
| 101         | John          |
| 102         | Sara          |
| 103         | David         |

***

### Student\_Phone

| Student\_ID | Phone      |
| ----------- | ---------- |
| 101         | 9876543210 |
| 101         | 9123456789 |
| 102         | 9988776655 |
| 103         | 9876501234 |
| 103         | 9012345678 |

***

## Before vs After 1NF

| Before 1NF                                   | After 1NF                    |
| -------------------------------------------- | ---------------------------- |
| Multiple values in one column                | One value per column         |
| Repeating groups exist                       | No repeating groups          |
| Difficult to search individual phone numbers | Easy to search phone numbers |
| Data is less organized                       | Data is well organized       |

***

## Advantages of First Normal Form

* Removes repeating groups.
* Stores only atomic values.
* Makes searching easier.
* Simplifies updates and maintenance.
* Improves data consistency.
* Forms the foundation for higher normal forms.

***

## Key Takeaways

* First Normal Form (1NF) ensures that every column contains only **atomic (single) values**.
* A single cell should never store multiple values.
* Repeating groups should be moved into separate tables.
* 1NF is the first step toward designing a well-structured and normalized database.

```sql
%%sql
# create schema normalization;
use normalization
```

```sql
%%sql

CREATE TABLE students_non1NF (
    id INT,
    name VARCHAR(100),
    age INT,
    phone VARCHAR(100)
);
```

```sql
%%sql
INSERT INTO students_non1NF VALUES
(101, 'John', 20, '9876543210,9123456789'),
(102, 'Sara', 21, '9988776655'),
(103, 'David', 22, '9876501234,9012345678');
```

```sql
%%sql
SELECT *
FROM students_non1NF;
```

```sql
%%sql
CREATE TABLE students (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    age INT
);
```

```sql
%%sql
CREATE TABLE student_phone (
    id INT,
    phone VARCHAR(15),
    FOREIGN KEY (id) REFERENCES students(id)
);
```

```sql
%%sql
INSERT INTO students VALUES
(101, 'John', 20),
(102, 'Sara', 21),
(103, 'David', 22);
```

```sql
%%sql
INSERT INTO student_phone VALUES
(101, '9876543210'),
(101, '9123456789'),
(102, '9988776655'),
(103, '9876501234'),
(103, '9012345678');
```

```sql
%%sql
SELECT
    s.id,
    s.name,
    s.age,
    sp.phone
FROM students AS s
INNER JOIN student_phone AS sp
ON s.id = sp.id;
```

## Second Normal Form (2NF)

### What is Second Normal Form (2NF)?

Second Normal Form (2NF) states that:

* The table must already be in **First Normal Form (1NF)**.
* Every non-key column must be **fully functionally dependent** on the **entire Primary Key**.
* There should be **no partial dependency**.

***

### Purpose of 2NF

The main purpose of Second Normal Form is to **remove partial dependencies**, where a non-key column depends on only part of a composite primary key instead of the whole key.

***

## What is Partial Dependency?

A **Partial Dependency** occurs when a table has a **Composite Primary Key**, but one or more non-key columns depend on only one part of that key.

***

## Before Second Normal Form

Suppose a student can enroll in multiple courses.

The combination of **Student\_ID** and **Course\_ID** uniquely identifies each record.

#### Student\_Course

| Student\_ID | Course\_ID | Student\_Name | Course\_Name | Course\_Fee |
| ----------- | ---------- | ------------- | ------------ | ----------- |
| 101         | C101       | John          | Java         | 5000        |
| 101         | C102       | John          | Python       | 6000        |
| 102         | C101       | Sara          | Java         | 5000        |
| 103         | C103       | David         | SQL          | 4500        |

#### Composite Primary Key

```
(Student_ID, Course_ID)
```

#### Problem

* **Student\_Name** depends only on **Student\_ID**.
* **Course\_Name** depends only on **Course\_ID**.
* **Course\_Fee** depends only on **Course\_ID**.

These columns do **not** depend on the entire composite key.

This is called **Partial Dependency**, which violates 2NF.

***

## SQL Example (Before 2NF)

#### Create Table

```sql
CREATE TABLE student_course_non2NF (
    student_id INT,
    course_id VARCHAR(10),
    student_name VARCHAR(100),
    course_name VARCHAR(100),
    course_fee DECIMAL(10,2),
    PRIMARY KEY (student_id, course_id)
);
```

#### Insert Data

```sql
INSERT INTO student_course_non2NF VALUES
(101, 'C101', 'John', 'Java', 5000),
(101, 'C102', 'John', 'Python', 6000),
(102, 'C101', 'Sara', 'Java', 5000),
(103, 'C103', 'David', 'SQL', 4500);
```

#### View Data

```sql
SELECT *
FROM student_course_non2NF;
```

***

## After Second Normal Form

To remove partial dependencies, split the table into three related tables.

***

### Students

| Student\_ID | Student\_Name |
| ----------- | ------------- |
| 101         | John          |
| 102         | Sara          |
| 103         | David         |

***

### Courses

| Course\_ID | Course\_Name | Course\_Fee |
| ---------- | ------------ | ----------- |
| C101       | Java         | 5000        |
| C102       | Python       | 6000        |
| C103       | SQL          | 4500        |

***

### Student\_Course

| Student\_ID | Course\_ID |
| ----------- | ---------- |
| 101         | C101       |
| 101         | C102       |
| 102         | C101       |
| 103         | C103       |

***

## SQL Example (After 2NF)

### Students Table

```sql
CREATE TABLE students (
    student_id INT PRIMARY KEY,
    student_name VARCHAR(100)
);
```

***

### Courses Table

```sql
CREATE TABLE courses (
    course_id VARCHAR(10) PRIMARY KEY,
    course_name VARCHAR(100),
    course_fee DECIMAL(10,2)
);
```

***

### Student\_Course Table

```sql
CREATE TABLE student_course (
    student_id INT,
    course_id VARCHAR(10),
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES students(student_id),
    FOREIGN KEY (course_id) REFERENCES courses(course_id)
);
```

***

### Insert Student Data

```sql
INSERT INTO students VALUES
(101, 'John'),
(102, 'Sara'),
(103, 'David');
```

***

### Insert Course Data

```sql
INSERT INTO courses VALUES
('C101', 'Java', 5000),
('C102', 'Python', 6000),
('C103', 'SQL', 4500);
```

***

### Insert Student-Course Mapping

```sql
INSERT INTO student_course VALUES
(101, 'C101'),
(101, 'C102'),
(102, 'C101'),
(103, 'C103');
```

***

## Join All Tables

```sql
SELECT
    sc.student_id,
    s.student_name,
    sc.course_id,
    c.course_name,
    c.course_fee
FROM student_course sc
INNER JOIN students s
ON sc.student_id = s.student_id
INNER JOIN courses c
ON sc.course_id = c.course_id;
```

#### Output

| Student\_ID | Student\_Name | Course\_ID | Course\_Name | Course\_Fee |
| ----------- | ------------- | ---------- | ------------ | ----------- |
| 101         | John          | C101       | Java         | 5000        |
| 101         | John          | C102       | Python       | 6000        |
| 102         | Sara          | C101       | Java         | 5000        |
| 103         | David         | C103       | SQL          | 4500        |

***

## Before vs After 2NF

| Before 2NF                       | After 2NF                                |
| -------------------------------- | ---------------------------------------- |
| Student information is repeated. | Student information is stored only once. |
| Course information is repeated.  | Course information is stored only once.  |
| Partial dependency exists.       | Partial dependency is removed.           |
| More data redundancy.            | Less data redundancy.                    |
| Difficult to maintain.           | Easier to maintain.                      |

***

## Advantages of Second Normal Form

* Removes partial dependencies.
* Reduces duplicate data.
* Improves data consistency.
* Makes updates easier.
* Reduces storage redundancy.
* Creates a better database structure.

***

## Key Takeaways

* Second Normal Form (2NF) requires the table to already be in **1NF**.
* Every non-key column must depend on the **entire composite primary key**.
* Partial dependencies are removed by splitting the table into related tables.
* 2NF improves database consistency and reduces redundancy.

````

# Third Normal Form (3NF)

## What is Third Normal Form (3NF)?

Third Normal Form (3NF) states that:

- The table must already be in **Second Normal Form (2NF)**.
- Every non-key column must depend **directly** on the Primary Key.
- There should be **no transitive dependency**.

---

## Purpose of 3NF

The main purpose of Third Normal Form is to **remove transitive dependencies**, where a non-key column depends on another non-key column instead of depending directly on the Primary Key.

---

# What is Transitive Dependency?

A **Transitive Dependency** occurs when a non-key column depends on another non-key column rather than directly on the Primary Key.

For example:

```text
Employee_ID → Department_ID → Department_Name
````

Here,

* `Department_Name` does **not** depend directly on `Employee_ID`.
* It depends on `Department_ID`, which is another non-key column.

This is called a **Transitive Dependency**.

***

## Before Third Normal Form

Suppose we have the following employee table.

### Employee

| Employee\_ID | Employee\_Name | Department\_ID | Department\_Name | Department\_Location |
| ------------ | -------------- | -------------- | ---------------- | -------------------- |
| 101          | John           | D1             | HR               | Chennai              |
| 102          | Sara           | D2             | Finance          | Bangalore            |
| 103          | David          | D1             | HR               | Chennai              |
| 104          | Emma           | D3             | IT               | Hyderabad            |

#### Problem

* `Department_Name` depends on `Department_ID`.
* `Department_Location` depends on `Department_ID`.

These columns do **not** depend directly on the Primary Key (`Employee_ID`).

This is a **Transitive Dependency**, which violates 3NF.

***

## SQL Example (Before 3NF)

### Create Table

```sql
CREATE TABLE employee_non3NF (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(100),
    department_id VARCHAR(10),
    department_name VARCHAR(100),
    department_location VARCHAR(100)
);
```

***

### Insert Data

```sql
INSERT INTO employee_non3NF VALUES
(101, 'John', 'D1', 'HR', 'Chennai'),
(102, 'Sara', 'D2', 'Finance', 'Bangalore'),
(103, 'David', 'D1', 'HR', 'Chennai'),
(104, 'Emma', 'D3', 'IT', 'Hyderabad');
```

***

### View Data

```sql
SELECT *
FROM employee_non3NF;
```

***

## After Third Normal Form

To remove the transitive dependency, split the table into two related tables.

***

### Employees

| Employee\_ID | Employee\_Name | Department\_ID |
| ------------ | -------------- | -------------- |
| 101          | John           | D1             |
| 102          | Sara           | D2             |
| 103          | David          | D1             |
| 104          | Emma           | D3             |

***

### Departments

| Department\_ID | Department\_Name | Department\_Location |
| -------------- | ---------------- | -------------------- |
| D1             | HR               | Chennai              |
| D2             | Finance          | Bangalore            |
| D3             | IT               | Hyderabad            |

***

## SQL Example (After 3NF)

### Employees Table

```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(100),
    department_id VARCHAR(10),
    FOREIGN KEY (department_id) REFERENCES departments(department_id)
);
```

***

### Departments Table

```sql
CREATE TABLE departments (
    department_id VARCHAR(10) PRIMARY KEY,
    department_name VARCHAR(100),
    department_location VARCHAR(100)
);
```

***

### Insert Department Data

```sql
INSERT INTO departments VALUES
('D1', 'HR', 'Chennai'),
('D2', 'Finance', 'Bangalore'),
('D3', 'IT', 'Hyderabad');
```

***

### Insert Employee Data

```sql
INSERT INTO employees VALUES
(101, 'John', 'D1'),
(102, 'Sara', 'D2'),
(103, 'David', 'D1'),
(104, 'Emma', 'D3');
```

***

## Join Both Tables

```sql
SELECT
    e.employee_id,
    e.employee_name,
    d.department_name,
    d.department_location
FROM employees e
INNER JOIN departments d
ON e.department_id = d.department_id;
```

#### Output

| Employee\_ID | Employee\_Name | Department\_Name | Department\_Location |
| ------------ | -------------- | ---------------- | -------------------- |
| 101          | John           | HR               | Chennai              |
| 102          | Sara           | Finance          | Bangalore            |
| 103          | David          | HR               | Chennai              |
| 104          | Emma           | IT               | Hyderabad            |

***

## Before vs After 3NF

| Before 3NF                                             | After 3NF                                   |
| ------------------------------------------------------ | ------------------------------------------- |
| Department information is repeated for every employee. | Department information is stored only once. |
| Transitive dependency exists.                          | Transitive dependency is removed.           |
| More duplicate data.                                   | Less duplicate data.                        |
| Updates require multiple changes.                      | Updates require only one change.            |
| Difficult to maintain.                                 | Easier to maintain.                         |

***

## Advantages of Third Normal Form

* Removes transitive dependencies.
* Eliminates duplicate department information.
* Improves data consistency.
* Reduces storage redundancy.
* Simplifies updates and maintenance.
* Provides a well-structured database design.

***

## Key Takeaways

* Third Normal Form (3NF) requires the table to already be in **2NF**.
* Every non-key column must depend **directly** on the Primary Key.
* Transitive dependencies are removed by separating related data into different tables.
* 3NF reduces redundancy, improves consistency, and makes the database easier to maintain.

## Slowly Changing Dimension (SCD)

### What is a Slowly Changing Dimension (SCD)?

A **Slowly Changing Dimension (SCD)** is a **data warehousing concept** used to manage changes in **dimension tables** over time.

A **dimension table** contains descriptive information about business entities, such as:

* Customers
* Employees
* Products
* Stores
* Suppliers
* Locations

Since this information can change over time, SCD defines **how those changes should be stored** while preserving the required level of historical information.

***

### Purpose of SCD

The main purpose of a Slowly Changing Dimension is to manage changes in dimension data while preserving the required history.

***

### Why is SCD Needed?

Without SCD:

* Historical reports become inaccurate.
* Past business transactions may show incorrect information.
* Historical trend analysis becomes difficult.
* Business decisions may be based on incorrect historical data.

#### Example

Initially, the customer lives in Chennai.

| Customer\_ID | Name | City    |
| ------------ | ---- | ------- |
| 101          | John | Chennai |

After a few months, John moves to Bangalore.

The question is:

* Should we replace **Chennai** with **Bangalore**?
* Should we keep both cities?
* Should we maintain the complete history?

The answer depends on the **type of SCD** being used.

***

### Business Example

Suppose a customer places an order while living in Chennai.

| Order Date | Customer City |
| ---------- | ------------- |
| Jan 2025   | Chennai       |
| Jul 2025   | Bangalore     |

If we simply overwrite the customer's city with **Bangalore**, then the January order would incorrectly appear as if it was placed from Bangalore.

Using SCD allows us to preserve historical information accurately.

***

## Types of Slowly Changing Dimensions

| SCD Type | Description                             | Keeps History          |
| -------- | --------------------------------------- | ---------------------- |
| Type 0   | Never change the data                   | Yes (Original Only)    |
| Type 1   | Overwrite the existing value            | No                     |
| Type 2   | Create a new row for every change       | Yes (Complete History) |
| Type 3   | Add a new column for the previous value | Partial                |
| Type 4   | Store history in a separate table       | Yes                    |
| Type 6   | Combination of Types 1, 2, and 3        | Yes                    |

***

## Real-World Examples

An employee's information may change over time, such as:

* Address
* Department
* Manager
* Salary Grade

Depending on business requirements:

* HR may require complete historical records (Type 2).
* Marketing may only need the latest address (Type 1).
* Finance may need both the current and previous department (Type 3).

***

## Common Use Cases

* Customer address changes
* Employee department changes
* Product category changes
* Supplier information updates
* Store location changes
* Sales territory changes

***

## Interview Definition

> **A Slowly Changing Dimension (SCD) is a technique used in data warehousing to manage and track changes in dimension table attributes over time while preserving the required level of historical information.**

***

## Which SCD Types Are Used Most?

In real-world Data Engineering projects:

1. SCD Type 1 — Very Common
2. SCD Type 2 — Most Important and Widely Used
3. SCD Type 3 — Occasionally Used
4. SCD Types 0, 4, and 6 — Used for specific business requirements

If you are learning Data Engineering, focus first on **SCD Type 1** and **SCD Type 2**, as these are the most commonly used in ETL pipelines, data warehouses, and technical interviews.

***

## Key Takeaways

* SCD stands for **Slowly Changing Dimension**.
* It is used to manage changes in dimension tables over time.
* SCD helps preserve historical data based on business requirements.
* Different SCD types provide different strategies for handling changes.
* Type 1 and Type 2 are the most widely used in real-world data engineering projects.

## Visual Examples of Slowly Changing Dimension (SCD)

Suppose the initial customer information is:

### Initial Customer Table

| Customer\_ID | Name | City    |
| ------------ | ---- | ------- |
| 101          | John | Chennai |

After a few months, John moves to **Bangalore**.

***

## SCD Type 0 (Fixed Dimension)

### Rule

**Never modify the original data.**

#### Before Change

| Customer\_ID | Name | City    |
| ------------ | ---- | ------- |
| 101          | John | Chennai |

⬇️ Customer moves to Bangalore

#### After Change

| Customer\_ID | Name | City    |
| ------------ | ---- | ------- |
| 101          | John | Chennai |

✅ Original value is preserved forever.

***

## SCD Type 1 (Overwrite)

### Rule

**Replace the old value with the new value.**

#### Before Change

| Customer\_ID | Name | City    |
| ------------ | ---- | ------- |
| 101          | John | Chennai |

⬇️ Customer moves to Bangalore

#### After Change

| Customer\_ID | Name | City      |
| ------------ | ---- | --------- |
| 101          | John | Bangalore |

❌ Previous city (Chennai) is lost.

***

## SCD Type 2 (New Row)

### Rule

**Create a new row whenever the data changes.**

#### Before Change

| Customer\_ID | Name | City    | Start\_Date | End\_Date | Current |
| ------------ | ---- | ------- | ----------- | --------- | ------- |
| 101          | John | Chennai | 01-Jan-2025 | NULL      | Yes     |

⬇️ Customer moves to Bangalore

#### After Change

| Customer\_ID | Name | City      | Start\_Date | End\_Date   | Current |
| ------------ | ---- | --------- | ----------- | ----------- | ------- |
| 101          | John | Chennai   | 01-Jan-2025 | 30-Jun-2025 | No      |
| 101          | John | Bangalore | 01-Jul-2025 | NULL        | Yes     |

✅ Complete history is preserved.

***

## SCD Type 3 (New Column)

### Rule

**Keep the current value and the previous value in separate columns.**

#### Before Change

| Customer\_ID | Name | Current\_City | Previous\_City |
| ------------ | ---- | ------------- | -------------- |
| 101          | John | Chennai       | NULL           |

⬇️ Customer moves to Bangalore

#### After Change

| Customer\_ID | Name | Current\_City | Previous\_City |
| ------------ | ---- | ------------- | -------------- |
| 101          | John | Bangalore     | Chennai        |

✅ Only the previous value is stored.

***

## SCD Type 4 (History Table)

### Rule

**Current data remains in one table, while historical records are stored in another table.**

### Current Customer Table

| Customer\_ID | Name | City      |
| ------------ | ---- | --------- |
| 101          | John | Bangalore |

***

### Customer History Table

| Customer\_ID | Name | City    | Changed\_On |
| ------------ | ---- | ------- | ----------- |
| 101          | John | Chennai | 01-Jul-2025 |

✅ Current table stays small while history is maintained separately.

***

## SCD Type 6 (Hybrid)

### Rule

**Combination of Type 1 + Type 2 + Type 3**

#### Customer Table

| Customer\_ID | Name | Current\_City | Previous\_City | Start\_Date | End\_Date   | Current |
| ------------ | ---- | ------------- | -------------- | ----------- | ----------- | ------- |
| 101          | John | Chennai       | NULL           | 01-Jan-2025 | 30-Jun-2025 | No      |
| 101          | John | Bangalore     | Chennai        | 01-Jul-2025 | NULL        | Yes     |

This combines:

* ✅ Type 1 → Current value
* ✅ Type 2 → Multiple historical rows
* ✅ Type 3 → Previous value stored as a column

***

## Comparison of All SCD Types

| Type       | Action Taken                      | History Preserved  |
| ---------- | --------------------------------- | ------------------ |
| **Type 0** | Never change data                 | ✅ Original only    |
| **Type 1** | Overwrite existing value          | ❌ No               |
| **Type 2** | Insert a new row                  | ✅ Complete history |
| **Type 3** | Add a previous value column       | ⚠️ Partial history |
| **Type 4** | Store history in a separate table | ✅ Complete history |
| **Type 6** | Combination of Types 1, 2, and 3  | ✅ Complete history |

***

## Easy Way to Remember

| Type       | Memory Trick            |
| ---------- | ----------------------- |
| **Type 0** | **Don't Touch**         |
| **Type 1** | **Replace**             |
| **Type 2** | **New Row**             |
| **Type 3** | **New Column**          |
| **Type 4** | **History Table**       |
| **Type 6** | **Everything Together** |
