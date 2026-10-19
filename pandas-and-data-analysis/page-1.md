# Page

## Pandas

> Imported from [`python/Pandas/Pandas.ipynb`](https://github.com/karthikeyanm-dev/DE-Notes/blob/main/python/Pandas/Pandas.ipynb). Code outputs are omitted; run the source notebook for live results.

## Pandas in Python

Pandas is one of the most important Python libraries for **Data Analysis, Data Engineering, Data Cleaning, ETL, and Data Processing**.

In this section, we will learn Pandas from the absolute basics and gradually move toward advanced Data Engineering usage.

***

## 1. What is Pandas?

**Pandas** is a Python library used to work with structured and tabular data.

For example:

```
Customer_ID   Name       City        Amount
101           Karthi     Chennai     500
102           Ravi       Bangalore   700
103           Arun       Coimbatore  900
```

This type of data can be easily handled using Pandas.

Pandas provides tools for:

```
Reading data
Writing data
Cleaning data
Filtering data
Sorting data
Transforming data
Grouping data
Joining data
Handling missing values
Removing duplicates
Working with dates
Working with strings
Aggregating data
ETL processing
```

***

## 2. Why Pandas is Important for Data Engineering

A Data Engineer frequently receives data in formats such as:

```
CSV
JSON
Excel
Database tables
API responses
Parquet
```

Pandas can act as a processing layer:

```
Raw Data
   ↓
Pandas
   ↓
Read
   ↓
Inspect
   ↓
Clean
   ↓
Transform
   ↓
Validate
   ↓
Aggregate
   ↓
Export
```

For example:

```
raw_orders.csv
       ↓
   pandas.read_csv()
       ↓
   DataFrame
       ↓
   Remove duplicates
       ↓
   Handle missing values
       ↓
   Convert data types
       ↓
   Standardize strings
       ↓
   Calculate revenue
       ↓
   Save clean_orders.csv
```

***

## 3. Installing Pandas

If Pandas is not installed:

```python
pip install pandas
```

In Jupyter Notebook:

```python
!pip install pandas
```

Usually, Anaconda/Jupyter environments already have Pandas installed.

***

## 4. Importing Pandas

```python
import pandas as pd
```

`pd` is the conventional alias.

Instead of:

```python
pandas.DataFrame()
```

we normally write:

```python
pd.DataFrame()
```

***

## 5. What is a DataFrame?

A **DataFrame** is the main Pandas data structure.

It represents data in rows and columns.

Example:

```
   name      age       city
0  Karthi    30        Chennai
1  Ravi      28        Bangalore
2  Arun      32        Coimbatore
```

Think of a DataFrame as similar to:

```
Excel Sheet
SQL Table
Database Table
```

***

## 6. What is a Series?

A **Series** is a one-dimensional labeled collection of data.

Example:

```python
import pandas as pd

marks = pd.Series([80, 90, 75, 88])

print(marks)
```

Output:

```
0    80
1    90
2    75
3    88
dtype: int64
```

A Series contains:

```
Index
Value
```

***

## 7. DataFrame vs Series

```
Series
   ↓
One column

DataFrame
   ↓
Multiple columns
```

Example:

```python
marks = pd.Series([80, 90, 75])
```

DataFrame:

```python
students = pd.DataFrame({
    "name": ["Karthi", "Ravi", "Arun"],
    "marks": [80, 90, 75]
})
```

***

## 8. Creating a DataFrame

The easiest way is from a dictionary.

```python
import pandas as pd

data = {
    "name": ["Karthi", "Ravi", "Arun"],
    "age": [30, 28, 32],
    "city": ["Chennai", "Bangalore", "Coimbatore"]
}

df = pd.DataFrame(data)

print(df)
```

Output:

```
    name  age        city
0  Karthi   30     Chennai
1    Ravi   28   Bangalore
2    Arun   32  Coimbatore
```

***

## 9. Understanding the Index

Notice:

```
0
1
2
```

These are the DataFrame's default indexes.

The index identifies each row.

```python
print(df.index)
```

Output:

```
RangeIndex(start=0, stop=3, step=1)
```

***

## 10. Understanding Columns

```python
print(df.columns)
```

Output:

```
Index(['name', 'age', 'city'], dtype='object')
```

To convert column names into a list:

```python
print(df.columns.tolist())
```

Output:

```
['name', 'age', 'city']
```

***

## 11. Basic DataFrame Properties

Important properties:

```python
df.shape
df.columns
df.index
df.dtypes
df.size
```

***

## 12. `shape`

`shape` returns:

```
(rows, columns)
```

Example:

```python
print(df.shape)
```

Output:

```
(3, 3)
```

Meaning:

```
3 rows
3 columns
```

***

## 13. `size`

```python
print(df.size)
```

Output:

```
9
```

Because:

```
3 rows × 3 columns = 9 values
```

***

## 14. `dtypes`

```python
print(df.dtypes)
```

Output may be:

```
name    object
age      int64
city    object
dtype: object
```

This tells us the data type of each column.

***

## 15. `head()`

`head()` shows the first rows.

```python
print(df.head())
```

By default, it shows 5 rows.

We can specify the number:

```python
print(df.head(2))
```

***

## 16. `tail()`

`tail()` shows the last rows.

```python
print(df.tail())
```

Or:

```python
print(df.tail(2))
```

***

## 17. `info()`

One of the most important Pandas functions.

```python
df.info()
```

It provides information about:

```
Rows
Columns
Column names
Non-null values
Data types
Memory usage
```

Example:

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 3 entries, 0 to 2
Data columns (total 3 columns):
...
```

For Data Cleaning, `info()` should be one of the first commands you run.

***

## 18. `describe()`

`describe()` generates statistical information.

```python
print(df.describe())
```

For numeric columns it can show:

```
count
mean
std
min
25%
50%
75%
max
```

Example:

```python
data = {
    "age": [20, 25, 30, 35, 40],
    "salary": [20000, 30000, 40000, 50000, 60000]
}

df = pd.DataFrame(data)

print(df.describe())
```

***

## 19. `describe(include="all")`

We can request statistics for all columns.

```python
print(df.describe(include="all"))
```

This is useful when the DataFrame contains both numeric and categorical columns.

***

## 20. Selecting a Column

```python
df["name"]
```

This returns a Series.

Example:

```python
print(df["name"])
```

Output:

```
0    Karthi
1      Ravi
2      Arun
Name: name, dtype: object
```

***

## 21. Selecting Multiple Columns

```python
df[["name", "city"]]
```

Example:

```python
print(df[["name", "city"]])
```

Output:

```
    name        city
0  Karthi     Chennai
1    Ravi   Bangalore
2    Arun  Coimbatore
```

Important:

```python
df["name"]
```

returns a Series.

```python
df[["name"]]
```

returns a DataFrame.

***

## 22. Selecting Rows with `iloc`

`iloc` selects data based on **integer position**.

```python
df.iloc[0]
```

Returns the first row.

```python
df.iloc[1]
```

Returns the second row.

***

## 23. `iloc` Row Slicing

```python
df.iloc[0:2]
```

Gets rows:

```
0
1
```

***

## 24. `iloc` Row and Column

```python
df.iloc[0, 1]
```

Meaning:

```
row position 0
column position 1
```

***

## 25. `iloc` with Multiple Rows and Columns

```python
df.iloc[0:3, 0:2]
```

Meaning:

```
Rows → 0 to 2
Columns → 0 to 1
```

***

## 26. Selecting Data with `loc`

`loc` selects using **labels**.

Example:

```python
df.loc[0]
```

With columns:

```python
df.loc[0, "name"]
```

Output:

```
Karthi
```

***

## 27. `loc` with Multiple Columns

```python
df.loc[:, ["name", "city"]]
```

`:` means:

```
all rows
```

***

## 28. `iloc` vs `loc`

| `iloc`                 | `loc`                  |
| ---------------------- | ---------------------- |
| Position-based         | Label-based            |
| Uses integer positions | Uses row/column labels |
| `df.iloc[0]`           | `df.loc[0]`            |
| `df.iloc[0,1]`         | `df.loc[0,"name"]`     |

A very important concept in Pandas.

***

## 29. Filtering Data

Suppose:

```python
df = pd.DataFrame({
    "name": ["Karthi", "Ravi", "Arun", "Priya"],
    "age": [30, 25, 35, 28]
})
```

Filter age greater than 28:

```python
result = df[df["age"] > 28]

print(result)
```

Output:

```
    name  age
0  Karthi   30
2    Arun   35
```

***

## 30. Multiple Conditions

Use:

```
&
|
~
```

Example:

```python
result = df[(df["age"] > 25) & (df["age"] < 35)]

print(result)
```

Output:

```
    name  age
0  Karthi   30
3  Priya   28
```

Important:

Use:

```python
&
```

instead of:

```python
and
```

when working with Pandas Series.

***

## 31. OR Condition

```python
result = df[(df["age"] < 26) | (df["age"] > 34)]
```

This means:

```
age < 26 OR age > 34
```

***

## 32. NOT Condition

```python
result = df[~(df["age"] > 30)]
```

***

## 33. `query()`

Pandas provides another readable way to filter.

```python
result = df.query("age > 28")
```

Multiple conditions:

```python
result = df.query("age > 25 and age < 35")
```

***

## 34. Checking Values with `isin()`

Suppose:

```python
df = pd.DataFrame({
    "city": ["Chennai", "Bangalore", "Mumbai", "Chennai"]
})
```

We want:

```
Chennai
Mumbai
```

Use:

```python
result = df[df["city"].isin(["Chennai", "Mumbai"])]

print(result)
```

***

## 35. `notna()` and `isna()`

These are extremely important for missing-value handling.

```python
df["salary"].isna()
```

Checks for missing values.

```python
df["salary"].notna()
```

Checks for non-missing values.

***

## 36. Creating a New Column

```python
df["salary"] = [30000, 40000, 50000, 35000]
```

***

## 37. Calculated Columns

Suppose:

```python
df["salary"]
```

contains salary.

We can calculate annual bonus:

```python
df["bonus"] = df["salary"] * 0.10
```

Now:

```
salary  bonus
30000   3000
40000   4000
50000   5000
35000   3500
```

***

## 38. Updating a Column

```python
df["age"] = df["age"] + 1
```

Every age is increased by 1.

***

## 39. Renaming Columns

```python
df = df.rename(columns={
    "name": "customer_name",
    "city": "customer_city"
})
```

***

## 40. Renaming All Columns

```python
df.columns = ["customer_name", "age", "city"]
```

Use this only when the number of names exactly matches the number of columns.

***

## 41. Dropping a Column

```python
df = df.drop(columns=["age"])
```

***

## 42. Dropping Multiple Columns

```python
df = df.drop(columns=["age", "city"])
```

***

## 43. Dropping Rows

```python
df = df.drop(index=2)
```

This removes row index `2`.

Multiple rows:

```python
df = df.drop(index=[1, 3])
```

***

## 44. `sort_values()`

Sort by a column:

```python
df = df.sort_values("age")
```

Descending:

```python
df = df.sort_values("age", ascending=False)
```

***

## 45. Sorting by Multiple Columns

```python
df = df.sort_values(
    by=["city", "age"],
    ascending=[True, False]
)
```

***

## 46. Resetting Index

After filtering or sorting, we may want a clean index.

```python
df = df.reset_index(drop=True)
```

Example:

Before:

```
0
3
5
```

After:

```
0
1
2
```

***

## 47. Unique Values

```python
df["city"].unique()
```

Example output:

```
array(['Chennai', 'Bangalore', 'Mumbai'], dtype=object)
```

***

## 48. Count Unique Values

```python
df["city"].nunique()
```

***

## 49. Frequency of Values

`value_counts()` is extremely useful.

```python
df["city"].value_counts()
```

Possible output:

```
Chennai      15
Bangalore    10
Mumbai        8
```

This is very useful for Data Profiling.

***

## 50. Missing Values

Real-world data commonly contains:

```
NaN
None
NaT
empty values
```

Example:

```python
df = pd.DataFrame({
    "name": ["Karthi", "Ravi", "Arun"],
    "age": [30, None, 28]
})
```

***

## 51. Detect Missing Values

```python
print(df.isna())
```

For total missing values by column:

```python
print(df.isna().sum())
```

Possible output:

```
name    0
age     1
dtype: int64
```

***

## 52. Percentage of Missing Values

```python
missing_percentage = df.isna().mean() * 100

print(missing_percentage)
```

This is useful when profiling large datasets.

***

## 53. `dropna()`

Remove rows containing missing values.

```python
df = df.dropna()
```

***

## 54. Drop Rows Only When a Specific Column is Missing

```python
df = df.dropna(subset=["email"])
```

This means:

```
Remove rows where email is missing.
```

***

## 55. `fillna()`

Instead of deleting missing data, we can replace it.

```python
df["age"] = df["age"].fillna(0)
```

***

## 56. Fill with Mean

```python
df["salary"] = df["salary"].fillna(df["salary"].mean())
```

***

## 57. Fill with Median

```python
df["salary"] = df["salary"].fillna(df["salary"].median())
```

Median is often useful when data contains extreme values.

***

## 58. Forward Fill

```python
df["city"] = df["city"].ffill()
```

It fills a missing value using the previous available value.

***

## 59. Backward Fill

```python
df["city"] = df["city"].bfill()
```

It uses the next available value.

***

## 60. Duplicate Records

Check duplicates:

```python
df.duplicated()
```

Count duplicates:

```python
df.duplicated().sum()
```

***

## 61. Remove Duplicates

```python
df = df.drop_duplicates()
```

***

## 62. Duplicate Based on Specific Columns

```python
df = df.drop_duplicates(subset=["customer_id"])
```

Keep the last record:

```python
df = df.drop_duplicates(
    subset=["customer_id"],
    keep="last"
)
```

***

## 63. Data Type Conversion

Use:

```python
astype()
```

Example:

```python
df["age"] = df["age"].astype(int)
```

For numeric conversion where bad values may exist:

```python
df["amount"] = pd.to_numeric(
    df["amount"],
    errors="coerce"
)
```

Bad values become:

```
NaN
```

instead of raising an exception.

***

## 64. `to_datetime()`

Convert a column to datetime:

```python
df["order_date"] = pd.to_datetime(df["order_date"])
```

If invalid dates are present:

```python
df["order_date"] = pd.to_datetime(
    df["order_date"],
    errors="coerce"
)
```

***

## 65. Working with Datetime

Suppose:

```python
df["order_date"]
```

is datetime.

We can extract:

```python
df["year"] = df["order_date"].dt.year
df["month"] = df["order_date"].dt.month
df["day"] = df["order_date"].dt.day
df["weekday"] = df["order_date"].dt.day_name()
```

***

## 66. Date Filtering

```python
result = df[
    df["order_date"] >= "2026-01-01"
]
```

***

## 67. String Operations

Pandas provides the `.str` accessor.

Example:

```python
df["name"] = df["name"].str.upper()
```

Other useful operations:

```python
.str.lower()
.str.upper()
.str.strip()
.str.replace()
.str.contains()
.str.startswith()
.str.endswith()
```

***

## 68. String Cleaning

Suppose:

```
"  Karthi  "
"  RAVI "
" Arun"
```

Use:

```python
df["name"] = df["name"].str.strip()
```

Then standardize:

```python
df["name"] = df["name"].str.title()
```

***

## 69. `str.contains()`

```python
result = df[
    df["email"].str.contains("@gmail.com", na=False)
]
```

***

## 70. Regex in Pandas

Since we already learned Regex, Pandas can combine Regex with string operations.

Example:

```python
df["email_domain"] = df["email"].str.extract(
    r"@(.+)$"
)
```

If:

```
karthi@gmail.com
ravi@yahoo.com
```

output:

```
gmail.com
yahoo.com
```

***

## 71. Mapping Values

Suppose:

```python
mapping = {
    "C": "Chennai",
    "B": "Bangalore",
    "M": "Mumbai"
}
```

Use:

```python
df["city"] = df["city_code"].map(mapping)
```

***

## 72. `replace()`

Replace individual values:

```python
df["status"] = df["status"].replace({
    "Y": "Active",
    "N": "Inactive"
})
```

***

## 73. `apply()`

`apply()` allows us to apply a function to values.

Example:

```python
df["amount"] = df["amount"].apply(
    lambda x: x * 1.18
)
```

This adds 18%.

***

## 74. `apply()` with a Normal Function

```python
def calculate_tax(amount):
    return amount * 0.18


df["tax"] = df["amount"].apply(calculate_tax)
```

***

## 75. Prefer Vectorized Operations

Instead of:

```python
df["tax"] = df["amount"].apply(
    lambda x: x * 0.18
)
```

Prefer:

```python
df["tax"] = df["amount"] * 0.18
```

Vectorized Pandas operations are generally faster and more idiomatic.

***

## 76. Aggregation Functions

Important functions:

```python
sum()
mean()
median()
min()
max()
count()
std()
```

Example:

```python
print(df["amount"].sum())
print(df["amount"].mean())
print(df["amount"].max())
```

***

## 77. `groupby()`

`groupby()` is one of the most important Pandas concepts.

Suppose:

```
city       amount
Chennai    500
Chennai    700
Bangalore  800
Bangalore  900
```

We want total sales by city.

```python
result = df.groupby("city")["amount"].sum()

print(result)
```

Output:

```
Bangalore    1700
Chennai      1200
```

***

## 78. `groupby()` with Multiple Aggregations

```python
result = df.groupby("city")["amount"].agg(
    ["sum", "mean", "min", "max", "count"]
)
```

***

## 79. Grouping by Multiple Columns

```python
result = df.groupby(
    ["city", "product"]
)["amount"].sum()
```

***

## 80. Named Aggregations

For cleaner output:

```python
result = df.groupby("city").agg(
    total_sales=("amount", "sum"),
    average_sales=("amount", "mean"),
    order_count=("order_id", "count")
)

print(result)
```

This is extremely useful in reporting and ETL.

***

## 81. `merge()` — Joining DataFrames

Pandas `merge()` is similar to SQL `JOIN`.

Example:

```python
customers = pd.DataFrame({
    "customer_id": [1, 2, 3],
    "name": ["Karthi", "Ravi", "Arun"]
})

orders = pd.DataFrame({
    "customer_id": [1, 2, 3],
    "amount": [500, 700, 900]
})
```

Join:

```python
result = pd.merge(
    customers,
    orders,
    on="customer_id",
    how="inner"
)
```

***

## 82. Types of `merge()`

SQL-style joins:

```
inner
left
right
outer
cross
```

Example:

```python
pd.merge(
    customers,
    orders,
    on="customer_id",
    how="left"
)
```

***

## 83. `merge()` Equivalent to SQL JOIN

```sql
SELECT *
FROM customers c
INNER JOIN orders o
ON c.customer_id = o.customer_id;
```

Pandas:

```python
pd.merge(
    customers,
    orders,
    on="customer_id",
    how="inner"
)
```

***

## 84. `concat()`

`concat()` combines DataFrames vertically or horizontally.

Example:

```python
df1 = pd.DataFrame({
    "id": [1, 2]
})

df2 = pd.DataFrame({
    "id": [3, 4]
})

result = pd.concat([df1, df2])
```

Output:

```
   id
0   1
1   2
0   3
1   4
```

Reset index:

```python
result = pd.concat(
    [df1, df2],
    ignore_index=True
)
```

Output:

```
   id
0   1
1   2
2   3
3   4
```

***

## 85. Horizontal Concatenation

```python
result = pd.concat(
    [df1, df2],
    axis=1
)
```

Here:

```
axis=0 → rows
axis=1 → columns
```

***

## 86. Pivot Tables

Pivoting is useful for reports.

Example:

```python
result = pd.pivot_table(
    df,
    values="amount",
    index="city",
    columns="product",
    aggfunc="sum",
    fill_value=0
)
```

This can transform transactional data into a reporting structure.

***

## 87. `pivot()` vs `pivot_table()`

`pivot()` requires the combination of index and column values to be unique.

```python
df.pivot(
    index="city",
    columns="product",
    values="amount"
)
```

`pivot_table()` can aggregate duplicate combinations.

```python
pd.pivot_table(
    df,
    index="city",
    columns="product",
    values="amount",
    aggfunc="sum"
)
```

***

## 88. Reading CSV

One of the most important Pandas operations.

```python
df = pd.read_csv("orders.csv")
```

***

## 89. Reading CSV with Options

```python
df = pd.read_csv(
    "orders.csv",
    encoding="utf-8"
)
```

Specify columns:

```python
df = pd.read_csv(
    "orders.csv",
    usecols=["order_id", "customer", "amount"]
)
```

***

## 90. Reading CSV with Missing Values

```python
df = pd.read_csv(
    "orders.csv",
    na_values=["NA", "N/A", "null", ""]
)
```

***

## 91. Reading Large CSV Files

If the CSV is very large:

```python
for chunk in pd.read_csv(
    "large_orders.csv",
    chunksize=10000
):
    print(chunk.shape)
```

This reads the file in chunks instead of loading the entire dataset into memory.

This is extremely important for Data Engineering.

***

## 92. Writing CSV

```python
df.to_csv("output.csv", index=False)
```

Why:

```python
index=False
```

?

Because otherwise Pandas may write the DataFrame index as an extra CSV column.

***

## 93. Reading Excel

```python
df = pd.read_excel("sales.xlsx")
```

Writing:

```python
df.to_excel("output.xlsx", index=False)
```

***

## 94. Reading JSON

```python
df = pd.read_json("data.json")
```

Writing:

```python
df.to_json("output.json", orient="records")
```

***

## 95. Parquet

For modern Data Engineering, Parquet is extremely important.

Read:

```python
df = pd.read_parquet("orders.parquet")
```

Write:

```python
df.to_parquet(
    "orders.parquet",
    index=False
)
```

Parquet is a columnar storage format and is commonly used in data platforms and analytics pipelines.

***

## 96. Sampling Data

For large datasets, inspect a small random sample:

```python
df.sample(10)
```

This is useful for data profiling.

***

## 97. Random Sampling with a Fixed Seed

```python
df.sample(
    10,
    random_state=42
)
```

Using the same `random_state` produces reproducible sampling.

***

## 98. Memory Usage

```python
print(df.memory_usage(deep=True))
```

Total:

```python
print(
    df.memory_usage(deep=True).sum()
)
```

Useful for large datasets.

***

## 99. Converting Strings to Category

Suppose:

```
city
Chennai
Chennai
Bangalore
Chennai
Bangalore
```

For columns with a small number of repeated values:

```python
df["city"] = df["city"].astype("category")
```

This can reduce memory usage in suitable datasets.

***

## 100. DataFrame Copy

This is important.

```python
df2 = df
```

does not create an independent DataFrame.

Both variables refer to the same object.

Use:

```python
df2 = df.copy()
```

when you need an independent copy.

***

## 101. Why `copy()` Matters

```python
df2 = df.copy()

df2["amount"] = df2["amount"] * 2
```

Changes to `df2` do not intentionally modify the original DataFrame.

***

## 102. Conditional Transformation

Suppose:

```python
df["amount"]
```

contains order amounts.

We want:

```
amount > 1000 → High
otherwise     → Normal
```

Use:

```python
import numpy as np

df["order_type"] = np.where(
    df["amount"] > 1000,
    "High",
    "Normal"
)
```

***

## 103. `np.select()`

For multiple conditions:

```python
conditions = [
    df["amount"] >= 5000,
    df["amount"] >= 2000,
    df["amount"] >= 1000
]

choices = [
    "Premium",
    "High",
    "Medium"
]

df["category"] = np.select(
    conditions,
    choices,
    default="Low"
)
```

This is useful for classification logic.

***

## 104. Column Selection with Data Types

Numeric columns:

```python
numeric_df = df.select_dtypes(
    include="number"
)
```

String columns:

```python
string_df = df.select_dtypes(
    include="object"
)
```

***

## 105. Selecting Columns by Pattern

Suppose:

```
customer_name
customer_city
customer_email
order_amount
order_date
```

We can select customer columns:

```python
customer_columns = df.filter(
    regex="^customer_"
)
```

***

## 106. Transposing a DataFrame

```python
df.T
```

Rows become columns and columns become rows.

Useful occasionally for inspection and formatting.

***

## 107. MultiIndex

Pandas supports multiple index levels.

Example:

```python
result = df.groupby(
    ["city", "product"]
)["amount"].sum()
```

The result may have:

```
city       product
Chennai    Laptop
Chennai    Mobile
Bangalore  Laptop
```

This is a MultiIndex.

Convert it back to normal columns:

```python
result = result.reset_index()
```

***

## 108. `reset_index()`

Very important after:

```python
groupby()
```

Example:

```python
result = (
    df.groupby("city")["amount"]
      .sum()
      .reset_index()
)
```

Instead of the city remaining as an index, it becomes a normal column.

***

## 109. Exploding List Columns

Suppose:

```python
df = pd.DataFrame({
    "customer": ["Karthi", "Ravi"],
    "products": [
        ["Laptop", "Mouse"],
        ["Phone", "Charger"]
    ]
})
```

We can use:

```python
result = df.explode("products")
```

Output:

```
  customer products
0  Karthi  Laptop
0  Karthi  Mouse
1  Ravi    Phone
1  Ravi    Charger
```

This is useful when processing nested API/JSON-style data.

***

## 110. JSON / Nested Data

Pandas can normalize nested structures using:

```python
pd.json_normalize()
```

Example:

```python
data = [
    {
        "customer": "Karthi",
        "address": {
            "city": "Chennai",
            "pincode": 600001
        }
    }
]

df = pd.json_normalize(data)

print(df)
```

Output:

```
  customer address.city  address.pincode
0  Karthi   Chennai       600001
```

This is very useful for API data processing.

***

## 111. Combining Pandas with Regex

Example:

```python
df["order_id"] = df["raw_text"].str.extract(
    r"(ORD-\d{4}-\d+)"
)
```

This is a good combination of:

```
Regex
   +
Pandas
   ↓
Data Extraction
```

***

## 112. Basic Data Profiling Workflow

When receiving a new dataset, don't immediately start cleaning.

First inspect it.

```python
df.head()
```

Then:

```python
df.shape
```

Then:

```python
df.info()
```

Then:

```python
df.describe(include="all")
```

Then:

```python
df.isna().sum()
```

Then:

```python
df.duplicated().sum()
```

Then:

```python
df.nunique()
```

Then inspect:

```python
df.sample(10)
```

A common workflow:

```
Load
 ↓
Shape
 ↓
Columns
 ↓
Data Types
 ↓
Missing Values
 ↓
Duplicates
 ↓
Unique Values
 ↓
Outliers
 ↓
Cleaning
```

***

## 113. Data Cleaning Checklist

When cleaning a dataset, check:

```
1. Column names
2. Data types
3. Missing values
4. Duplicate records
5. Invalid values
6. Incorrect spelling
7. Leading/trailing spaces
8. Inconsistent capitalization
9. Invalid dates
10. Invalid numeric values
11. Outliers
12. Wrong formats
13. Null keys
14. Duplicate IDs
15. Business-rule violations
```

***

## 114. A Practical Cleaning Sequence

```python
import pandas as pd

df = pd.read_csv("raw_data.csv")

# Inspect
print(df.shape)
print(df.info())
print(df.head())

# Clean column names
df.columns = (
    df.columns
      .str.strip()
      .str.lower()
      .str.replace(" ", "_")
)

# Remove duplicates
df = df.drop_duplicates()

# Strip string columns
df["name"] = df["name"].str.strip()

# Convert numeric data
df["amount"] = pd.to_numeric(
    df["amount"],
    errors="coerce"
)

# Convert dates
df["order_date"] = pd.to_datetime(
    df["order_date"],
    errors="coerce"
)

# Handle missing values
df["amount"] = df["amount"].fillna(0)

# Reset index
df = df.reset_index(drop=True)

# Save
df.to_csv(
    "clean_data.csv",
    index=False
)
```

This is the beginning of a real cleaning pipeline.

***

## 115. Method Chaining

Instead of writing many separate assignments:

```python
df = df.drop_duplicates()
df = df.reset_index(drop=True)
df = df.sort_values("amount")
```

we can sometimes chain operations:

```python
df = (
    df
    .drop_duplicates()
    .sort_values("amount")
    .reset_index(drop=True)
)
```

This can make data transformations easier to read.

***

## 116. `assign()`

Another way to create columns:

```python
df = df.assign(
    tax=df["amount"] * 0.18,
    total=df["amount"] * 1.18
)
```

***

## 117. `rename()` + `assign()` + `drop()`

These can be combined:

```python
df = (
    df
    .rename(columns={"amt": "amount"})
    .assign(total=lambda x: x["amount"] * 1.18)
    .drop(columns=["old_column"])
)
```

This style is useful for transformation pipelines.

***

## 118. Avoiding Unnecessary `apply()`

This:

```python
df["total"] = df["amount"].apply(
    lambda x: x * 1.18
)
```

is usually less efficient than:

```python
df["total"] = df["amount"] * 1.18
```

Prefer Pandas/NumPy vectorized operations where possible.

***

## 119. Chunk Processing — Practical ETL

For a huge CSV:

```python
import pandas as pd

for chunk in pd.read_csv(
    "huge_orders.csv",
    chunksize=50000
):

    chunk["amount"] = pd.to_numeric(
        chunk["amount"],
        errors="coerce"
    )

    chunk = chunk.dropna(
        subset=["order_id"]
    )

    # Process chunk here
```

Conceptually:

```
Huge File
   ↓
Chunk 1
   ↓
Clean
   ↓
Load

Chunk 2
   ↓
Clean
   ↓
Load

Chunk 3
   ↓
Clean
   ↓
Load
```

This avoids requiring the entire file to fit in memory.

***

## 120. Reading Only Required Columns

Instead of:

```python
df = pd.read_csv("orders.csv")
```

if we only need a few columns:

```python
df = pd.read_csv(
    "orders.csv",
    usecols=[
        "order_id",
        "customer_id",
        "amount"
    ]
)
```

This can reduce memory usage and improve performance.

***

## 121. Specify Data Types

For large files, we can specify types while reading:

```python
df = pd.read_csv(
    "orders.csv",
    dtype={
        "order_id": "string",
        "customer_id": "int64"
    }
)
```

This can help avoid unnecessary type inference and unexpected types.

***

## 122. Selecting Rows with `between()`

Instead of:

```python
df[
    (df["amount"] >= 1000) &
    (df["amount"] <= 5000)
]
```

we can write:

```python
df[
    df["amount"].between(1000, 5000)
]
```

***

## 123. Selecting Rows with `query()`

```python
result = df.query(
    "amount >= 1000 and city == 'Chennai'"
)
```

This can make complex filters easier to read.

***

## 124. `where()` and `mask()`

`where()` keeps values that satisfy a condition.

```python
df["amount"] = df["amount"].where(
    df["amount"] >= 0,
    0
)
```

Negative amounts become:

```
0
```

`mask()` replaces values where the condition is true:

```python
df["amount"] = df["amount"].mask(
    df["amount"] < 0,
    0
)
```

***

## 125. Cumulative Operations

Pandas supports:

```python
df["amount"].cumsum()
df["amount"].cummax()
df["amount"].cummin()
df["amount"].cumprod()
```

Example:

```python
df["running_sales"] = df["amount"].cumsum()
```

***

## 126. Ranking

```python
df["sales_rank"] = df["amount"].rank(
    ascending=False
)
```

***

## 127. Window-Style Operations

For example:

```python
df["rolling_avg"] = (
    df["amount"]
    .rolling(3)
    .mean()
)
```

This calculates a rolling average over three rows.

This becomes useful in time-series analysis.

***

## 128. Resampling Time-Series Data

Suppose dates are the index:

```python
df = df.set_index("order_date")
```

Monthly aggregation:

```python
monthly_sales = df["amount"].resample("ME").sum()
```

This is useful for:

```
Daily → Monthly
Hourly → Daily
Monthly → Quarterly
```

***

## 129. Set an Index

```python
df = df.set_index("customer_id")
```

Now:

```
customer_id
101
102
103
```

becomes the index.

Retrieve:

```python
df.loc[101]
```

***

## 130. Reset the Index

```python
df = df.reset_index()
```

The index becomes a normal column again.

***

## 131. Checking Data Consistency

Example:

```python
print(df["amount"].dtype)
print(df["customer_id"].isna().sum())
print(df["order_id"].duplicated().sum())
```

These checks can be turned into pipeline validation rules.

***

## 132. Basic Data Quality Validation

```python
assert df["order_id"].notna().all()
assert df["amount"].notna().all()
assert (df["amount"] >= 0).all()
```

If an assertion fails, Python raises:

```
AssertionError
```

For production validation, dedicated validation frameworks may be used, but assertions are useful for learning and simple checks.

***

## 133. Pandas + SQL Thinking

There is a strong relationship between SQL and Pandas.

| SQL        | Pandas              |
| ---------- | ------------------- |
| `SELECT`   | `df[...]`           |
| `WHERE`    | Boolean filtering   |
| `GROUP BY` | `groupby()`         |
| `ORDER BY` | `sort_values()`     |
| `JOIN`     | `merge()`           |
| `UNION`    | `concat()`          |
| `DISTINCT` | `drop_duplicates()` |
| `COUNT()`  | `count()`           |
| `SUM()`    | `sum()`             |
| `AVG()`    | `mean()`            |

Example SQL:

```sql
SELECT city, SUM(amount)
FROM orders
GROUP BY city;
```

Pandas:

```python
df.groupby("city")["amount"].sum()
```

***

## 134. Pandas + ETL Thinking

A typical Pandas ETL pipeline:

```
EXTRACT
   ↓
pd.read_csv()
   ↓
TRANSFORM
   ↓
Clean
Validate
Convert
Filter
Join
Aggregate
   ↓
LOAD
   ↓
to_csv()
to_parquet()
to_excel()
```

***

## 135. Advanced Practical Project

Now we will create our own CSV file with intentionally dirty data.

The idea is:

```
Generate Raw CSV
       ↓
Read with Pandas
       ↓
Inspect
       ↓
Clean
       ↓
Validate
       ↓
Transform
       ↓
Save Clean CSV
```

***

## 136. Project Scenario

Imagine a company receives an order file from different source systems.

The raw data contains problems such as:

```
Duplicate orders
Missing customer names
Extra spaces
Different city capitalization
Invalid amounts
Missing email addresses
Invalid dates
Wrong order IDs
```

Our goal is to clean the data using Pandas.

***

## 137. Step 1 — Generate a Dirty CSV File

We can generate the CSV entirely using Python.

```python
import pandas as pd

raw_data = {
    "Order ID": [
        "ORD-1001",
        " ORD-1002 ",
        "ORD-1003",
        "ORD-1004",
        "ORD-1004",
        "ORD-1005",
        "ORD-1006",
        "ORD-1007",
        None,
        "ORD-1009"
    ],

    "Customer Name": [
        "Karthi Kumar",
        "  Ravi Kumar  ",
        "ARUN",
        None,
        "Priya",
        " priya ",
        "Suresh",
        "Kumar",
        "Anitha",
        "  Rahul  "
    ],

    "Email": [
        "karthi@gmail.com",
        "ravi@gmail.com",
        "arun@yahoo.com",
        "priya@gmail.com",
        "priya@gmail.com",
        None,
        "suresh@gmail.com",
        "invalid-email",
        "anitha@gmail.com",
        "rahul@gmail.com"
    ],

    "City": [
        "Chennai",
        " chennai ",
        "BANGALORE",
        "Mumbai",
        "Mumbai",
        " chennai",
        "Bangalore",
        "mumbai",
        None,
        "CHENNAI"
    ],

    "Amount": [
        "500",
        "700",
        "1200",
        "1500",
        "1500",
        "INVALID",
        "900",
        "-200",
        "1000",
        "2500"
    ],

    "Order Date": [
        "2026-09-01",
        "2026-09-02",
        "2026-09-03",
        "2026-09-04",
        "2026-09-04",
        "2026-09-05",
        "2026-09-06",
        "2026-09-07",
        "INVALID DATE",
        "2026-09-09"
    ]
}

raw_df = pd.DataFrame(raw_data)

raw_df.to_csv(
    "raw_orders.csv",
    index=False
)

print("raw_orders.csv created successfully")
```

This creates:

```
raw_orders.csv
```

***

## 138. Step 2 — Read the CSV

```python
import pandas as pd

df = pd.read_csv("raw_orders.csv")

print(df)
```

***

## 139. Step 3 — Inspect the Data

First:

```python
print(df.head())
```

Then:

```python
print(df.shape)
```

Then:

```python
df.info()
```

And:

```python
print(df.describe(include="all"))
```

***

## 140. Step 4 — Check Missing Values

```python
print(df.isna().sum())
```

We may get something like:

```
Order ID          1
Customer Name     1
Email             1
City              1
Amount            0
Order Date        0
```

Note:

Values such as:

```
"INVALID"
"INVALID DATE"
```

are not automatically considered missing because they are strings.

We must clean them.

***

## 141. Step 5 — Clean Column Names

Current columns:

```
Order ID
Customer Name
Email
City
Amount
Order Date
```

Convert them to standard Data Engineering style:

```
order_id
customer_name
email
city
amount
order_date
```

Code:

```python
df.columns = (
    df.columns
      .str.strip()
      .str.lower()
      .str.replace(" ", "_")
)

print(df.columns)
```

***

## 142. Step 6 — Remove Duplicate Records

Check duplicates:

```python
print(df.duplicated().sum())
```

Remove duplicates:

```python
df = df.drop_duplicates()
```

However, in our project, the duplicate is based on `order_id`, so we can perform a business-specific duplicate check:

```python
df = df.drop_duplicates(
    subset=["order_id"],
    keep="first"
)
```

***

## 143. Step 7 — Clean Order IDs

Remove spaces:

```python
df["order_id"] = df["order_id"].str.strip()
```

***

## 144. Step 8 — Clean Customer Names

First remove spaces:

```python
df["customer_name"] = (
    df["customer_name"]
    .str.strip()
)
```

Standardize capitalization:

```python
df["customer_name"] = (
    df["customer_name"]
    .str.title()
)
```

***

## 145. Step 9 — Clean Email

Remove spaces:

```python
df["email"] = df["email"].str.strip()
```

Convert to lowercase:

```python
df["email"] = df["email"].str.lower()
```

***

## 146. Step 10 — Validate Email

We can use Regex.

```python
email_pattern = r"^[\w.-]+@[\w.-]+\.\w+$"

valid_email = df["email"].str.match(
    email_pattern,
    na=False
)

print(valid_email)
```

Invalid emails can be converted to missing values:

```python
df.loc[~valid_email, "email"] = pd.NA
```

***

## 147. Step 11 — Clean Cities

```python
df["city"] = (
    df["city"]
    .str.strip()
    .str.title()
)
```

Now:

```
Chennai
chennai
CHENNAI
 chennai
```

become:

```
Chennai
```

***

## 148. Step 12 — Convert Amount

Currently:

```
500
700
1200
INVALID
```

Convert using:

```python
df["amount"] = pd.to_numeric(
    df["amount"],
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

## 149. Step 13 — Handle Invalid Amounts

First inspect:

```python
print(df[df["amount"].isna()])
```

If invalid amounts should not be accepted, remove them:

```python
df = df.dropna(
    subset=["amount"]
)
```

***

## 150. Step 14 — Remove Negative Amounts

Negative sales amounts are invalid in our business example.

```python
df = df[df["amount"] >= 0]
```

***

## 151. Step 15 — Convert Order Date

```python
df["order_date"] = pd.to_datetime(
    df["order_date"],
    errors="coerce"
)
```

Invalid dates become:

```
NaT
```

`NaT` means:

```
Not a Time
```

***

## 152. Step 16 — Handle Invalid Dates

Check:

```python
print(
    df[df["order_date"].isna()]
)
```

Remove invalid dates:

```python
df = df.dropna(
    subset=["order_date"]
)
```

***

## 153. Step 17 — Handle Missing Customer Names

Suppose customer name is mandatory.

Then:

```python
df = df.dropna(
    subset=["customer_name"]
)
```

***

## 154. Step 18 — Handle Missing City

We can either remove the record:

```python
df = df.dropna(
    subset=["city"]
)
```

or assign:

```python
df["city"] = df["city"].fillna(
    "Unknown"
)
```

The correct choice depends on the business requirement.

***

## 155. Step 19 — Reset Index

After all the filtering:

```python
df = df.reset_index(drop=True)
```

***

## 156. Step 20 — Add Derived Columns

Calculate a tax:

```python
df["tax"] = df["amount"] * 0.18
```

Calculate total:

```python
df["total_amount"] = (
    df["amount"] + df["tax"]
)
```

***

## 157. Step 21 — Extract Date Components

```python
df["order_year"] = df["order_date"].dt.year

df["order_month"] = df["order_date"].dt.month

df["order_day"] = df["order_date"].dt.day
```

***

## 158. Step 22 — Validate the Final Data

Check:

```python
print(df.info())
```

Check missing values:

```python
print(df.isna().sum())
```

Check duplicates:

```python
print(
    df["order_id"].duplicated().sum()
)
```

Check negative values:

```python
print(
    (df["amount"] < 0).sum()
)
```

***

## 159. Step 23 — Save Clean Data

```python
df.to_csv(
    "clean_orders.csv",
    index=False
)
```

Now we have:

```
raw_orders.csv
      ↓
Pandas Cleaning
      ↓
clean_orders.csv
```

***

## 160. Complete Mini Project

Here is the complete project in one program.

```python
import pandas as pd

# -----------------------------------
# 1. Generate Raw Data
# -----------------------------------

raw_data = {
    "Order ID": [
        "ORD-1001",
        " ORD-1002 ",
        "ORD-1003",
        "ORD-1004",
        "ORD-1004",
        "ORD-1005",
        "ORD-1006",
        "ORD-1007",
        None,
        "ORD-1009"
    ],

    "Customer Name": [
        "Karthi Kumar",
        "  Ravi Kumar  ",
        "ARUN",
        None,
        "Priya",
        " priya ",
        "Suresh",
        "Kumar",
        "Anitha",
        "  Rahul  "
    ],

    "Email": [
        "karthi@gmail.com",
        "ravi@gmail.com",
        "arun@yahoo.com",
        "priya@gmail.com",
        "priya@gmail.com",
        None,
        "suresh@gmail.com",
        "invalid-email",
        "anitha@gmail.com",
        "rahul@gmail.com"
    ],

    "City": [
        "Chennai",
        " chennai ",
        "BANGALORE",
        "Mumbai",
        "Mumbai",
        " chennai",
        "Bangalore",
        "mumbai",
        None,
        "CHENNAI"
    ],

    "Amount": [
        "500",
        "700",
        "1200",
        "1500",
        "1500",
        "INVALID",
        "900",
        "-200",
        "1000",
        "2500"
    ],

    "Order Date": [
        "2026-09-01",
        "2026-09-02",
        "2026-09-03",
        "2026-09-04",
        "2026-09-04",
        "2026-09-05",
        "2026-09-06",
        "2026-09-07",
        "INVALID DATE",
        "2026-09-09"
    ]
}

raw_df = pd.DataFrame(raw_data)

raw_df.to_csv(
    "raw_orders.csv",
    index=False
)

print("Raw CSV created.")


# -----------------------------------
# 2. Read Raw CSV
# -----------------------------------

df = pd.read_csv("raw_orders.csv")

print("\nRaw Data:")
print(df)


# -----------------------------------
# 3. Inspect
# -----------------------------------

print("\nShape:")
print(df.shape)

print("\nData Types:")
print(df.dtypes)

print("\nMissing Values:")
print(df.isna().sum())


# -----------------------------------
# 4. Clean Column Names
# -----------------------------------

df.columns = (
    df.columns
      .str.strip()
      .str.lower()
      .str.replace(" ", "_")
)


# -----------------------------------
# 5. Remove Duplicate Order IDs
# -----------------------------------

df["order_id"] = df["order_id"].str.strip()

df = df.drop_duplicates(
    subset=["order_id"],
    keep="first"
)


# -----------------------------------
# 6. Clean Customer Name
# -----------------------------------

df["customer_name"] = (
    df["customer_name"]
    .str.strip()
    .str.title()
)


# -----------------------------------
# 7. Clean Email
# -----------------------------------

df["email"] = (
    df["email"]
    .str.strip()
    .str.lower()
)

email_pattern = r"^[\w.-]+@[\w.-]+\.\w+$"

valid_email = df["email"].str.match(
    email_pattern,
    na=False
)

df.loc[~valid_email, "email"] = pd.NA


# -----------------------------------
# 8. Clean City
# -----------------------------------

df["city"] = (
    df["city"]
    .str.strip()
    .str.title()
)

df["city"] = df["city"].fillna("Unknown")


# -----------------------------------
# 9. Convert Amount
# -----------------------------------

df["amount"] = pd.to_numeric(
    df["amount"],
    errors="coerce"
)

# Remove invalid amounts
df = df.dropna(
    subset=["amount"]
)

# Remove negative amounts
df = df[df["amount"] >= 0]


# -----------------------------------
# 10. Convert Date
# -----------------------------------

df["order_date"] = pd.to_datetime(
    df["order_date"],
    errors="coerce"
)

df = df.dropna(
    subset=["order_date"]
)


# -----------------------------------
# 11. Remove Records Missing
#     Mandatory Customer Name
# -----------------------------------

df = df.dropna(
    subset=["customer_name"]
)


# -----------------------------------
# 12. Derived Columns
# -----------------------------------

df["tax"] = df["amount"] * 0.18

df["total_amount"] = (
    df["amount"] + df["tax"]
)


# -----------------------------------
# 13. Date Features
# -----------------------------------

df["order_year"] = (
    df["order_date"].dt.year
)

df["order_month"] = (
    df["order_date"].dt.month
)


# -----------------------------------
# 14. Reset Index
# -----------------------------------

df = df.reset_index(drop=True)


# -----------------------------------
# 15. Validation
# -----------------------------------

print("\nFinal Shape:")
print(df.shape)

print("\nMissing Values:")
print(df.isna().sum())

print("\nDuplicate Order IDs:")
print(
    df["order_id"].duplicated().sum()
)

print("\nNegative Amounts:")
print(
    (df["amount"] < 0).sum()
)


# -----------------------------------
# 16. Save Clean Data
# -----------------------------------

df.to_csv(
    "clean_orders.csv",
    index=False
)

print("\nClean CSV created successfully.")

print("\nFinal Data:")
print(df)
```

***

## 161. Project Architecture

The project we built follows:

```
Python
  ↓
Generate Dirty CSV
  ↓
raw_orders.csv
  ↓
Pandas read_csv()
  ↓
Data Inspection
  ↓
Column Cleaning
  ↓
Duplicate Handling
  ↓
String Cleaning
  ↓
Email Validation
  ↓
Numeric Conversion
  ↓
Date Conversion
  ↓
Missing Value Handling
  ↓
Business Rule Validation
  ↓
Derived Columns
  ↓
Final Validation
  ↓
clean_orders.csv
```

This is a very simple version of a real ETL process.

***

## 162. Expected Project Files

After running the project:

```
project_folder/
│
├── raw_orders.csv
│
├── clean_orders.csv
│
└── pandas_project.py
```

In Jupyter Notebook, the CSV files will normally appear in the notebook's current working directory.

***

## 163. Check Current Working Directory

In Jupyter:

```python
import os

print(os.getcwd())
```

This tells you where Python is currently working.

List files:

```python
print(os.listdir())
```

***

## 164. A More Professional Cleaning Pattern

For reusable pipelines, separate the cleaning logic into functions.

```python
def clean_columns(df):

    df = df.copy()

    df.columns = (
        df.columns
          .str.strip()
          .str.lower()
          .str.replace(" ", "_")
    )

    return df
```

Then:

```python
df = clean_columns(df)
```

***

## 165. Separate Function for Customer Cleaning

```python
def clean_customer_data(df):

    df = df.copy()

    df["customer_name"] = (
        df["customer_name"]
        .str.strip()
        .str.title()
    )

    df["email"] = (
        df["email"]
        .str.strip()
        .str.lower()
    )

    return df
```

***

## 166. Separate Function for Numeric Cleaning

```python
def clean_amount(df):

    df = df.copy()

    df["amount"] = pd.to_numeric(
        df["amount"],
        errors="coerce"
    )

    df = df.dropna(
        subset=["amount"]
    )

    df = df[df["amount"] >= 0]

    return df
```

***

## 167. Combining Cleaning Functions

```python
df = clean_columns(df)

df = clean_customer_data(df)

df = clean_amount(df)
```

This approach becomes more maintainable as a project grows.

***

## 168. Advanced ETL-Style Structure

A more realistic architecture:

```python
def extract(file_path):
    return pd.read_csv(file_path)


def transform(df):

    df = df.copy()

    # Cleaning
    # Validation
    # Transformation

    return df


def load(df, file_path):
    df.to_csv(
        file_path,
        index=False
    )


df = extract("raw_orders.csv")

df = transform(df)

load(df, "clean_orders.csv")
```

This directly reflects:

```
Extract
Transform
Load
```

***

## 169. Pandas Learning Roadmap

The recommended progression is:

```
Level 1 — Basics
│
├── What is Pandas
├── Series
├── DataFrame
├── Index
├── Columns
├── Creating DataFrames
├── head()
├── tail()
├── info()
├── describe()
└── shape / size / dtypes

Level 2 — Selection
│
├── Column selection
├── Multiple columns
├── loc
├── iloc
├── Boolean filtering
├── isin()
├── query()
└── between()

Level 3 — Data Manipulation
│
├── Add columns
├── Update columns
├── Rename
├── Drop
├── Sort
├── Reset index
├── Unique
├── nunique
└── value_counts()

Level 4 — Data Cleaning
│
├── isna()
├── notna()
├── dropna()
├── fillna()
├── ffill()
├── bfill()
├── duplicated()
├── drop_duplicates()
├── astype()
├── to_numeric()
└── to_datetime()

Level 5 — String & Regex
│
├── .str
├── strip()
├── lower()
├── upper()
├── replace()
├── contains()
├── startswith()
├── endswith()
└── Regex extraction

Level 6 — Transformation
│
├── map()
├── replace()
├── apply()
├── assign()
├── where()
├── mask()
├── np.where()
└── np.select()

Level 7 — Aggregation
│
├── sum()
├── mean()
├── median()
├── count()
├── min()
├── max()
├── groupby()
└── agg()

Level 8 — Combining Data
│
├── merge()
├── concat()
├── join()
└── SQL-style joins

Level 9 — Reshaping
│
├── pivot()
├── pivot_table()
├── melt()
├── stack()
├── unstack()
└── explode()

Level 10 — Dates
│
├── to_datetime()
├── .dt
├── date filtering
├── date arithmetic
├── resample()
└── time-series operations

Level 11 — Files
│
├── read_csv()
├── to_csv()
├── read_excel()
├── to_excel()
├── read_json()
├── to_json()
├── read_parquet()
└── to_parquet()

Level 12 — Large Data
│
├── chunksize
├── usecols
├── dtype
├── memory_usage()
├── category
└── efficient transformations

Level 13 — Advanced
│
├── MultiIndex
├── Window operations
├── rolling()
├── ranking
├── json_normalize()
├── method chaining
├── performance optimization
└── ETL architecture
```

***

## 170. Most Important Pandas Commands Cheat Sheet

### Import

```python
import pandas as pd
```

### Create

```python
pd.Series()
pd.DataFrame()
```

### Inspect

```python
df.head()
df.tail()
df.info()
df.describe()
df.shape
df.dtypes
df.columns
df.index
```

### Select

```python
df["column"]
df[["col1", "col2"]]
df.loc[]
df.iloc[]
```

### Filter

```python
df[df["age"] > 30]
df[df["city"].isin(["Chennai", "Mumbai"])]
df.query("age > 30")
```

### Missing Values

```python
df.isna()
df.notna()
df.dropna()
df.fillna()
df.ffill()
df.bfill()
```

### Duplicates

```python
df.duplicated()
df.drop_duplicates()
```

### Cleaning

```python
df.astype()
pd.to_numeric()
pd.to_datetime()
```

### Strings

```python
df["name"].str.strip()
df["name"].str.lower()
df["name"].str.upper()
df["name"].str.title()
df["name"].str.replace()
df["name"].str.contains()
df["name"].str.extract()
```

### Transform

```python
df["new"] = ...
df.apply()
df.map()
df.assign()
```

### Sort

```python
df.sort_values()
df.sort_index()
```

### Aggregation

```python
df.sum()
df.mean()
df.median()
df.min()
df.max()
df.count()
```

### Grouping

```python
df.groupby()
df.groupby().agg()
```

### Combining

```python
pd.merge()
pd.concat()
df.join()
```

### Reshaping

```python
df.pivot()
pd.pivot_table()
df.melt()
df.explode()
```

### File Handling

```python
pd.read_csv()
df.to_csv()

pd.read_excel()
df.to_excel()

pd.read_json()
df.to_json()

pd.read_parquet()
df.to_parquet()
```

***

## 171. Pandas vs Python Data Structures

Before Pandas:

```python
customers = [
    {"name": "Karthi", "age": 30},
    {"name": "Ravi", "age": 28}
]
```

We can process this using normal Python loops.

With Pandas:

```python
df = pd.DataFrame(customers)
```

Then:

```python
df[df["age"] > 29]
```

Pandas provides a much more convenient structure for tabular data.

***

## 172. Key Data Engineering Mindset

Do not think of Pandas as just:

```
"Python for Excel"
```

Think of it as:

```
Structured Data Processing Layer
```

For example:

```
CSV
 │
 ├── Read
 │
 ├── Profile
 │
 ├── Clean
 │
 ├── Validate
 │
 ├── Transform
 │
 ├── Join
 │
 ├── Aggregate
 │
 └── Export
```

***

## 173. Final Key Takeaways

Pandas provides two major structures:

```
Series
DataFrame
```

The DataFrame is the main structure you will use for tabular Data Engineering work.

The most important beginner commands are:

```python
pd.read_csv()
df.head()
df.tail()
df.info()
df.describe()
df.shape
df.dtypes
```

For selection:

```python
df["column"]
df[["column1", "column2"]]
df.loc[]
df.iloc[]
```

For filtering:

```python
df[df["column"] > value]
df.query()
df.isin()
```

For cleaning:

```python
isna()
dropna()
fillna()
drop_duplicates()
astype()
pd.to_numeric()
pd.to_datetime()
```

For transformation:

```python
assign()
apply()
map()
str
```

For aggregation:

```python
groupby()
agg()
```

For combining:

```python
merge()
concat()
```

For files:

```python
read_csv()
to_csv()
read_json()
to_json()
read_parquet()
to_parquet()
```

For large datasets:

```python
chunksize
usecols
dtype
memory_usage()
```

And the most important Data Engineering pattern is:

```
            RAW DATA
               ↓
          Read / Extract
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
       Join / Aggregate
               ↓
          Final Dataset
               ↓
         CSV / Parquet
               ↓
         Downstream System
```

The mini-project above gives you the first practical experience of this complete flow:

```
Python
  ↓
Generate dirty CSV
  ↓
Pandas
  ↓
Clean
  ↓
Validate
  ↓
Transform
  ↓
Generate clean CSV
```

This project can be extended later into a larger Pandas ETL project using multiple CSV files, joins, date analysis, aggregations, data-quality reports, and finally Parquet output.
