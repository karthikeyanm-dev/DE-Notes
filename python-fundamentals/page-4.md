# Page

## List Comprehension

> Imported from [`python/List Comprehension.ipynb`](https://github.com/karthikeyanm-dev/DE-Notes/blob/main/python/List%20Comprehension.ipynb). Code outputs are omitted; run the source notebook for live results.

## List Comprehension

### Introduction

List comprehension is a concise way of creating a new list from an existing iterable.

Instead of writing multiple lines using a `for` loop, we can often create the same list in a single line.

List comprehensions are frequently used in Data Engineering for:

* Data transformation
* Filtering records
* Cleaning values
* Extracting columns
* Creating derived values
* Processing collections

## 1. Basic List Comprehension

### Definition

A basic list comprehension creates a new list by applying an expression to every element of an iterable.

### Syntax

```python
[expression for item in iterable]
```

The basic flow is:

```
Iterable
   ↓
Take one element
   ↓
Apply expression
   ↓
Store result in new list
```

### Real-World Analogy

Imagine you have a list of customer names:

```
Arun
Rahul
Priya
Vijay
```

You want to create a new list containing the length of every customer's name.

Instead of manually processing each customer, we can say:

```
For every customer:
    Find the length of the name
    Add it to the new list
```

List comprehension provides a compact way to express this operation.

## 2. Basic Example

Using a normal `for` loop:

```python
numbers = [1, 2, 3, 4, 5]

squares = []

for number in numbers:
    squares.append(number ** 2)

print(squares)
```

Output:

```
[1, 4, 9, 16, 25]
```

The same operation using list comprehension:

```python
numbers = [1, 2, 3, 4, 5]

squares = [number ** 2 for number in numbers]

print(squares)
```

Output:

```
[1, 4, 9, 16, 25]
```

### Breakdown

```python
[number ** 2 for number in numbers]
```

Can be understood as:

```
number ** 2
      ↑
   Expression

for number in numbers
      ↑
   Iteration
```

## 3. Another Basic Example

Convert a list of numbers into their doubled values.

```python
numbers = [10, 20, 30, 40]

result = [number * 2 for number in numbers]

print(result)
```

Output:

```
[20, 40, 60, 80]
```

## 4. String Example

Convert all names to uppercase.

```python
names = ["arun", "rahul", "priya"]

upper_names = [name.upper() for name in names]

print(upper_names)
```

Output:

```
['ARUN', 'RAHUL', 'PRIYA']
```

This is a simple example of data transformation.

## 5. Data Engineering Example

Suppose we have file names and want to extract the file extensions.

```python
files = [
    "customers.csv",
    "orders.json",
    "products.parquet"
]

extensions = [file.split(".")[-1] for file in files]

print(extensions)
```

Output:

```
['csv', 'json', 'parquet']
```

This demonstrates how list comprehension can be used for simple data extraction.

## 6. Conditional List Comprehension

### Definition

A conditional list comprehension allows us to filter elements while creating a new list.

### Syntax

```python
[expression for item in iterable if condition]
```

The condition determines which elements are included.

### Real-World Analogy

Imagine a dataset containing:

```
10
25
40
15
60
```

You only want values greater than `20`.

Instead of:

```
Check each value
    ↓
If value > 20
    ↓
Add it to the new list
```

we can use conditional list comprehension.

## 7. Filtering Even Numbers

Using a normal loop:

```python
numbers = [1, 2, 3, 4, 5, 6]

even_numbers = []

for number in numbers:

    if number % 2 == 0:
        even_numbers.append(number)

print(even_numbers)
```

Output:

```
[2, 4, 6]
```

Using list comprehension:

```python
numbers = [1, 2, 3, 4, 5, 6]

even_numbers = [
    number
    for number in numbers
    if number % 2 == 0
]

print(even_numbers)
```

Output:

```
[2, 4, 6]
```

## 8. Filtering Odd Numbers

```python
numbers = [1, 2, 3, 4, 5, 6]

odd_numbers = [
    number
    for number in numbers
    if number % 2 != 0
]

print(odd_numbers)
```

Output:

```
[1, 3, 5]
```

## 9. Filtering Values Greater Than a Number

```python
numbers = [10, 25, 5, 40, 15, 60]

result = [
    number
    for number in numbers
    if number > 20
]

print(result)
```

Output:

```
[25, 40, 60]
```

## 10. Data Engineering Example - Filtering Records

Suppose we have transaction amounts.

```python
transactions = [
    500,
    1200,
    250,
    5000,
    750,
    3000
]
```

We want transactions greater than `1000`.

```python
large_transactions = [
    amount
    for amount in transactions
    if amount > 1000
]

print(large_transactions)
```

Output:

```
[1200, 5000, 3000]
```

This is similar to filtering records based on a business rule.

## 11. Conditional Transformation

List comprehension can both transform and filter values.

### Example

```python
numbers = [1, 2, 3, 4, 5, 6]

squares_of_even = [
    number ** 2
    for number in numbers
    if number % 2 == 0
]

print(squares_of_even)
```

Output:

```
[4, 16, 36]
```

The process is:

```
1 → Odd → Skip
2 → Even → 2² → 4
3 → Odd → Skip
4 → Even → 4² → 16
5 → Odd → Skip
6 → Even → 6² → 36
```

## 12. if-else Inside List Comprehension

List comprehension also supports an `if-else` expression.

### Syntax

```python
[expression_if_true if condition else expression_if_false for item in iterable]
```

Notice the difference:

#### Filtering

```python
[expression for item in iterable if condition]
```

#### if-else

```python
[expression_if_true if condition else expression_if_false for item in iterable]
```

### Example

```python
numbers = [1, 2, 3, 4, 5]

result = [
    "Even" if number % 2 == 0 else "Odd"
    for number in numbers
]

print(result)
```

Output:

```
['Odd', 'Even', 'Odd', 'Even', 'Odd']
```

## 13. Data Engineering Example - Record Status

Suppose we have transaction amounts.

```python
amounts = [500, 0, 1200, 0, 3000]
```

We want to classify each amount.

```python
status = [
    "Valid" if amount > 0 else "Invalid"
    for amount in amounts
]

print(status)
```

Output:

```
['Valid', 'Invalid', 'Valid', 'Invalid', 'Valid']
```

This demonstrates a simple data quality classification.

## 14. Nested List Comprehension

### Definition

A nested list comprehension is a list comprehension containing another `for` loop.

It is useful when working with nested collections such as:

```
List of Lists
Matrix
Nested records
Multiple groups of values
```

### Basic Syntax

```python
[expression for outer_item in outer_iterable for inner_item in inner_iterable]
```

The order is equivalent to nested loops.

## 15. Normal Nested Loop

Consider a matrix:

```python
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]
```

Using nested loops:

```python
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

result = []

for row in matrix:

    for value in row:
        result.append(value)

print(result)
```

Output:

```
[1, 2, 3, 4, 5, 6, 7, 8, 9]
```

The same operation using nested list comprehension:

```python
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

result = [
    value
    for row in matrix
    for value in row
]

print(result)
```

Output:

```
[1, 2, 3, 4, 5, 6, 7, 8, 9]
```

## 16. Understanding Nested List Comprehension

This:

```python
[
    value
    for row in matrix
    for value in row
]
```

is equivalent to:

```python
for row in matrix:
    for value in row:
        result.append(value)
```

The outer loop comes first:

```python
for row in matrix
```

The inner loop comes second:

```python
for value in row
```

The expression comes first in the comprehension:

```python
value
```

## 17. Nested Loop for Pairs

Using normal loops:

```python
numbers = [1, 2, 3]

pairs = []

for x in numbers:

    for y in numbers:
        pairs.append((x, y))

print(pairs)
```

Output:

```
[(1, 1), (1, 2), (1, 3),
 (2, 1), (2, 2), (2, 3),
 (3, 1), (3, 2), (3, 3)]
```

Using nested list comprehension:

```python
numbers = [1, 2, 3]

pairs = [
    (x, y)
    for x in numbers
    for y in numbers
]

print(pairs)
```

Output:

```
[(1, 1), (1, 2), (1, 3),
 (2, 1), (2, 2), (2, 3),
 (3, 1), (3, 2), (3, 3)]
```

## 18. Nested List Comprehension with Condition

We can also add a condition.

```python
numbers = [1, 2, 3, 4]

pairs = [
    (x, y)
    for x in numbers
    for y in numbers
    if x < y
]

print(pairs)
```

Output:

```
[(1, 2), (1, 3), (1, 4),
 (2, 3), (2, 4),
 (3, 4)]
```

## 19. Data Engineering Example - Multiple Files

Suppose files are organized by date.

```python
files = [
    ["customers.csv", "orders.csv"],
    ["customers.csv", "orders.csv"],
    ["customers.csv", "orders.csv"]
]
```

We can flatten the structure:

```python
all_files = [
    file
    for daily_files in files
    for file in daily_files
]

print(all_files)
```

Output:

```
['customers.csv', 'orders.csv',
 'customers.csv', 'orders.csv',
 'customers.csv', 'orders.csv']
```

This is a simple example of flattening nested data.

## 20. Nested List Comprehension - Matrix Transformation

Suppose we want to multiply every value in a matrix by `2`.

```python
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

result = [
    [value * 2 for value in row]
    for row in matrix
]

print(result)
```

Output:

```
[
    [2, 4, 6],
    [8, 10, 12],
    [14, 16, 18]
]
```

Here there are two list comprehensions:

```python
[value * 2 for value in row]
```

creates each transformed row.

The outer comprehension:

```python
[
    [value * 2 for value in row]
    for row in matrix
]
```

creates the complete matrix.

## 21. Data Engineering Example - Clean Multiple Records

Suppose raw customer names contain unwanted spaces.

```python
customers = [
    ["  Arun  ", " Chennai "],
    ["  Rahul ", " Mumbai "],
    ["Priya  ", " Delhi  "]
]
```

We can clean all values:

```python
cleaned_customers = [
    [value.strip() for value in customer]
    for customer in customers
]

print(cleaned_customers)
```

Output:

```
[
    ['Arun', 'Chennai'],
    ['Rahul', 'Mumbai'],
    ['Priya', 'Delhi']
]
```

This demonstrates how nested list comprehension can be used for simple data cleaning.

## 22. List Comprehension with Strings

We can process characters using list comprehension.

```python
text = "Python"

characters = [
    character.upper()
    for character in text
]

print(characters)
```

Output:

```
['P', 'Y', 'T', 'H', 'O', 'N']
```

## 23. Extracting Valid File Names

Suppose we receive different types of files.

```python
files = [
    "customers.csv",
    "orders.json",
    "products.csv",
    "sales.txt",
    "payments.csv"
]
```

We only want CSV files.

```python
csv_files = [
    file
    for file in files
    if file.endswith(".csv")
]

print(csv_files)
```

Output:

```
['customers.csv', 'products.csv', 'payments.csv']
```

This is a very practical Data Engineering use case.

## 24. Transforming File Names

Convert file names to lowercase.

```python
files = [
    "CUSTOMERS.CSV",
    "ORDERS.CSV",
    "PRODUCTS.CSV"
]

normalized_files = [
    file.lower()
    for file in files
]

print(normalized_files)
```

Output:

```
['customers.csv', 'orders.csv', 'products.csv']
```

## 25. Combining Filtering and Transformation

Suppose we want only CSV files and want to remove the extension.

```python
files = [
    "customers.csv",
    "orders.json",
    "products.csv",
    "payments.csv"
]

file_names = [
    file.replace(".csv", "")
    for file in files
    if file.endswith(".csv")
]

print(file_names)
```

Output:

```
['customers', 'products', 'payments']
```

This follows:

```
Filter → Transform
```

## 26. List Comprehension with Dictionary Data

Suppose we have customer records:

```python
customers = [
    {"id": 1001, "name": "Arun", "city": "Chennai"},
    {"id": 1002, "name": "Rahul", "city": "Mumbai"},
    {"id": 1003, "name": "Priya", "city": "Delhi"}
]
```

Extract customer IDs:

```python
customer_ids = [
    customer["id"]
    for customer in customers
]

print(customer_ids)
```

Output:

```
[1001, 1002, 1003]
```

Extract customer names:

```python
customer_names = [
    customer["name"]
    for customer in customers
]

print(customer_names)
```

Output:

```
['Arun', 'Rahul', 'Priya']
```

## 27. Filtering Dictionary Records

Suppose we want customers from Chennai.

```python
customers = [
    {"id": 1001, "name": "Arun", "city": "Chennai"},
    {"id": 1002, "name": "Rahul", "city": "Mumbai"},
    {"id": 1003, "name": "Priya", "city": "Chennai"}
]

chennai_customers = [
    customer
    for customer in customers
    if customer["city"] == "Chennai"
]

print(chennai_customers)
```

Output:

```
[
    {'id': 1001, 'name': 'Arun', 'city': 'Chennai'},
    {'id': 1003, 'name': 'Priya', 'city': 'Chennai'}
]
```

## 28. Extracting Data After Filtering

We can filter records and extract only the required field.

```python
customers = [
    {"id": 1001, "name": "Arun", "city": "Chennai"},
    {"id": 1002, "name": "Rahul", "city": "Mumbai"},
    {"id": 1003, "name": "Priya", "city": "Chennai"}
]

chennai_names = [
    customer["name"]
    for customer in customers
    if customer["city"] == "Chennai"
]

print(chennai_names)
```

Output:

```
['Arun', 'Priya']
```

This pattern is very useful:

```
Filter records
      ↓
Extract required field
      ↓
Create new list
```

## 29. Nested List Comprehension with Data

Suppose we have departments and employees.

```python
departments = {
    "IT": ["Arun", "Rahul"],
    "HR": ["Priya", "Divya"],
    "Finance": ["Vijay", "Suresh"]
}
```

We can create a list of all employees:

```python
employees = [
    employee
    for department_employees in departments.values()
    for employee in department_employees
]

print(employees)
```

Output:

```
['Arun', 'Rahul', 'Priya', 'Divya', 'Vijay', 'Suresh']
```

This is a practical example of flattening nested collections.

## 30. When NOT to Use List Comprehension

List comprehension is powerful, but it should not always be used.

Avoid extremely complicated comprehensions.

Difficult to read:

```python
result = [
    x * 2
    for x in numbers
    if x > 10
    if x % 2 == 0
    if x != 20
]
```

A normal loop may be clearer when the logic becomes complex:

```python
result = []

for x in numbers:

    if x > 10:

        if x % 2 == 0:

            if x != 20:
                result.append(x * 2)
```

The goal is not to write the shortest code.

The goal is to write code that is clear and maintainable.

## 31. List Comprehension vs for Loop

### Normal Loop

```python
numbers = [1, 2, 3, 4, 5]

squares = []

for number in numbers:
    squares.append(number ** 2)
```

### List Comprehension

```python
numbers = [1, 2, 3, 4, 5]

squares = [
    number ** 2
    for number in numbers
]
```

Both produce:

```
[1, 4, 9, 16, 25]
```

List comprehension is more concise.

## 32. General Patterns

### Basic

```python
[expression for item in iterable]
```

### Conditional Filtering

```python
[expression for item in iterable if condition]
```

### if-else

```python
[value_if_true if condition else value_if_false for item in iterable]
```

### Nested

```python
[expression for outer_item in outer_iterable for inner_item in inner_iterable]
```

### Nested with Condition

```python
[
    expression
    for outer_item in outer_iterable
    for inner_item in inner_iterable
    if condition
]
```

## 33. Common Mistake

Do not confuse the position of the `if` in these two patterns.

### Filtering

```python
[
    number
    for number in numbers
    if number > 10
]
```

The `if` comes at the end.

### if-else Expression

```python
[
    "Large" if number > 10 else "Small"
    for number in numbers
]
```

The `if-else` expression comes before the `for`.

## 34. Complete Data Engineering Example

Suppose a pipeline receives customer records.

```python
customers = [
    {
        "id": 1001,
        "name": "  Arun ",
        "city": "Chennai",
        "amount": 25000
    },
    {
        "id": 1002,
        "name": " Rahul ",
        "city": "Mumbai",
        "amount": 15000
    },
    {
        "id": 1003,
        "name": " Priya ",
        "city": "Chennai",
        "amount": 35000
    },
    {
        "id": 1004,
        "name": " Vijay ",
        "city": "Delhi",
        "amount": 8000
    }
]
```

### Step 1 - Extract Names

```python
names = [
    customer["name"].strip()
    for customer in customers
]

print(names)
```

Output:

```
['Arun', 'Rahul', 'Priya', 'Vijay']
```

### Step 2 - Find High-Value Customers

```python
high_value_customers = [
    customer
    for customer in customers
    if customer["amount"] >= 20000
]

print(high_value_customers)
```

### Step 3 - Extract High-Value Customer Names

```python
high_value_names = [
    customer["name"].strip()
    for customer in customers
    if customer["amount"] >= 20000
]

print(high_value_names)
```

Output:

```
['Arun', 'Priya']
```

### Step 4 - Create Customer Status

```python
customer_status = [
    "High Value" if customer["amount"] >= 20000 else "Regular"
    for customer in customers
]

print(customer_status)
```

Output:

```
['High Value', 'Regular', 'High Value', 'Regular']
```

This demonstrates how list comprehension can be used for:

```
Extraction
Cleaning
Filtering
Transformation
Classification
```

## 35. Practice Problems

### Problem 1 - Square Even Numbers

Given:

```python
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

Using list comprehension, create a list containing the squares of only the even numbers.

Expected output:

```
[4, 16, 36, 64, 100]
```

Requirement:

Use a single list comprehension.

## Problem 2 - Clean Customer Names

Given:

```python
names = [
    "  Arun  ",
    " Rahul",
    "Priya ",
    "  Vijay "
]
```

Using list comprehension:

1. Remove leading and trailing spaces.
2. Convert every name to uppercase.

Expected output:

```
['ARUN', 'RAHUL', 'PRIYA', 'VIJAY']
```

Requirement:

Use:

```python
strip()
```

and:

```python
upper()
```

## Problem 3 - Filter CSV Files

Given:

```python
files = [
    "customers.csv",
    "orders.json",
    "products.csv",
    "sales.txt",
    "payments.csv",
    "employees.parquet"
]
```

Using list comprehension, create a new list containing only CSV files.

Expected output:

```
[
    'customers.csv',
    'products.csv',
    'payments.csv'
]
```

Then create another list containing only the file names without `.csv`.

Expected:

```
[
    'customers',
    'products',
    'payments'
]
```

## Problem 4 - Customer Amounts

Given:

```python
customers = [
    {"name": "Arun", "amount": 25000},
    {"name": "Rahul", "amount": 15000},
    {"name": "Priya", "amount": 35000},
    {"name": "Vijay", "amount": 8000}
]
```

Using list comprehension:

1. Extract all customer names.
2. Extract customers with amount greater than `20000`.
3. Extract the names of customers with amount greater than `20000`.

Expected:

```
Names:
['Arun', 'Rahul', 'Priya', 'Vijay']

High Value Customers:
Arun
Priya

High Value Names:
['Arun', 'Priya']
```

## Problem 5 - Flatten Nested List

Given:

```python
data = [
    [10, 20, 30],
    [40, 50],
    [60, 70, 80]
]
```

Using nested list comprehension, flatten the list.

Expected output:

```
[10, 20, 30, 40, 50, 60, 70, 80]
```

Requirement:

Use nested list comprehension.

## Problem 6 - Matrix Transformation

Given:

```python
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]
```

Using nested list comprehension, create a new matrix where every value is multiplied by `10`.

Expected output:

```
[
    [10, 20, 30],
    [40, 50, 60],
    [70, 80, 90]
]
```

## Problem 7 - Data Quality Classification

Given:

```python
amounts = [500, 0, 1500, -100, 3000, 0]
```

Using list comprehension, classify each value as:

```
"Valid"
```

when the amount is greater than `0`.

Otherwise:

```
"Invalid"
```

Expected output:

```
['Valid', 'Invalid', 'Valid', 'Invalid', 'Valid', 'Invalid']
```

## Problem 8 - Nested Department Data

Given:

```python
departments = {
    "IT": ["Arun", "Rahul"],
    "HR": ["Priya", "Divya"],
    "Finance": ["Vijay", "Suresh"]
}
```

Using nested list comprehension:

1. Create a list containing all employees.
2. Create a list containing only employee names starting with `"P"` or `"A"`.

Expected:

```
All Employees:
['Arun', 'Rahul', 'Priya', 'Divya', 'Vijay', 'Suresh']

Selected Employees:
['Arun', 'Priya']
```

## 36. Key Takeaways

1. List comprehension provides a concise way to create lists.
2. The basic syntax is:

```python
[expression for item in iterable]
```

3. Conditional filtering uses:

```python
[expression for item in iterable if condition]
```

4. `if-else` expressions can also be used inside list comprehensions.
5. Nested list comprehensions can process nested collections.
6. List comprehensions can be used for:
   * Transformation
   * Filtering
   * Extraction
   * Cleaning
   * Classification
7. They are especially useful for processing lists of records and nested collections.
8. A list comprehension should remain readable.
9. For complex business logic, a normal `for` loop may be better.
10. In Data Engineering, list comprehensions are useful for small and medium-sized in-memory transformations.
11. For very large datasets, avoid assuming that list comprehensions are suitable for the entire dataset; tools such as Pandas and Spark provide specialized approaches for large-scale data processing.
