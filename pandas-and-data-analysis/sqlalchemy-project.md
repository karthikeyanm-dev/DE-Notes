# Page

## SQLAlchemy Prj

> Imported from [`python/Pandas/Prj/SQLAlchemy_Prj.ipynb`](https://github.com/karthikeyanm-dev/DE-Notes/blob/main/python/Pandas/Prj/SQLAlchemy_Prj.ipynb). Code outputs are omitted; run the source notebook for live results.

## SQLAlchemy with MySQL — From Basics to ETL Project

We are going to build this section in **two parts**.

```
PART 1
SQLAlchemy + MySQL Basics
        ↓
Connection
CRUD
Transactions
Parameterized Queries
Pandas Integration

PART 2
Real E-Commerce ETL Project
        ↓
MySQL Source Tables
        ↓
Extract with SQLAlchemy
        ↓
Pandas Cleaning
        ↓
Validation
        ↓
Transformation
        ↓
Load into Target Tables
        ↓
Reporting
```

We will use **MySQL only** in this section.

***

## PART 1 — SQLAlchemy with MySQL Basics

## 1. What is SQLAlchemy?

**SQLAlchemy** is a Python library that provides tools for working with relational databases.

It allows Python applications to:

```
Connect to databases
Execute SQL
Insert data
Read data
Update data
Delete data
Manage transactions
Create tables
Work with database metadata
Integrate with Pandas
```

For our Data Engineering use case:

```
Python
   ↓
SQLAlchemy
   ↓
MySQL
```

***

## 2. Why SQLAlchemy?

Suppose we want Python to communicate with MySQL.

We could use a MySQL-specific Python driver such as:

```
PyMySQL
mysql-connector-python
```

SQLAlchemy works as an abstraction layer above the database driver.

```
Python
   ↓
SQLAlchemy
   ↓
PyMySQL
   ↓
MySQL
```

This means our Python code can use SQLAlchemy's common API while the underlying driver handles communication with MySQL.

***

## 3. SQLAlchemy Components

For our course, understand these concepts:

```
Engine
Connection
SQL text
Transactions
Parameters
Result
Session
ORM
```

We will start with:

```
Engine
   ↓
Connection
   ↓
SQL
   ↓
CRUD
```

Then later:

```
Pandas
   ↓
read_sql()
   ↓
to_sql()
```

And after that:

```
ORM
```

***

## 4. Install SQLAlchemy

```python
pip install sqlalchemy pymysql pandas
```

In Jupyter:

```python
!pip install sqlalchemy pymysql pandas
```

For Excel support in our later project:

```python
!pip install openpyxl
```

***

## 5. MySQL Setup

Before using SQLAlchemy, MySQL Server should be running.

For example:

```
MySQL Server
localhost
Port: 3306
Username: root
Password: your_password
```

You can manage MySQL using:

```
MySQL Workbench
```

or:

```
MySQL command line
```

***

## 6. Create the Database

You can create the database from MySQL Workbench:

```sql
CREATE DATABASE ecommerce_etl;
```

Then:

```sql
USE ecommerce_etl;
```

Check:

```sql
SHOW DATABASES;
```

***

## 7. Import SQLAlchemy

```python
from sqlalchemy import create_engine
```

***

## 8. Create a MySQL Engine

Basic connection string:

```python
engine = create_engine(
    "mysql+pymysql://root:password@localhost:3306/ecommerce_etl"
)
```

Structure:

```
mysql+pymysql
      ↓
MySQL using PyMySQL

root
      ↓
username

password
      ↓
password

localhost
      ↓
server

3306
      ↓
port

ecommerce_etl
      ↓
database
```

***

## 9. Better Method — `URL.create()`

If your password contains special characters such as:

```
@
#
$
%
```

the URL string can become difficult to handle.

SQLAlchemy provides a safer way:

```python
from sqlalchemy import create_engine
from sqlalchemy.engine import URL

connection_url = URL.create(
    drivername="mysql+pymysql",
    username="root",
    password="your_password",
    host="localhost",
    port=3306,
    database="ecommerce_etl"
)

engine = create_engine(
    connection_url
)
```

This approach is easier to manage when credentials contain special characters.

***

## 10. Test the Connection

```python
from sqlalchemy import text

with engine.connect() as connection:

    result = connection.execute(
        text("SELECT 1")
    )

    print(result.scalar())
```

Output:

```
1
```

This means:

```
Python
   ↓
SQLAlchemy
   ↓
PyMySQL
   ↓
MySQL
```

is working.

***

## 11. Understanding `engine`

The engine is not simply:

```
"the database"
```

It manages the connection infrastructure between your Python application and the database.

Conceptually:

```
Python Application
        ↓
     Engine
        ↓
   Connection Pool
        ↓
   MySQL Driver
        ↓
      MySQL
```

***

## 12. Connection

We can explicitly create a connection:

```python
with engine.connect() as connection:

    print("Connected to MySQL")
```

Using:

```python
with
```

ensures that the connection is properly released.

***

## 13. Execute SQL

SQLAlchemy provides:

```python
text()
```

for writing SQL statements.

Example:

```python
from sqlalchemy import text

sql = text("""
    SELECT 1
""")

with engine.connect() as connection:

    result = connection.execute(sql)

    print(result.scalar())
```

***

## 14. `SELECT VERSION()`

A useful connection test:

```python
with engine.connect() as connection:

    result = connection.execute(
        text("SELECT VERSION()")
    )

    print(
        result.scalar()
    )
```

This prints the MySQL server version.

***

## 15. Read Current Database

```python
with engine.connect() as connection:

    result = connection.execute(
        text("SELECT DATABASE()")
    )

    print(
        result.scalar()
    )
```

Possible output:

```
ecommerce_etl
```

***

## 16. Create a Table

Let's create a customer table.

```python
create_table = text("""
    CREATE TABLE customers (
        customer_id INT PRIMARY KEY,
        customer_name VARCHAR(100),
        email VARCHAR(150),
        city VARCHAR(100),
        state VARCHAR(100),
        age INT
    )
""")
```

Execute:

```python
with engine.begin() as connection:

    connection.execute(
        create_table
    )
```

***

## 17. Why `engine.begin()`?

There is an important difference between:

```python
engine.connect()
```

and:

```python
engine.begin()
```

`begin()` creates a transaction context.

Conceptually:

```
engine.begin()
      ↓
START TRANSACTION
      ↓
SQL Operations
      ↓
Success → COMMIT

Error → ROLLBACK
```

For insert/update/delete operations, this is very useful.

***

## 18. Check the Table

```python
with engine.connect() as connection:

    result = connection.execute(
        text("SHOW TABLES")
    )

    for row in result:

        print(row)
```

***

## 19. Describe the Table

```python
with engine.connect() as connection:

    result = connection.execute(
        text("DESCRIBE customers")
    )

    for row in result:

        print(row)
```

***

## 20. CRUD

CRUD means:

```
C → Create
R → Read
U → Update
D → Delete
```

In SQL:

```
CREATE TABLE
INSERT
SELECT
UPDATE
DELETE
```

***

## 21. CREATE — Insert One Record

```python
sql = text("""
    INSERT INTO customers
    (
        customer_id,
        customer_name,
        email,
        city,
        state,
        age
    )
    VALUES
    (
        1001,
        'Karthi',
        'karthi@gmail.com',
        'Chennai',
        'Tamil Nadu',
        30
    )
""")
```

Execute:

```python
with engine.begin() as connection:

    connection.execute(sql)
```

***

## 22. READ — Select Records

```python
sql = text("""
    SELECT *
    FROM customers
""")

with engine.connect() as connection:

    result = connection.execute(sql)

    for row in result:

        print(row)
```

***

## 23. Reading Individual Values

Suppose:

```python
row
```

contains:

```
(1001, 'Karthi', 'karthi@gmail.com', ...)
```

We can access by position:

```python
print(row[0])
```

Or by column:

```python
print(row.customer_name)
```

***

## 24. `fetchone()`

```python
with engine.connect() as connection:

    result = connection.execute(
        text("""
            SELECT *
            FROM customers
        """)
    )

    row = result.fetchone()

    print(row)
```

Only one row is returned.

***

## 25. `fetchall()`

```python
with engine.connect() as connection:

    result = connection.execute(
        text("""
            SELECT *
            FROM customers
        """)
    )

    rows = result.fetchall()

    print(rows)
```

***

## 26. `SELECT` with WHERE

```python
sql = text("""
    SELECT *
    FROM customers
    WHERE city = 'Chennai'
""")

with engine.connect() as connection:

    result = connection.execute(sql)

    for row in result:

        print(row)
```

***

## 27. Parameterized SQL

Instead of manually creating SQL:

```python
sql = f"""
SELECT *
FROM customers
WHERE city = '{city}'
"""
```

use parameters.

```python
sql = text("""
    SELECT *
    FROM customers
    WHERE city = :city
""")
```

Execute:

```python
with engine.connect() as connection:

    result = connection.execute(
        sql,
        {
            "city": "Chennai"
        }
    )

    for row in result:

        print(row)
```

This is the recommended approach.

***

## 28. Why Parameterized Queries?

Avoid:

```python
f"SELECT * FROM users WHERE name = '{name}'"
```

when values come from users or external systems.

Prefer:

```python
text("""
    SELECT *
    FROM users
    WHERE name = :name
""")
```

with:

```python
{"name": name}
```

The SQL structure and data values remain separate.

***

## 29. INSERT with Parameters

```python
sql = text("""
    INSERT INTO customers
    (
        customer_id,
        customer_name,
        email,
        city,
        state,
        age
    )
    VALUES
    (
        :customer_id,
        :customer_name,
        :email,
        :city,
        :state,
        :age
    )
""")
```

Data:

```python
customer = {
    "customer_id": 1002,
    "customer_name": "Ravi",
    "email": "ravi@gmail.com",
    "city": "Bangalore",
    "state": "Karnataka",
    "age": 28
}
```

Execute:

```python
with engine.begin() as connection:

    connection.execute(
        sql,
        customer
    )
```

***

## 30. Insert Multiple Records

```python
customers = [
    {
        "customer_id": 1003,
        "customer_name": "Arun",
        "email": "arun@gmail.com",
        "city": "Coimbatore",
        "state": "Tamil Nadu",
        "age": 35
    },
    {
        "customer_id": 1004,
        "customer_name": "Priya",
        "email": "priya@gmail.com",
        "city": "Mumbai",
        "state": "Maharashtra",
        "age": 26
    },
    {
        "customer_id": 1005,
        "customer_name": "Suresh",
        "email": "suresh@gmail.com",
        "city": "Hyderabad",
        "state": "Telangana",
        "age": 32
    }
]
```

Execute:

```python
with engine.begin() as connection:

    connection.execute(
        sql,
        customers
    )
```

***

## 31. UPDATE

```python
sql = text("""
    UPDATE customers
    SET city = :city
    WHERE customer_id = :customer_id
""")
```

Execute:

```python
with engine.begin() as connection:

    connection.execute(
        sql,
        {
            "city": "Chennai",
            "customer_id": 1002
        }
    )
```

***

## 32. UPDATE Multiple Columns

```python
sql = text("""
    UPDATE customers

    SET
        city = :city,
        age = :age

    WHERE customer_id = :customer_id
""")

with engine.begin() as connection:

    connection.execute(
        sql,
        {
            "city": "Bangalore",
            "age": 29,
            "customer_id": 1002
        }
    )
```

***

## 33. DELETE

```python
sql = text("""
    DELETE FROM customers
    WHERE customer_id = :customer_id
""")

with engine.begin() as connection:

    connection.execute(
        sql,
        {
            "customer_id": 1005
        }
    )
```

***

## 34. Check Affected Rows

The returned result contains:

```python
rowcount
```

Example:

```python
with engine.begin() as connection:

    result = connection.execute(
        text("""
            UPDATE customers
            SET age = age + 1
            WHERE city = :city
        """),
        {
            "city": "Chennai"
        }
    )

    print(
        "Rows updated:",
        result.rowcount
    )
```

***

## 35. Transactions

Consider two operations:

```
Insert Customer
       ↓
Insert Order
```

Suppose the customer insert succeeds but the order insert fails.

We may not want the customer insert to remain committed.

A transaction allows both operations to behave as one logical unit.

```python
with engine.begin() as connection:

    connection.execute(
        text("""
            INSERT INTO customers (...)
            VALUES (...)
        """)
    )

    connection.execute(
        text("""
            INSERT INTO orders (...)
            VALUES (...)
        """)
    )
```

If everything succeeds:

```
COMMIT
```

If an exception occurs:

```
ROLLBACK
```

***

## 36. Exception Handling with SQLAlchemy

```python
try:

    with engine.begin() as connection:

        connection.execute(
            text("""
                INSERT INTO customers
                VALUES (
                    1006,
                    'Anitha',
                    'anitha@gmail.com',
                    'Chennai',
                    'Tamil Nadu',
                    29
                )
            """)
        )

except Exception as error:

    print(
        "Database operation failed:",
        error
    )
```

***

## 37. Create an Orders Table

```python
create_orders = text("""
    CREATE TABLE orders (

        order_id INT PRIMARY KEY,

        customer_id INT,

        product_id INT,

        order_date DATE,

        quantity INT,

        amount DECIMAL(12,2),

        status VARCHAR(30),

        payment_method VARCHAR(50)

    )
""")

with engine.begin() as connection:

    connection.execute(
        create_orders
    )
```

***

## 38. Foreign Keys

In a relational database:

```
customers
    |
    | customer_id
    |
    ↓
orders
```

We can create a foreign key:

```python
create_orders = text("""
    CREATE TABLE orders (

        order_id INT PRIMARY KEY,

        customer_id INT,

        product_id INT,

        order_date DATE,

        quantity INT,

        amount DECIMAL(12,2),

        status VARCHAR(30),

        payment_method VARCHAR(50),

        FOREIGN KEY (customer_id)
            REFERENCES customers(customer_id)

    )
""")
```

This enforces referential integrity at the database level.

***

## 39. Pandas + SQLAlchemy

This is the most important part for our Data Engineering project.

We can extract SQL data directly into a Pandas DataFrame.

```python
import pandas as pd

df = pd.read_sql(
    "SELECT * FROM customers",
    engine
)
```

Now:

```
MySQL
  ↓
SQLAlchemy
  ↓
Pandas
  ↓
DataFrame
```

***

## 40. `read_sql()` with SQLAlchemy

```python
query = """
    SELECT *
    FROM customers
    WHERE city = 'Chennai'
"""

df = pd.read_sql(
    query,
    engine
)

print(df)
```

***

## 41. `read_sql_query()`

You can also explicitly use:

```python
df = pd.read_sql_query(
    "SELECT * FROM customers",
    engine
)
```

***

## 42. Load DataFrame into MySQL

Pandas provides:

```python
to_sql()
```

Example:

```python
df.to_sql(
    "customer_backup",
    engine,
    if_exists="replace",
    index=False
)
```

Now:

```
Pandas DataFrame
       ↓
     to_sql()
       ↓
MySQL table
```

***

## 43. `if_exists`

Important options:

```
fail
replace
append
```

#### `fail`

```python
if_exists="fail"
```

Raises an error if the table exists.

#### `replace`

```python
if_exists="replace"
```

Drops/recreates the table.

#### `append`

```python
if_exists="append"
```

Adds rows to the existing table.

***

## 44. `index=False`

Always think carefully about:

```python
index=False
```

Example:

```python
df.to_sql(
    "customers",
    engine,
    if_exists="replace",
    index=False
)
```

Without `index=False`, Pandas may save the DataFrame index as an extra database column.

***

## 45. Read a Query into Pandas

We can also perform joins in MySQL and directly retrieve the result.

```python
query = """
    SELECT
        o.order_id,
        o.customer_id,
        c.customer_name,
        c.city,
        o.amount,
        o.status

    FROM orders o

    LEFT JOIN customers c
        ON o.customer_id = c.customer_id
"""

df = pd.read_sql(
    query,
    engine
)
```

This gives us:

```
Database
   ↓
SQL JOIN
   ↓
Pandas DataFrame
```

***

## 46. SQLAlchemy Core vs Pandas

Think about their roles.

#### SQLAlchemy

Good for:

```
Connection
SQL
Transactions
CRUD
Database operations
```

#### Pandas

Good for:

```
Cleaning
Transformation
Analysis
Aggregation
Data validation
DataFrames
```

Together:

```
SQLAlchemy
     ↓
Extract
     ↓
Pandas
     ↓
Transform
     ↓
SQLAlchemy / to_sql
     ↓
Load
```

That is exactly what we need for ETL.

***

## 47. PART 1 Cheat Sheet

```python
from sqlalchemy import create_engine, text
```

Create engine:

```python
engine = create_engine(
    "mysql+pymysql://root:password@localhost:3306/ecommerce_etl"
)
```

Connect:

```python
with engine.connect() as connection:
    ...
```

Transaction:

```python
with engine.begin() as connection:
    ...
```

Execute SQL:

```python
connection.execute(
    text("SELECT 1")
)
```

Read:

```python
pd.read_sql(
    "SELECT * FROM customers",
    engine
)
```

Write:

```python
df.to_sql(
    "table_name",
    engine,
    if_exists="append",
    index=False
)
```

CRUD:

```
INSERT
SELECT
UPDATE
DELETE
```

***

## PART 2 — Real E-Commerce ETL Using MySQL + SQLAlchemy + Pandas

Now we will build the same type of project we built with CSV/Excel.

But this time:

```
MySQL
   ↓
SQLAlchemy
   ↓
Pandas
   ↓
Cleaning
   ↓
Validation
   ↓
Transformation
   ↓
MySQL
```

***

## 48. Project Architecture

Our source system contains:

```
raw_customers
raw_products
raw_sales
```

These are our source tables.

After processing:

```
clean_customers
clean_products
clean_sales
```

And an enriched table:

```
sales_detail
```

The complete architecture:

```
                 MYSQL SOURCE
                      │
       ┌──────────────┼──────────────┐
       ↓              ↓              ↓
 raw_customers   raw_products    raw_sales
       │              │              │
       └──────────────┼──────────────┘
                      ↓
                 SQLAlchemy
                      ↓
                 Pandas
                      ↓
               Data Cleaning
                      ↓
              Data Validation
                      ↓
                Transformation
                      ↓
              Data Enrichment
                      ↓
                 SQLAlchemy
                      ↓
                MYSQL TARGET
                      │
       ┌──────────────┼──────────────┐
       ↓              ↓              ↓
clean_customers clean_products clean_sales
                      │
                      ↓
                 sales_detail
```

***

## 49. Step 1 — Create MySQL Database

In MySQL Workbench:

```sql
CREATE DATABASE ecommerce_etl;
```

Use it:

```sql
USE ecommerce_etl;
```

***

## 50. Step 2 — Create SQLAlchemy Engine

```python
from sqlalchemy import create_engine, text
import pandas as pd
import numpy as np

engine = create_engine(
    "mysql+pymysql://root:password@localhost:3306/ecommerce_etl"
)
```

Test:

```python
with engine.connect() as connection:

    print(
        connection.execute(
            text("SELECT DATABASE()")
        ).scalar()
    )
```

Expected:

```
ecommerce_etl
```

***

## 51. Step 3 — Generate or Use Existing Source Data

We already created:

```
customers.csv
products.csv
sales.csv
```

We can use these as our source data.

Read them:

```python
customers = pd.read_csv(
    "customers.csv"
)

products = pd.read_csv(
    "products.csv"
)

sales = pd.read_csv(
    "sales.csv"
)
```

***

## 52. Step 4 — Load Raw Data into MySQL

This is our initial source ingestion.

```python
customers.to_sql(
    "raw_customers",
    engine,
    if_exists="replace",
    index=False
)
```

Products:

```python
products.to_sql(
    "raw_products",
    engine,
    if_exists="replace",
    index=False
)
```

Sales:

```python
sales.to_sql(
    "raw_sales",
    engine,
    if_exists="replace",
    index=False
)
```

Now MySQL contains:

```
raw_customers
raw_products
raw_sales
```

***

## 53. Step 5 — Verify Raw Tables

```python
with engine.connect() as connection:

    result = connection.execute(
        text("SHOW TABLES")
    )

    for row in result:

        print(row)
```

***

## 54. Step 6 — Extract from MySQL

Instead of reading the CSV files directly, now our ETL starts from MySQL.

Customers:

```python
customers = pd.read_sql(
    """
    SELECT *
    FROM raw_customers
    """,
    engine
)
```

Products:

```python
products = pd.read_sql(
    """
    SELECT *
    FROM raw_products
    """,
    engine
)
```

Sales:

```python
sales = pd.read_sql(
    """
    SELECT *
    FROM raw_sales
    """,
    engine
)
```

Now:

```
MySQL
  ↓
SQLAlchemy
  ↓
Pandas
```

This is the **Extract** stage.

***

## 55. Step 7 — Standardize Column Names

```python
def clean_column_names(df):

    df = df.copy()

    df.columns = (
        df.columns
        .str.strip()
        .str.lower()
        .str.replace(" ", "_")
        .str.replace("-", "_")
    )

    return df
```

Apply:

```python
customers = clean_column_names(
    customers
)

products = clean_column_names(
    products
)

sales = clean_column_names(
    sales
)
```

***

## 56. Step 8 — Clean Customers

```python
def clean_customers(df):

    df = df.copy()

    df = df.drop_duplicates(
        subset=["customer_id"]
    )

    df["customer_name"] = (
        df["customer_name"]
        .astype("string")
        .str.strip()
        .str.title()
    )

    df["email"] = (
        df["email"]
        .astype("string")
        .str.strip()
        .str.lower()
    )

    df["city"] = (
        df["city"]
        .astype("string")
        .str.strip()
        .str.title()
    )

    df["state"] = (
        df["state"]
        .astype("string")
        .str.strip()
        .str.title()
    )

    df["age"] = pd.to_numeric(
        df["age"],
        errors="coerce"
    )

    df["customer_since"] = pd.to_datetime(
        df["customer_since"],
        errors="coerce"
    )

    return df
```

Apply:

```python
customers = clean_customers(
    customers
)
```

***

## 57. Step 9 — Clean Products

```python
def clean_products(df):

    df = df.copy()

    df = df.drop_duplicates(
        subset=["product_id"]
    )

    df["product_name"] = (
        df["product_name"]
        .astype("string")
        .str.strip()
        .str.title()
    )

    df["category"] = (
        df["category"]
        .astype("string")
        .str.strip()
        .str.title()
    )

    df["price"] = pd.to_numeric(
        df["price"],
        errors="coerce"
    )

    df = df[
        df["price"].notna()
    ]

    df = df[
        df["price"] >= 0
    ]

    return df
```

Apply:

```python
products = clean_products(
    products
)
```

***

## 58. Step 10 — Clean Sales

```python
def clean_sales(df):

    df = df.copy()

    id_columns = [
        "order_id",
        "customer_id",
        "product_id"
    ]

    for column in id_columns:

        df[column] = pd.to_numeric(
            df[column],
            errors="coerce"
        )

    df["order_date"] = pd.to_datetime(
        df["order_date"],
        errors="coerce"
    )

    numeric_columns = [
        "quantity",
        "unit_price",
        "gross_amount",
        "discount_percent",
        "discount_amount",
        "tax_percent",
        "tax_amount",
        "final_amount"
    ]

    for column in numeric_columns:

        df[column] = pd.to_numeric(
            df[column],
            errors="coerce"
        )

    string_columns = [
        "payment_method",
        "order_status"
    ]

    for column in string_columns:

        df[column] = (
            df[column]
            .astype("string")
            .str.strip()
            .str.title()
        )

    df = df.drop_duplicates(
        subset=["order_id"]
    )

    df = df.dropna(
        subset=[
            "order_id",
            "customer_id",
            "product_id",
            "order_date",
            "quantity",
            "final_amount"
        ]
    )

    df = df[
        df["quantity"] > 0
    ]

    df = df[
        df["final_amount"] >= 0
    ]

    return df
```

Apply:

```python
sales = clean_sales(
    sales
)
```

***

## 59. Step 11 — Referential Integrity

Make sure all sales customers exist.

```python
sales = sales[
    sales["customer_id"].isin(
        customers["customer_id"]
    )
]
```

Make sure all products exist.

```python
sales = sales[
    sales["product_id"].isin(
        products["product_id"]
    )
]
```

***

## 60. Step 12 — Create Sales Detail

Join with Products:

```python
sales_detail = sales.merge(
    products,
    on="product_id",
    how="left",
    suffixes=(
        "",
        "_master"
    )
)
```

Join with Customers:

```python
sales_detail = sales_detail.merge(
    customers[
        [
            "customer_id",
            "customer_name",
            "email",
            "city",
            "state",
            "gender",
            "customer_type"
        ]
    ],
    on="customer_id",
    how="left"
)
```

Now:

```
Sales
 +
Product
 +
Customer
```

are combined.

***

## 61. Step 13 — Calculate Business Metrics

```python
sales_detail["calculated_gross"] = (
    sales_detail["quantity"] *
    sales_detail["unit_price"]
)
```

Discount:

```python
sales_detail["calculated_discount"] = (
    sales_detail["calculated_gross"]
    *
    sales_detail["discount_percent"]
    / 100
)
```

Taxable:

```python
sales_detail["taxable_amount"] = (
    sales_detail["calculated_gross"]
    -
    sales_detail["calculated_discount"]
)
```

Tax:

```python
sales_detail["calculated_tax"] = (
    sales_detail["taxable_amount"]
    *
    sales_detail["tax_percent"]
    / 100
)
```

Final:

```python
sales_detail["net_amount"] = (
    sales_detail["taxable_amount"]
    +
    sales_detail["calculated_tax"]
)
```

***

## 62. Step 14 — Create Date Attributes

```python
sales_detail["order_year"] = (
    sales_detail["order_date"]
    .dt.year
)

sales_detail["order_month"] = (
    sales_detail["order_date"]
    .dt.month
)

sales_detail["month_name"] = (
    sales_detail["order_date"]
    .dt.month_name()
)
```

***

## 63. Step 15 — Customer Summary

```python
customer_summary = (
    sales_detail
    .groupby(
        [
            "customer_id",
            "customer_name",
            "city",
            "state",
            "customer_type"
        ],
        as_index=False
    )
    .agg(
        total_orders=(
            "order_id",
            "nunique"
        ),
        total_quantity=(
            "quantity",
            "sum"
        ),
        total_revenue=(
            "net_amount",
            "sum"
        ),
        average_order_value=(
            "net_amount",
            "mean"
        )
    )
)
```

***

## 64. Step 16 — Product Summary

```python
product_summary = (
    sales_detail
    .groupby(
        [
            "product_id",
            "product_name",
            "category"
        ],
        as_index=False
    )
    .agg(
        total_orders=(
            "order_id",
            "nunique"
        ),
        units_sold=(
            "quantity",
            "sum"
        ),
        total_revenue=(
            "net_amount",
            "sum"
        )
    )
)
```

***

## 65. Step 17 — City Summary

```python
city_summary = (
    sales_detail
    .groupby(
        [
            "city",
            "state"
        ],
        as_index=False
    )
    .agg(
        total_orders=(
            "order_id",
            "nunique"
        ),
        total_customers=(
            "customer_id",
            "nunique"
        ),
        total_revenue=(
            "net_amount",
            "sum"
        )
    )
)
```

***

## 66. Step 18 — Monthly Summary

```python
monthly_summary = (
    sales_detail
    .groupby(
        [
            "order_year",
            "order_month",
            "month_name"
        ],
        as_index=False
    )
    .agg(
        total_orders=(
            "order_id",
            "nunique"
        ),
        total_quantity=(
            "quantity",
            "sum"
        ),
        total_revenue=(
            "net_amount",
            "sum"
        )
    )
)
```

***

## 67. Step 19 — Status Summary

```python
status_summary = (
    sales_detail
    .groupby(
        "order_status",
        as_index=False
    )
    .agg(
        order_count=(
            "order_id",
            "nunique"
        ),
        total_revenue=(
            "net_amount",
            "sum"
        )
    )
)
```

***

## 68. Step 20 — KPI Summary

```python
kpi_summary = pd.DataFrame({

    "Metric": [
        "Total Orders",
        "Total Customers",
        "Total Products",
        "Total Quantity",
        "Total Revenue",
        "Average Order Value"
    ],

    "Value": [

        sales_detail[
            "order_id"
        ].nunique(),

        sales_detail[
            "customer_id"
        ].nunique(),

        sales_detail[
            "product_id"
        ].nunique(),

        sales_detail[
            "quantity"
        ].sum(),

        round(
            sales_detail[
                "net_amount"
            ].sum(),
            2
        ),

        round(
            sales_detail[
                "net_amount"
            ].mean(),
            2
        )
    ]
})
```

***

## 69. Step 21 — Load Clean Data Back to MySQL

Now we have:

```
clean customers
clean products
clean sales
sales detail
summaries
```

Load customers:

```python
customers.to_sql(
    "clean_customers",
    engine,
    if_exists="replace",
    index=False
)
```

Products:

```python
products.to_sql(
    "clean_products",
    engine,
    if_exists="replace",
    index=False
)
```

Sales:

```python
sales.to_sql(
    "clean_sales",
    engine,
    if_exists="replace",
    index=False
)
```

Sales detail:

```python
sales_detail.to_sql(
    "sales_detail",
    engine,
    if_exists="replace",
    index=False
)
```

***

## 70. Load Summary Tables

```python
customer_summary.to_sql(
    "customer_summary",
    engine,
    if_exists="replace",
    index=False
)
```

```python
product_summary.to_sql(
    "product_summary",
    engine,
    if_exists="replace",
    index=False
)
```

```python
city_summary.to_sql(
    "city_summary",
    engine,
    if_exists="replace",
    index=False
)
```

```python
monthly_summary.to_sql(
    "monthly_summary",
    engine,
    if_exists="replace",
    index=False
)
```

```python
status_summary.to_sql(
    "status_summary",
    engine,
    if_exists="replace",
    index=False
)
```

```python
kpi_summary.to_sql(
    "kpi_summary",
    engine,
    if_exists="replace",
    index=False
)
```

***

## 71. Final Database Structure

After the ETL:

```
MYSQL DATABASE
│
├── raw_customers
├── raw_products
├── raw_sales
│
├── clean_customers
├── clean_products
├── clean_sales
│
├── sales_detail
│
├── customer_summary
├── product_summary
├── city_summary
├── monthly_summary
├── status_summary
└── kpi_summary
```

This resembles a simple ETL pipeline with:

```
RAW LAYER
    ↓
TRANSFORM
    ↓
CLEAN LAYER
    ↓
ANALYTICAL TABLES
```

***

## 72. Verify Target Tables

```python
with engine.connect() as connection:

    result = connection.execute(
        text("SHOW TABLES")
    )

    for row in result:

        print(row)
```

***

## 73. Verify Clean Sales

```python
clean_sales_check = pd.read_sql(
    """
    SELECT *
    FROM clean_sales
    LIMIT 10
    """,
    engine
)

print(
    clean_sales_check
)
```

***

## 74. Verify Sales Detail

```python
detail_check = pd.read_sql(
    """
    SELECT
        order_id,
        customer_name,
        product_name,
        city,
        category,
        quantity,
        net_amount

    FROM sales_detail

    LIMIT 10
    """,
    engine
)

print(detail_check)
```

***

## 75. Query the Final Database Using SQL

Now that our ETL has created analytical tables, we can use normal SQL.

Top products by revenue:

```python
query = text("""
    SELECT
        product_name,
        total_revenue

    FROM product_summary

    ORDER BY total_revenue DESC

    LIMIT 10
""")

with engine.connect() as connection:

    result = connection.execute(query)

    for row in result:

        print(row)
```

***

## 76. Monthly Revenue

```python
query = text("""
    SELECT
        order_year,
        order_month,
        month_name,
        total_revenue

    FROM monthly_summary

    ORDER BY
        order_year,
        order_month
""")

monthly_df = pd.read_sql(
    query,
    engine
)

print(monthly_df)
```

***

## 77. Customer Revenue

```python
query = text("""
    SELECT
        customer_id,
        customer_name,
        total_orders,
        total_revenue

    FROM customer_summary

    ORDER BY total_revenue DESC

    LIMIT 10
""")

top_customers = pd.read_sql(
    query,
    engine
)

print(top_customers)
```

***

## 78. Database ETL Mental Model

The entire project can now be remembered as:

```
                  MYSQL
                    │
                    ↓
              SQLAlchemy
                    │
                    ↓
                  READ
                    │
                    ↓
             Pandas DataFrame
                    │
             ┌──────┼──────┐
             ↓      ↓      ↓
          Clean   Validate  Transform
             │      │      │
             └──────┼──────┘
                    ↓
              Enriched Data
                    │
                    ↓
              Pandas Summary
                    │
                    ↓
              SQLAlchemy
                    │
                    ↓
                  WRITE
                    │
                    ↓
                  MYSQL
```

***

## 79. Two-Part Learning Summary

### PART 1 — SQLAlchemy + MySQL

You should understand:

```
SQLAlchemy
     ↓
Engine
     ↓
Connection
     ↓
text()
     ↓
execute()
     ↓
CRUD
```

CRUD:

```
INSERT
SELECT
UPDATE
DELETE
```

Transactions:

```python
with engine.begin():
```

Parameters:

```python
:param_name
```

Pandas integration:

```python
pd.read_sql()
df.to_sql()
```

***

## 80. PART 2 — MySQL ETL

The actual ETL flow:

```
raw_customers
raw_products
raw_sales
      ↓
SQLAlchemy
      ↓
Pandas
      ↓
Data Profiling
      ↓
Data Cleaning
      ↓
Data Validation
      ↓
Data Transformation
      ↓
Data Enrichment
      ↓
Aggregation
      ↓
SQLAlchemy
      ↓
clean_customers
clean_products
clean_sales
sales_detail
summary tables
```

***

## 81. SQLAlchemy + Pandas Roles

This distinction is important.

#### SQLAlchemy

Think:

```
DATABASE CONNECTION
DATABASE OPERATIONS
SQL EXECUTION
TRANSACTIONS
```

#### Pandas

Think:

```
DATAFRAME
CLEANING
TRANSFORMATION
ANALYSIS
AGGREGATION
```

Together:

```
SQLAlchemy
    ↓
Extract from MySQL
    ↓
Pandas
    ↓
Transform
    ↓
SQLAlchemy / to_sql
    ↓
MySQL
```

***

## 82. Final Production-Style Pattern

The project we built is a simplified version of a real pipeline:

```
                 SOURCE DATABASE
                       │
                       ↓
                  SQLAlchemy
                       │
                       ↓
                  EXTRACTION
                       │
                       ↓
                 Pandas DataFrame
                       │
             ┌─────────┼─────────┐
             ↓         ↓         ↓
          Cleaning  Validation Transformation
             │         │         │
             └─────────┼─────────┘
                       ↓
                 Enriched Data
                       │
                       ↓
                  Aggregations
                       │
                       ↓
                 Target Tables
                       │
                       ↓
                    MySQL
                       │
            ┌──────────┼──────────┐
            ↓          ↓          ↓
        Reporting   BI Tools   Analytics
```

***

## 83. Recommended Next Step

Now that you understand:

```
Pandas ETL
+
SQLAlchemy
+
MySQL
```

the natural next project progression is:

```
Project 1
CSV / Excel
    ↓
Pandas
    ↓
Excel Report
```

```
Project 2
CSV / Excel
    ↓
Pandas
    ↓
SQLAlchemy
    ↓
MySQL
```

```
Project 3
MySQL Source
    ↓
SQLAlchemy
    ↓
Pandas
    ↓
Clean
    ↓
MySQL Target
```

```
Project 4
Large MySQL / CSV Data
    ↓
PySpark
    ↓
Transform
    ↓
Parquet
```

That progression gives you a clear bridge from **Python data handling → Pandas → SQLAlchemy → MySQL ETL → PySpark**.
