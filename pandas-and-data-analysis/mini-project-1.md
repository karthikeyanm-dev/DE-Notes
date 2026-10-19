# Page

## Mini Project

> Imported from [`python/Pandas/Prj/Mini Project.ipynb`](https://github.com/karthikeyanm-dev/DE-Notes/blob/main/python/Pandas/Prj/Mini%20Project.ipynb). Code outputs are omitted; run the source notebook for live results.

## Mini ETL with Pandas + Excel/CSV + SQLAlchemy

This section will connect the Python concepts we have already learned:

```
Python
   ↓
File Handling
   ↓
Pandas
   ↓
Exception Handling
   ↓
Regex
   ↓
SQL
   ↓
SQLAlchemy
```

We will divide this into **two practical parts**.

***

## PART 1 — Excel / CSV Mini ETL Using Pandas

We will build a small ETL pipeline:

```
Excel / CSV
     ↓
   Extract
     ↓
   Pandas
     ↓
   Inspect
     ↓
   Clean
     ↓
   Transform
     ↓
   Validate
     ↓
    Load
     ↓
Clean Excel / CSV
```

The important idea is:

```
E = Extract
T = Transform
L = Load
```

***

## 1. What is ETL?

ETL stands for:

```
E → Extract
T → Transform
L → Load
```

### Extract

Get data from a source.

Examples:

```
CSV
Excel
Database
API
JSON
Logs
```

### Transform

Change the raw data into a useful format.

Examples:

```
Remove duplicates
Handle missing values
Convert data types
Clean strings
Validate emails
Convert dates
Rename columns
Calculate new columns
Filter bad records
```

### Load

Save the processed data into another destination.

Examples:

```
Excel
CSV
Database
Parquet
Data Warehouse
```

***

## 2. Our First Mini ETL

We will use:

```
customers.csv
sales.xlsx
```

The pipeline will be:

```
customers.csv ────────┐
                       │
                       ↓
                  Pandas
                       │
sales.xlsx ────────────┘
                       ↓
                   Cleaning
                       ↓
                    Merge
                       ↓
                  Validation
                       ↓
               Clean Sales Data
                       ↓
            clean_sales.xlsx
            clean_sales.csv
```

***

## 3. Install Required Libraries

For CSV:

```python
pip install pandas
```

For Excel:

```python
pip install openpyxl
```

In Jupyter:

```python
!pip install pandas openpyxl
```

***

## 4. Import Pandas

```python
import pandas as pd
```

***

## 5. Reading a CSV File

```python
customers = pd.read_csv("customers.csv")
```

Inspect:

```python
print(customers)
```

Or:

```python
customers.head()
```

***

## 6. Reading an Excel File

```python
sales = pd.read_excel("sales.xlsx")
```

Inspect:

```python
sales.head()
```

***

## 7. Inspect Both Sources

Always start ETL with data profiling.

```python
print("Customers:")
print(customers.shape)

print("\nSales:")
print(sales.shape)
```

Then:

```python
customers.info()
sales.info()
```

And:

```python
print(customers.head())
print(sales.head())
```

***

## 8. Example Raw CSV

Suppose `customers.csv` contains:

```csv
customer_id,customer_name,city,email
101,Karthi,Chennai,karthi@gmail.com
102," Ravi ",Bangalore,ravi@gmail.com
103,ARUN,Coimbatore,arun@gmail.com
104,Priya,Mumbai,priya@gmail.com
105,,Hyderabad,suresh@gmail.com
```

***

## 9. Example Raw Excel

Suppose `sales.xlsx` contains:

```
order_id | customer_id | order_date | amount | status
------------------------------------------------------
1001     | 101         | 2026-09-01 | 500    | Delivered
1002     | 102         | 2026-09-02 | 700    | delivered
1003     | 103         | 2026-09-03 | INVALID| Delivered
1004     | 104         | 2026-09-04 | 1200   | Cancelled
1005     | 104         | INVALID     | 900    | Delivered
```

Our objective is to clean both datasets and create a final dataset.

***

## 10. Extract — Read the Data

```python
customers = pd.read_csv("customers.csv")

sales = pd.read_excel("sales.xlsx")
```

At this point:

```
Extract completed
```

***

## 11. Transform — Standardize Column Names

A good ETL practice is to standardize column names.

```python
customers.columns = (
    customers.columns
    .str.strip()
    .str.lower()
    .str.replace(" ", "_")
)

sales.columns = (
    sales.columns
    .str.strip()
    .str.lower()
    .str.replace(" ", "_")
)
```

For example:

```
Customer ID
Customer Name
Order Date
```

becomes:

```
customer_id
customer_name
order_date
```

***

## 12. Clean Customer Names

```python
customers["customer_name"] = (
    customers["customer_name"]
    .str.strip()
    .str.title()
)
```

Before:

```
" Ravi "
"ARUN"
```

After:

```
Ravi
Arun
```

***

## 13. Clean City

```python
customers["city"] = (
    customers["city"]
    .str.strip()
    .str.title()
)
```

***

## 14. Clean Email

```python
customers["email"] = (
    customers["email"]
    .str.strip()
    .str.lower()
)
```

***

## 15. Validate Email Using Regex

Since we already learned Regex:

```python
email_pattern = r"^[\w.-]+@[\w.-]+\.\w+$"
```

Check:

```python
valid_email = customers["email"].str.match(
    email_pattern,
    na=False
)
```

Then:

```python
customers.loc[~valid_email, "email"] = pd.NA
```

***

## 16. Handle Missing Customer Names

Suppose customer name is mandatory.

```python
customers = customers.dropna(
    subset=["customer_name"]
)
```

***

## 17. Check Duplicate Customers

```python
print(
    customers["customer_id"].duplicated().sum()
)
```

Remove duplicate customer IDs:

```python
customers = customers.drop_duplicates(
    subset=["customer_id"],
    keep="first"
)
```

***

## 18. Clean Sales Order ID

```python
sales["order_id"] = (
    sales["order_id"]
    .astype("string")
    .str.strip()
)
```

***

## 19. Clean Sales Status

```python
sales["status"] = (
    sales["status"]
    .str.strip()
    .str.title()
)
```

So:

```
Delivered
delivered
 DELIVERED
```

becomes:

```
Delivered
```

***

## 20. Convert Amount

Raw data may contain:

```
500
700
INVALID
1200
```

Use:

```python
sales["amount"] = pd.to_numeric(
    sales["amount"],
    errors="coerce"
)
```

Now:

```
INVALID
```

becomes:

```
NaN
```

***

## 21. Handle Invalid Amounts

Check:

```python
print(
    sales[sales["amount"].isna()]
)
```

If invalid amounts should be removed:

```python
sales = sales.dropna(
    subset=["amount"]
)
```

***

## 22. Remove Negative Amounts

```python
sales = sales[
    sales["amount"] >= 0
]
```

***

## 23. Convert Date

```python
sales["order_date"] = pd.to_datetime(
    sales["order_date"],
    errors="coerce"
)
```

Invalid dates become:

```
NaT
```

***

## 24. Remove Invalid Dates

```python
sales = sales.dropna(
    subset=["order_date"]
)
```

***

## 25. Check Missing Values

```python
print(customers.isna().sum())
print(sales.isna().sum())
```

This is one of the most important ETL data-quality checks.

***

## 26. Merge Excel and CSV

Now the interesting part.

We have:

```
customers.csv
       ↓
customer_id

sales.xlsx
       ↓
customer_id
```

We can join them.

```python
final_df = pd.merge(
    sales,
    customers,
    on="customer_id",
    how="left"
)
```

This is equivalent to:

```sql
SELECT *
FROM sales
LEFT JOIN customers
ON sales.customer_id = customers.customer_id;
```

***

## 27. Inspect the Merged Data

```python
print(final_df.head())
```

Possible structure:

```
order_id
customer_id
order_date
amount
status
customer_name
city
email
```

***

## 28. Check for Customers That Did Not Match

A left join can produce missing customer information.

```python
print(
    final_df[
        final_df["customer_name"].isna()
    ]
)
```

These are records where:

```
sales.customer_id
```

did not have a corresponding customer.

***

## 29. Create Derived Columns

Calculate tax:

```python
final_df["tax"] = (
    final_df["amount"] * 0.18
)
```

Calculate total:

```python
final_df["total_amount"] = (
    final_df["amount"] +
    final_df["tax"]
)
```

***

## 30. Extract Date Components

```python
final_df["order_year"] = (
    final_df["order_date"].dt.year
)

final_df["order_month"] = (
    final_df["order_date"].dt.month
)
```

***

## 31. Data Validation

Check required columns:

```python
required_columns = [
    "order_id",
    "customer_id",
    "amount",
    "order_date"
]

for column in required_columns:

    if final_df[column].isna().any():
        print(
            f"Warning: {column} contains missing values"
        )
```

***

## 32. Duplicate Order Validation

```python
duplicate_orders = final_df[
    final_df["order_id"].duplicated()
]

print(duplicate_orders)
```

***

## 33. Business Rule Validation

For example:

```
amount >= 0
```

Check:

```python
invalid_amounts = final_df[
    final_df["amount"] < 0
]

print(invalid_amounts)
```

***

## 34. Final Data Cleaning

```python
final_df = final_df.drop_duplicates(
    subset=["order_id"]
)

final_df = final_df.reset_index(
    drop=True
)
```

***

## 35. Load — Export to CSV

```python
final_df.to_csv(
    "clean_sales.csv",
    index=False
)
```

***

## 36. Load — Export to Excel

```python
final_df.to_excel(
    "clean_sales.xlsx",
    index=False
)
```

Now:

```
Input:

customers.csv
sales.xlsx

Output:

clean_sales.csv
clean_sales.xlsx
```

***

## 37. Complete Mini Excel/CSV ETL

```python
import pandas as pd

# ---------------------------------------
# EXTRACT
# ---------------------------------------

customers = pd.read_csv(
    "customers.csv"
)

sales = pd.read_excel(
    "sales.xlsx"
)


# ---------------------------------------
# PROFILE
# ---------------------------------------

print("Customers:")
print(customers.shape)
customers.info()

print("\nSales:")
print(sales.shape)
sales.info()


# ---------------------------------------
# STANDARDIZE COLUMN NAMES
# ---------------------------------------

customers.columns = (
    customers.columns
    .str.strip()
    .str.lower()
    .str.replace(" ", "_")
)

sales.columns = (
    sales.columns
    .str.strip()
    .str.lower()
    .str.replace(" ", "_")
)


# ---------------------------------------
# CLEAN CUSTOMER DATA
# ---------------------------------------

customers["customer_name"] = (
    customers["customer_name"]
    .str.strip()
    .str.title()
)

customers["city"] = (
    customers["city"]
    .str.strip()
    .str.title()
)

customers["email"] = (
    customers["email"]
    .str.strip()
    .str.lower()
)


# ---------------------------------------
# EMAIL VALIDATION
# ---------------------------------------

email_pattern = r"^[\w.-]+@[\w.-]+\.\w+$"

valid_email = customers["email"].str.match(
    email_pattern,
    na=False
)

customers.loc[
    ~valid_email,
    "email"
] = pd.NA


# ---------------------------------------
# REMOVE INVALID CUSTOMERS
# ---------------------------------------

customers = customers.dropna(
    subset=["customer_name"]
)

customers = customers.drop_duplicates(
    subset=["customer_id"]
)


# ---------------------------------------
# CLEAN SALES DATA
# ---------------------------------------

sales["status"] = (
    sales["status"]
    .str.strip()
    .str.title()
)

sales["amount"] = pd.to_numeric(
    sales["amount"],
    errors="coerce"
)

sales["order_date"] = pd.to_datetime(
    sales["order_date"],
    errors="coerce"
)


# ---------------------------------------
# REMOVE INVALID RECORDS
# ---------------------------------------

sales = sales.dropna(
    subset=[
        "amount",
        "order_date"
    ]
)

sales = sales[
    sales["amount"] >= 0
]


# ---------------------------------------
# MERGE
# ---------------------------------------

final_df = pd.merge(
    sales,
    customers,
    on="customer_id",
    how="left"
)


# ---------------------------------------
# TRANSFORMATION
# ---------------------------------------

final_df["tax"] = (
    final_df["amount"] * 0.18
)

final_df["total_amount"] = (
    final_df["amount"] +
    final_df["tax"]
)

final_df["order_year"] = (
    final_df["order_date"].dt.year
)

final_df["order_month"] = (
    final_df["order_date"].dt.month
)


# ---------------------------------------
# FINAL VALIDATION
# ---------------------------------------

final_df = final_df.drop_duplicates(
    subset=["order_id"]
)

final_df = final_df.reset_index(
    drop=True
)

print("\nFinal Data:")
print(final_df)

print("\nMissing Values:")
print(final_df.isna().sum())


# ---------------------------------------
# LOAD
# ---------------------------------------

final_df.to_csv(
    "clean_sales.csv",
    index=False
)

final_df.to_excel(
    "clean_sales.xlsx",
    index=False
)

print("\nETL completed successfully.")
```

***

## 38. ETL Architecture — Part 1

```
           customers.csv
                 │
                 │
                 ↓
              Extract
                 │
                 │
            customers
                 │
                 ↓
             Cleaning
                 │
                 ├── Strip strings
                 ├── Standardize case
                 ├── Validate email
                 └── Remove duplicates

           sales.xlsx
                 │
                 ↓
              Extract
                 │
                 ↓
              Cleaning
                 │
                 ├── Convert amount
                 ├── Convert date
                 ├── Remove invalid data
                 └── Standardize status

                 ↓
              MERGE
                 ↓
          Final DataFrame
                 ↓
            Validation
                 ↓
        ┌────────┴────────┐
        ↓                 ↓
 clean_sales.csv   clean_sales.xlsx
```

***

## PART 2 — SQLAlchemy

Now we move from:

```
Files
```

to:

```
Database
```

The ETL idea remains exactly the same.

Instead of:

```
Excel / CSV
```

we will use:

```
Database Table
```

***

## 39. What is SQLAlchemy?

**SQLAlchemy** is a Python library for working with relational databases.

It allows Python applications to:

```
Connect to databases
Execute SQL
Create tables
Insert records
Read records
Update records
Delete records
Manage transactions
Work with ORM models
```

It supports databases such as:

```
SQLite
MySQL
PostgreSQL
Oracle
Microsoft SQL Server
```

***

## 40. Why SQLAlchemy?

Without SQLAlchemy, you normally need database-specific Python drivers.

For example:

```
MySQL
PostgreSQL
Oracle
```

each has its own connection mechanism.

SQLAlchemy provides a common interface.

Conceptually:

```
Python
   ↓
SQLAlchemy
   ↓
Database Driver
   ↓
Database
```

***

## 41. SQLAlchemy Has Two Major Styles

SQLAlchemy provides:

```
1. SQLAlchemy Core
2. SQLAlchemy ORM
```

### Core

Work more directly with:

```
SQL
Tables
Connections
Transactions
```

Example:

```python
from sqlalchemy import text

connection.execute(
    text("SELECT * FROM customers")
)
```

### ORM

Represent database tables as Python classes.

Example:

```python
class Customer:
    ...
```

Then interact with objects instead of writing every SQL statement manually.

For a Data Engineering course, start with **Core + SQL**, then learn ORM.

***

## 42. Installing SQLAlchemy

```python
pip install sqlalchemy
```

Jupyter:

```python
!pip install sqlalchemy
```

For MySQL:

```python
!pip install pymysql
```

***

## 43. Starting with SQLite

SQLite is perfect for learning because:

```
No server
No username
No password
No database installation
```

It stores the database in a file.

We will start with:

```
etl_demo.db
```

***

## 44. Creating an Engine

```python
from sqlalchemy import create_engine

engine = create_engine(
    "sqlite:///etl_demo.db"
)
```

This creates the SQLAlchemy database engine.

***

## 45. Understanding the Connection String

```
sqlite:///etl_demo.db
```

means:

```
sqlite
   ↓
SQLite database
```

and:

```
etl_demo.db
```

is the database file.

***

## 46. Connecting to the Database

```python
with engine.connect() as connection:
    print("Connected successfully")
```

***

## 47. Execute SQL with SQLAlchemy

Use:

```python
from sqlalchemy import text
```

Then:

```python
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

***

## 48. Creating a Table

```python
from sqlalchemy import text

create_table_sql = """
CREATE TABLE customers (
    customer_id INTEGER PRIMARY KEY,
    customer_name VARCHAR(100),
    city VARCHAR(100),
    email VARCHAR(150),
    age INTEGER
)
"""

with engine.begin() as connection:
    connection.execute(
        text(create_table_sql)
    )
```

We use:

```python
engine.begin()
```

when we want transaction handling.

***

## 49. CRUD

CRUD stands for:

```
C → Create
R → Read
U → Update
D → Delete
```

These are the four basic database operations.

***

## 50. CREATE — Insert Data

```python
insert_sql = """
INSERT INTO customers
(customer_id, customer_name, city, email, age)
VALUES
(101, 'Karthi', 'Chennai', 'karthi@gmail.com', 30)
"""

with engine.begin() as connection:
    connection.execute(
        text(insert_sql)
    )
```

***

## 51. Insert Multiple Records

A better approach is parameterized SQL.

```python
insert_sql = text("""
    INSERT INTO customers
    (customer_id, customer_name, city, email, age)
    VALUES
    (:customer_id, :customer_name, :city, :email, :age)
""")
```

Then:

```python
data = [
    {
        "customer_id": 102,
        "customer_name": "Ravi",
        "city": "Bangalore",
        "email": "ravi@gmail.com",
        "age": 28
    },
    {
        "customer_id": 103,
        "customer_name": "Arun",
        "city": "Coimbatore",
        "email": "arun@gmail.com",
        "age": 35
    }
]
```

Execute:

```python
with engine.begin() as connection:

    connection.execute(
        insert_sql,
        data
    )
```

***

## 52. Why Use Parameters?

Avoid:

```python
sql = f"""
INSERT INTO customers
VALUES ({customer_id}, '{name}')
"""
```

Prefer:

```python
text("""
INSERT INTO customers
VALUES (:customer_id, :name)
""")
```

Parameter binding is safer and avoids many SQL injection and quoting problems.

***

## 53. READ — SELECT

```python
select_sql = text("""
    SELECT *
    FROM customers
""")
```

Execute:

```python
with engine.connect() as connection:

    result = connection.execute(
        select_sql
    )

    for row in result:
        print(row)
```

***

## 54. Read One Record

```python
select_sql = text("""
    SELECT *
    FROM customers
    WHERE customer_id = :customer_id
""")

with engine.connect() as connection:

    result = connection.execute(
        select_sql,
        {
            "customer_id": 101
        }
    )

    row = result.fetchone()

    print(row)
```

***

## 55. Read All Records

```python
with engine.connect() as connection:

    result = connection.execute(
        text("SELECT * FROM customers")
    )

    rows = result.fetchall()

    for row in rows:
        print(row)
```

***

## 56. Read Using Pandas

This is where SQLAlchemy becomes extremely powerful for Data Engineering.

```python
import pandas as pd

df = pd.read_sql(
    "SELECT * FROM customers",
    engine
)

print(df)
```

Now:

```
Database
   ↓
SQLAlchemy
   ↓
Pandas
   ↓
DataFrame
```

This is the bridge between database ETL and Pandas.

***

## 57. UPDATE

```python
update_sql = text("""
    UPDATE customers
    SET city = :city
    WHERE customer_id = :customer_id
""")
```

Execute:

```python
with engine.begin() as connection:

    connection.execute(
        update_sql,
        {
            "city": "Chennai",
            "customer_id": 102
        }
    )
```

***

## 58. DELETE

```python
delete_sql = text("""
    DELETE FROM customers
    WHERE customer_id = :customer_id
""")
```

Execute:

```python
with engine.begin() as connection:

    connection.execute(
        delete_sql,
        {
            "customer_id": 103
        }
    )
```

***

## 59. CRUD Summary

```
CREATE
    ↓
INSERT

READ
    ↓
SELECT

UPDATE
    ↓
UPDATE

DELETE
    ↓
DELETE
```

***

## 60. Transaction Handling

Database operations should generally be managed through transactions.

Example:

```python
with engine.begin() as connection:

    connection.execute(
        text("""
            INSERT INTO customers
            VALUES (
                104,
                'Priya',
                'Mumbai',
                'priya@gmail.com',
                26
            )
        """)
    )
```

If successful:

```
COMMIT
```

If an error occurs:

```
ROLLBACK
```

The context manager handles the transaction lifecycle.

***

## 61. Creating an Orders Table

```python
create_orders = text("""
CREATE TABLE orders (
    order_id INTEGER PRIMARY KEY,
    customer_id INTEGER,
    order_date DATE,
    amount DECIMAL(10, 2),
    status VARCHAR(30)
)
""")

with engine.begin() as connection:
    connection.execute(create_orders)
```

***

## 62. Load CSV/Excel Data into a Database Table

This is where our two parts connect.

Suppose:

```python
final_df
```

contains cleaned data.

We can load it into SQL using:

```python
final_df.to_sql(
    "clean_orders",
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
Database Table
```

***

## 63. `to_sql()` Parameters

```python
df.to_sql(
    name="clean_orders",
    con=engine,
    if_exists="replace",
    index=False
)
```

Important options:

```
append
replace
fail
```

#### `append`

Add rows:

```python
if_exists="append"
```

#### `replace`

Delete existing table and recreate:

```python
if_exists="replace"
```

#### `fail`

Raise an error if the table already exists:

```python
if_exists="fail"
```

***

## 64. Read Database Table into Pandas

```python
df = pd.read_sql(
    "SELECT * FROM clean_orders",
    engine
)
```

***

## 65. Part 2 ETL Architecture

Now we can build:

```
Source Table
     ↓
SQLAlchemy
     ↓
pd.read_sql()
     ↓
Pandas DataFrame
     ↓
Cleaning
     ↓
Validation
     ↓
Transformation
     ↓
Pandas DataFrame
     ↓
to_sql()
     ↓
Target Table
```

This is our second ETL pipeline.

***

## 66. Source Table

Let's create a raw source table.

```python
create_source = text("""
CREATE TABLE raw_orders (
    order_id INTEGER,
    customer_name VARCHAR(100),
    city VARCHAR(100),
    amount VARCHAR(50),
    order_date VARCHAR(50),
    status VARCHAR(50)
)
""")

with engine.begin() as connection:

    connection.execute(
        create_source
    )
```

***

## 67. Insert Dirty Data

```python
raw_orders = [
    {
        "order_id": 1001,
        "customer_name": " Karthi ",
        "city": "chennai",
        "amount": "500",
        "order_date": "2026-09-01",
        "status": "delivered"
    },
    {
        "order_id": 1002,
        "customer_name": " Ravi ",
        "city": "BANGALORE",
        "amount": "700",
        "order_date": "2026-09-02",
        "status": "Delivered"
    },
    {
        "order_id": 1003,
        "customer_name": "ARUN",
        "city": "Coimbatore",
        "amount": "INVALID",
        "order_date": "2026-09-03",
        "status": "pending"
    },
    {
        "order_id": 1004,
        "customer_name": None,
        "city": "Mumbai",
        "amount": "1200",
        "order_date": "INVALID DATE",
        "status": "cancelled"
    }
]
```

Insert:

```python
insert_sql = text("""
INSERT INTO raw_orders (
    order_id,
    customer_name,
    city,
    amount,
    order_date,
    status
)
VALUES (
    :order_id,
    :customer_name,
    :city,
    :amount,
    :order_date,
    :status
)
""")

with engine.begin() as connection:

    connection.execute(
        insert_sql,
        raw_orders
    )
```

***

## 68. Extract from Database

```python
raw_df = pd.read_sql(
    "SELECT * FROM raw_orders",
    engine
)
```

Now:

```python
print(raw_df)
```

***

## 69. Inspect the Data

```python
print(raw_df.info())
```

Also:

```python
print(raw_df.isna().sum())
```

***

## 70. Clean the Data

```python
df = raw_df.copy()
```

Clean names:

```python
df["customer_name"] = (
    df["customer_name"]
    .str.strip()
    .str.title()
)
```

Clean cities:

```python
df["city"] = (
    df["city"]
    .str.strip()
    .str.title()
)
```

Clean status:

```python
df["status"] = (
    df["status"]
    .str.strip()
    .str.title()
)
```

***

## 71. Convert Amount

```python
df["amount"] = pd.to_numeric(
    df["amount"],
    errors="coerce"
)
```

***

## 72. Convert Date

```python
df["order_date"] = pd.to_datetime(
    df["order_date"],
    errors="coerce"
)
```

***

## 73. Handle Invalid Data

```python
df = df.dropna(
    subset=[
        "customer_name",
        "amount",
        "order_date"
    ]
)
```

***

## 74. Add Derived Column

```python
df["tax"] = (
    df["amount"] * 0.18
)
```

```python
df["total_amount"] = (
    df["amount"] +
    df["tax"]
)
```

***

## 75. Load to Another Table

```python
df.to_sql(
    "clean_orders",
    engine,
    if_exists="replace",
    index=False
)
```

Now:

```
raw_orders
     ↓
Extract
     ↓
Pandas
     ↓
Clean
     ↓
Transform
     ↓
clean_orders
```

***

## 76. Verify Target Table

```python
result = pd.read_sql(
    "SELECT * FROM clean_orders",
    engine
)

print(result)
```

***

## 77. Complete Database ETL

```python
import pandas as pd
from sqlalchemy import create_engine, text

# -----------------------------------
# 1. DATABASE CONNECTION
# -----------------------------------

engine = create_engine(
    "sqlite:///etl_demo.db"
)


# -----------------------------------
# 2. EXTRACT
# -----------------------------------

raw_df = pd.read_sql(
    "SELECT * FROM raw_orders",
    engine
)


# -----------------------------------
# 3. COPY DATA
# -----------------------------------

df = raw_df.copy()


# -----------------------------------
# 4. CLEAN STRINGS
# -----------------------------------

df["customer_name"] = (
    df["customer_name"]
    .str.strip()
    .str.title()
)

df["city"] = (
    df["city"]
    .str.strip()
    .str.title()
)

df["status"] = (
    df["status"]
    .str.strip()
    .str.title()
)


# -----------------------------------
# 5. CONVERT TYPES
# -----------------------------------

df["amount"] = pd.to_numeric(
    df["amount"],
    errors="coerce"
)

df["order_date"] = pd.to_datetime(
    df["order_date"],
    errors="coerce"
)


# -----------------------------------
# 6. REMOVE INVALID RECORDS
# -----------------------------------

df = df.dropna(
    subset=[
        "customer_name",
        "amount",
        "order_date"
    ]
)


# -----------------------------------
# 7. TRANSFORM
# -----------------------------------

df["tax"] = (
    df["amount"] * 0.18
)

df["total_amount"] = (
    df["amount"] +
    df["tax"]
)


# -----------------------------------
# 8. LOAD
# -----------------------------------

df.to_sql(
    "clean_orders",
    engine,
    if_exists="replace",
    index=False
)


# -----------------------------------
# 9. VERIFY
# -----------------------------------

result = pd.read_sql(
    "SELECT * FROM clean_orders",
    engine
)

print(result)
```

***

## 78. Complete ETL Architecture — Part 2

```
                  DATABASE
                     │
                     ↓
                raw_orders
                     │
                     ↓
                SQLAlchemy
                     │
                     ↓
                pd.read_sql()
                     │
                     ↓
                DataFrame
                     │
        ┌────────────┼─────────────┐
        ↓            ↓             ↓
     Cleaning    Validation    Transform
        │            │             │
        └────────────┼─────────────┘
                     ↓
                Clean DataFrame
                     │
                     ↓
                 to_sql()
                     │
                     ↓
                clean_orders
```

***

## 79. CRUD + ETL Together

Now the overall database workflow becomes:

```
CREATE
   ↓
Create Database/Table

INSERT
   ↓
Load Raw Data

READ
   ↓
Extract Data

UPDATE
   ↓
Modify Data

DELETE
   ↓
Remove Data

ETL
   ↓
Read
 ↓
Pandas
 ↓
Clean
 ↓
Transform
 ↓
Write
```

***

## 80. Using SQLAlchemy with MySQL

Once SQLite is understood, we can move to MySQL.

Install:

```python
pip install sqlalchemy pymysql
```

Connection format:

```python
engine = create_engine(
    "mysql+pymysql://username:password@localhost:3306/database_name"
)
```

For example:

```python
engine = create_engine(
    "mysql+pymysql://root:password@localhost:3306/company_db"
)
```

The structure is:

```
mysql+pymysql
       ↓
database driver

username
       ↓
database username

password
       ↓
database password

localhost
       ↓
database server

3306
       ↓
MySQL port

company_db
       ↓
database name
```

***

## 81. Connecting to MySQL

```python
with engine.connect() as connection:

    result = connection.execute(
        text("SELECT 1")
    )

    print(result.scalar())
```

If the connection works:

```
1
```

***

## 82. MySQL ETL Architecture

In real Data Engineering:

```
CSV
Excel
API
Other DB
 │
 └──────────────┐
                ↓
             Pandas
                ↓
             Cleaning
                ↓
             Validation
                ↓
             Transform
                ↓
          SQLAlchemy
                ↓
             MySQL
                ↓
           Target Table
```

Or:

```
MySQL Source Table
       ↓
   SQLAlchemy
       ↓
    Pandas
       ↓
    Clean
       ↓
 Transform
       ↓
 SQLAlchemy
       ↓
MySQL Target Table
```

***

## 83. SQLAlchemy ORM — Introduction

So far we used:

```python
text()
```

to execute SQL.

That is SQLAlchemy Core style.

SQLAlchemy also provides ORM.

ORM means:

```
Object Relational Mapping
```

It maps:

```
Python Class
      ↕
Database Table
```

***

## 84. Simple ORM Model

```python
from sqlalchemy.orm import DeclarativeBase
from sqlalchemy.orm import Mapped
from sqlalchemy.orm import mapped_column
from sqlalchemy import Integer, String

class Base(DeclarativeBase):
    pass
```

Create model:

```python
class Customer(Base):

    __tablename__ = "customers"

    customer_id: Mapped[int] = mapped_column(
        Integer,
        primary_key=True
    )

    customer_name: Mapped[str] = mapped_column(
        String(100)
    )

    city: Mapped[str] = mapped_column(
        String(100)
    )
```

***

## 85. Create ORM Tables

```python
Base.metadata.create_all(engine)
```

This creates the table if it does not already exist.

***

## 86. ORM Session

```python
from sqlalchemy.orm import Session

with Session(engine) as session:
    ...
```

A session manages ORM operations and transactions.

***

## 87. ORM Create

```python
customer = Customer(
    customer_id=101,
    customer_name="Karthi",
    city="Chennai"
)
```

Add:

```python
with Session(engine) as session:

    session.add(customer)

    session.commit()
```

***

## 88. ORM Read

```python
from sqlalchemy import select

with Session(engine) as session:

    stmt = select(Customer)

    customers = session.scalars(stmt).all()

    for customer in customers:
        print(
            customer.customer_id,
            customer.customer_name,
            customer.city
        )
```

***

## 89. ORM Update

```python
with Session(engine) as session:

    customer = session.get(
        Customer,
        101
    )

    customer.city = "Bangalore"

    session.commit()
```

***

## 90. ORM Delete

```python
with Session(engine) as session:

    customer = session.get(
        Customer,
        101
    )

    session.delete(customer)

    session.commit()
```

***

## 91. Core vs ORM

| SQLAlchemy Core             | SQLAlchemy ORM                        |
| --------------------------- | ------------------------------------- |
| SQL-focused                 | Object-focused                        |
| Tables                      | Classes                               |
| `text()`                    | ORM models                            |
| `Connection`                | `Session`                             |
| Direct SQL                  | Python objects                        |
| Excellent for SQL-heavy ETL | Excellent for application development |

For Data Engineering, Core is very useful because you will often work directly with SQL and DataFrames.

***

## 92. Recommended Learning Path

For your Python Data Engineering course, learn SQLAlchemy in this order:

```
SQL Refresher
    ↓
Database Concepts
    ↓
SQLAlchemy Installation
    ↓
create_engine()
    ↓
Connection
    ↓
text()
    ↓
SELECT
    ↓
INSERT
    ↓
UPDATE
    ↓
DELETE
    ↓
Transactions
    ↓
Parameterized Queries
    ↓
Pandas read_sql()
    ↓
Pandas to_sql()
    ↓
Database ETL
    ↓
MySQL
    ↓
SQLAlchemy ORM
    ↓
Sessions
    ↓
Models
    ↓
Advanced ORM
```

***

## 93. Final Combined Project

Eventually, you should build one project containing **both parts**.

```
                    MINI DATA PIPELINE
                           │
            ┌──────────────┴──────────────┐
            │                             │
         Part 1                         Part 2
            │                             │
      Excel / CSV                    Database
            │                             │
            ↓                             ↓
        Pandas                       SQLAlchemy
            │                             │
            ↓                             ↓
         Extract                      Extract
            │                             │
            ↓                             ↓
         Clean                         Pandas
            │                             │
            ↓                             ↓
       Transform                      Clean
            │                             │
            ↓                             ↓
        Validate                    Transform
            │                             │
            ↓                             ↓
      Excel / CSV                      SQL
                                        │
                                        ↓
                                   Target Table
```

***

## 94. The Final Data Engineering Pattern

By the time you complete this section, you should understand this pattern:

```
                     DATA SOURCES
                          │
          ┌───────────────┼────────────────┐
          │               │                │
        CSV             Excel           Database
          │               │                │
          └───────────────┼────────────────┘
                          ↓
                       Extract
                          ↓
                     Pandas DataFrame
                          ↓
                  Data Profiling
                          ↓
                       Cleaning
                          ↓
                  Data Validation
                          ↓
                    Transformation
                          ↓
                  Joining / Aggregation
                          ↓
                      Final Data
                          │
             ┌────────────┼────────────┐
             ↓            ↓            ↓
            CSV          Excel       Database
             │            │            │
             ↓            ↓            ↓
          Output        Output       Target
```

***

## 95. Important Concepts to Remember

### Part 1 — Pandas ETL

```python
pd.read_csv()
pd.read_excel()

df.info()
df.describe()
df.isna()
df.dropna()
df.fillna()
df.drop_duplicates()

df["column"].str.strip()
df["column"].str.lower()
df["column"].str.upper()

pd.to_numeric()
pd.to_datetime()

pd.merge()

df.to_csv()
df.to_excel()
```

***

## 96. Part 2 — SQLAlchemy

Important functions/classes:

```python
create_engine()

text()

engine.connect()

engine.begin()

connection.execute()

pd.read_sql()

df.to_sql()
```

CRUD:

```
INSERT
SELECT
UPDATE
DELETE
```

ORM:

```python
DeclarativeBase
Mapped
mapped_column
Session
select
```

***

## 97. Final ETL Mental Model

Remember this simple pattern:

```
EXTRACT
   ↓
Get data from somewhere
   ↓
TRANSFORM
   ↓
Clean
Validate
Convert
Join
Calculate
   ↓
LOAD
   ↓
Store processed data
```

For files:

```
CSV / Excel
    ↓
Pandas
    ↓
Clean
    ↓
Excel / CSV
```

For databases:

```
Database Table
    ↓
SQLAlchemy
    ↓
Pandas
    ↓
Clean
    ↓
SQLAlchemy
    ↓
Target Table
```

This is the foundation for the larger Data Engineering concepts that come later:

```
Python
   ↓
Pandas
   ↓
SQL
   ↓
SQLAlchemy
   ↓
ETL
   ↓
PySpark
   ↓
Data Lakes
   ↓
Data Warehouses
   ↓
Cloud
   ↓
Databricks
```

The next practical step should be to take your actual **Excel + CSV files**, inspect their columns and data-quality issues, and build the Part 1 ETL around their real structure rather than using the sample columns above.
