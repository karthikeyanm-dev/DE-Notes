# Page

## Data Structure

> Imported from [`python/Data Structure.ipynb`](https://github.com/karthikeyanm-dev/DE-Notes/blob/main/python/Data%20Structure.ipynb). Code outputs are omitted; run the source notebook for live results.

## Data Structures in Python

### Introduction

Data structures are ways of organizing and storing data so that we can efficiently access, modify, and process it.

In Python, the four commonly used built-in data structures are:

```
List
Tuple
Set
Dictionary
```

Each data structure has a different purpose and behavior.

### Real-World Analogy

Think about a Data Engineering project where we are processing customer information.

We may need:

* A collection of customer records in a specific order
* A fixed collection of values
* A collection containing only unique values
* A structure where each value is associated with a key

Python provides different data structures for these requirements.

***

## 1. List

### Definition

A list is an ordered and mutable collection of elements.

Example:

```python
customers = ["Arun", "Rahul", "Priya", "Karthik"]

print(customers)
```

Output:

```
['Arun', 'Rahul', 'Priya', 'Karthik']
```

### Characteristics

```
Ordered       → Yes
Mutable       → Yes
Duplicates    → Allowed
Indexing      → Supported
Slicing       → Supported
```

### Real-World Example

A list can represent a collection of files that a data pipeline needs to process.

```python
files = [
    "customers.csv",
    "orders.csv",
    "products.csv"
]
```

***

## 2. Tuple

### Definition

A tuple is an ordered and immutable collection of elements.

Example:

```python
coordinates = (13.0827, 80.2707)

print(coordinates)
```

Output:

```
(13.0827, 80.2707)
```

### Characteristics

```
Ordered       → Yes
Mutable       → No
Duplicates    → Allowed
Indexing      → Supported
Slicing       → Supported
```

### Real-World Example

A tuple can represent fixed configuration values.

```python
database_config = ("localhost", 3306, "sales_db")

print(database_config)
```

The values are expected to remain fixed.

***

## 3. Set

### Definition

A set is an unordered collection of unique elements.

Example:

```python
cities = {"Chennai", "Mumbai", "Delhi", "Chennai"}

print(cities)
```

The duplicate `"Chennai"` is automatically removed.

Possible output:

```
{'Chennai', 'Mumbai', 'Delhi'}
```

### Characteristics

```
Ordered       → No
Mutable       → Yes
Duplicates    → Not allowed
Indexing      → Not supported
```

### Real-World Example

Suppose we want to find all unique cities in a dataset.

```python
cities = {
    "Chennai",
    "Mumbai",
    "Chennai",
    "Delhi",
    "Mumbai"
}

print(cities)
```

The set keeps only unique values.

***

## 4. Dictionary

### Definition

A dictionary stores data as key-value pairs.

Example:

```python
customer = {
    "id": 1001,
    "name": "Arun",
    "city": "Chennai"
}

print(customer)
```

Output:

```
{'id': 1001, 'name': 'Arun', 'city': 'Chennai'}
```

### Characteristics

```
Key-Value pairs → Yes
Mutable         → Yes
Duplicate keys  → Not allowed
Access by key   → Yes
```

### Real-World Example

A dictionary can represent a single customer record.

```python
customer = {
    "customer_id": 1001,
    "customer_name": "Arun",
    "city": "Chennai",
    "order_count": 15
}
```

This structure is very common when working with JSON and API responses.

***

## 5. Overview of Python Data Structures

| Feature      | List       | Tuple            | Set           | Dictionary     |
| ------------ | ---------- | ---------------- | ------------- | -------------- |
| Ordered      | Yes        | Yes              | No            | Yes            |
| Mutable      | Yes        | No               | Yes           | Yes            |
| Duplicates   | Yes        | Yes              | No            | Keys: No       |
| Indexing     | Yes        | Yes              | No            | By key         |
| Slicing      | Yes        | Yes              | No            | No             |
| Main Purpose | Collection | Fixed collection | Unique values | Key-value data |

***

## 6. Choosing the Right Data Structure

Use a:

#### List

When:

```
You need an ordered collection
You need to modify the collection
Duplicates are allowed
```

Example:

```python
files = ["a.csv", "b.csv", "c.csv"]
```

#### Tuple

When:

```
You need an ordered collection
The collection should not be modified
```

Example:

```python
coordinates = (13.08, 80.27)
```

#### Set

When:

```
You need unique values
Fast membership checking is important
Order is not the main requirement
```

Example:

```python
unique_cities = {"Chennai", "Mumbai", "Delhi"}
```

#### Dictionary

When:

```
You need key-value relationships
You want to access values using meaningful keys
```

Example:

```python
customer = {
    "id": 1001,
    "name": "Arun"
}
```

***

## Part 2 - List and Tuple

## 7. List

### Definition

A list is an ordered, mutable collection that can contain multiple values.

### Creating a List

```python
numbers = [10, 20, 30, 40]

print(numbers)
```

Output:

```
[10, 20, 30, 40]
```

A list can contain different data types.

```python
data = [
    1001,
    "Arun",
    25000.50,
    True
]

print(data)
```

Output:

```
[1001, 'Arun', 25000.5, True]
```

***

## 8. List CRUD Operations

CRUD stands for:

```
C → Create
R → Read
U → Update
D → Delete
```

These operations are fundamental when working with mutable data structures.

***

## 9. Create - List

```python
customers = ["Arun", "Rahul", "Priya"]

print(customers)
```

We can also create an empty list:

```python
customers = []

print(customers)
```

Output:

```
[]
```

***

## 10. Read - List

### Access Using Index

```python
customers = ["Arun", "Rahul", "Priya"]

print(customers[0])
print(customers[1])
print(customers[2])
```

Output:

```
Arun
Rahul
Priya
```

### Negative Indexing

```python
customers = ["Arun", "Rahul", "Priya"]

print(customers[-1])
```

Output:

```
Priya
```

### Slicing

```python
customers = ["Arun", "Rahul", "Priya", "Vijay"]

print(customers[1:3])
```

Output:

```
['Rahul', 'Priya']
```

***

## 11. Update - List

Because lists are mutable, we can change individual elements.

```python
customers = ["Arun", "Rahul", "Priya"]

customers[1] = "Karthik"

print(customers)
```

Output:

```
['Arun', 'Karthik', 'Priya']
```

### Updating Multiple Elements

```python
numbers = [10, 20, 30, 40]

numbers[1:3] = [200, 300]

print(numbers)
```

Output:

```
[10, 200, 300, 40]
```

## 12. Delete - List

### Using del

```python
customers = ["Arun", "Rahul", "Priya"]

del customers[1]

print(customers)
```

Output:

```
['Arun', 'Priya']
```

### Using remove()

```python
customers = ["Arun", "Rahul", "Priya"]

customers.remove("Rahul")

print(customers)
```

Output:

```
['Arun', 'Priya']
```

### Using pop()

```python
customers = ["Arun", "Rahul", "Priya"]

removed_customer = customers.pop()

print(removed_customer)
print(customers)
```

Output:

```
Priya
['Arun', 'Rahul']
```

***

## 13. List Methods

Python provides many built-in methods for manipulating lists.

The major list methods are:

```
append()
extend()
insert()
remove()
pop()
clear()
index()
count()
sort()
reverse()
copy()
```

***

## 14. append()

### Definition

`append()` adds one element to the end of the list.

```python
numbers = [10, 20, 30]

numbers.append(40)

print(numbers)
```

Output:

```
[10, 20, 30, 40]
```

### Data Engineering Example

```python
files = ["customers.csv", "orders.csv"]

files.append("products.csv")

print(files)
```

Output:

```
['customers.csv', 'orders.csv', 'products.csv']
```

***

## 15. extend()

### Definition

`extend()` adds multiple elements to the end of a list.

```python
numbers = [10, 20]

numbers.extend([30, 40, 50])

print(numbers)
```

Output:

```
[10, 20, 30, 40, 50]
```

### append() vs extend()

Using `append()`:

```python
numbers = [10, 20]

numbers.append([30, 40])

print(numbers)
```

Output:

```
[10, 20, [30, 40]]
```

Using `extend()`:

```python
numbers = [10, 20]

numbers.extend([30, 40])

print(numbers)
```

Output:

```
[10, 20, 30, 40]
```

***

## 16. insert()

### Definition

`insert()` adds an element at a specific position.

### Syntax

```python
list.insert(index, value)
```

Example:

```python
cities = ["Chennai", "Mumbai", "Delhi"]

cities.insert(1, "Bangalore")

print(cities)
```

Output:

```
['Chennai', 'Bangalore', 'Mumbai', 'Delhi']
```

***

## 17. remove()

### Definition

`remove()` removes the first occurrence of a specified value.

```python
numbers = [10, 20, 30, 20]

numbers.remove(20)

print(numbers)
```

Output:

```
[10, 30, 20]
```

Only the first `20` is removed.

***

## 18. pop()

### Definition

`pop()` removes and returns an element from the list.

Without an index, it removes the last element.

```python
numbers = [10, 20, 30]

value = numbers.pop()

print(value)
print(numbers)
```

Output:

```
30
[10, 20]
```

### pop() with Index

```python
numbers = [10, 20, 30]

value = numbers.pop(1)

print(value)
print(numbers)
```

Output:

```
20
[10, 30]
```

***

## 19. clear()

### Definition

`clear()` removes all elements from the list.

```python
numbers = [10, 20, 30]

numbers.clear()

print(numbers)
```

Output:

```
[]
```

***

## 20. index()

### Definition

`index()` returns the position of the first occurrence of a value.

```python
cities = ["Chennai", "Mumbai", "Delhi"]

print(cities.index("Mumbai"))
```

Output:

```
1
```

***

## 21. count()

### Definition

`count()` returns the number of times a value occurs in a list.

```python
numbers = [10, 20, 10, 30, 10]

print(numbers.count(10))
```

Output:

```
3
```

### Data Engineering Example

```python
status = [
    "SUCCESS",
    "FAILED",
    "SUCCESS",
    "SUCCESS",
    "FAILED"
]

print(status.count("FAILED"))
```

Output:

```
2
```

This can be useful for basic status analysis.

## 22. sort()

### Definition

`sort()` sorts the elements of the list in ascending order by default.

```python
numbers = [50, 10, 40, 20, 30]

numbers.sort()

print(numbers)
```

Output:

```
[10, 20, 30, 40, 50]
```

### Descending Order

```python
numbers = [50, 10, 40, 20, 30]

numbers.sort(reverse=True)

print(numbers)
```

Output:

```
[50, 40, 30, 20, 10]
```

***

## 23. reverse()

### Definition

`reverse()` reverses the order of elements in the list.

```python
numbers = [10, 20, 30, 40]

numbers.reverse()

print(numbers)
```

Output:

```
[40, 30, 20, 10]
```

Important:

`reverse()` does not sort the list in descending order.

It simply reverses the existing order.

## 24. copy()

### Definition

`copy()` creates a shallow copy of a list.

```python
numbers = [10, 20, 30]

new_numbers = numbers.copy()

print(new_numbers)
```

Output:

```
[10, 20, 30]
```

The two lists are separate list objects.

```python
numbers = [10, 20, 30]

new_numbers = numbers.copy()

new_numbers.append(40)

print(numbers)
print(new_numbers)
```

Output:

```
[10, 20, 30]
[10, 20, 30, 40]
```

## 25. Useful Built-in Functions with Lists

These are functions, not list methods.

### len()

```python
numbers = [10, 20, 30, 40]

print(len(numbers))
```

Output:

```
4
```

### min()

```python
numbers = [10, 20, 30, 40]

print(min(numbers))
```

Output:

```
10
```

### max()

```python
numbers = [10, 20, 30, 40]

print(max(numbers))
```

Output:

```
40
```

### sum()

```python
numbers = [10, 20, 30, 40]

print(sum(numbers))
```

Output:

```
100
```

***

## 26. Tuple

### Definition

A tuple is an ordered and immutable collection.

### Creating a Tuple

```python
numbers = (10, 20, 30, 40)

print(numbers)
```

Output:

```
(10, 20, 30, 40)
```

### Tuple with Different Data Types

```python
customer = (
    1001,
    "Arun",
    "Chennai",
    25000.50
)

print(customer)
```

***

## 27. Tuple CRUD Concept

Tuples are immutable, so CRUD works differently.

#### Create

```python
customer = (1001, "Arun", "Chennai")
```

#### Read

```python
print(customer[0])
print(customer[1])
print(customer[2])
```

#### Update

Direct update is **not allowed**.

```python
customer = (1001, "Arun", "Chennai")

customer[1] = "Rahul"
```

This produces:

```
TypeError: 'tuple' object does not support item assignment
```

#### Delete

Individual elements cannot be deleted.

However, the entire tuple variable can be deleted:

```python
customer = (1001, "Arun", "Chennai")

del customer
```

***

## 28. Tuple Methods

Tuples have only two built-in methods:

```
count()
index()
```

### count()

```python
numbers = (10, 20, 10, 30, 10)

print(numbers.count(10))
```

Output:

```
3
```

### index()

```python
cities = ("Chennai", "Mumbai", "Delhi")

print(cities.index("Mumbai"))
```

Output:

```
1
```

***

## 29. Tuple Packing

### Definition

Tuple packing means placing multiple values into a tuple without explicitly using parentheses.

```python
customer = 1001, "Arun", "Chennai"

print(customer)
print(type(customer))
```

Output:

```
(1001, 'Arun', 'Chennai')
<class 'tuple'>
```

Python automatically creates a tuple.

***

## 30. Tuple Unpacking

### Definition

Tuple unpacking means assigning the elements of a tuple to multiple variables.

```python
customer = (1001, "Arun", "Chennai")

customer_id, name, city = customer

print(customer_id)
print(name)
print(city)
```

Output:

```
1001
Arun
Chennai
```

The number of variables must normally match the number of values.

***

## 31. Extended Unpacking

Python allows `*` to collect multiple values.

```python
numbers = (10, 20, 30, 40, 50)

first, *middle, last = numbers

print(first)
print(middle)
print(last)
```

Output:

```
10
[20, 30, 40]
50
```

Notice that `middle` becomes a list.

## 32. Swapping Variables Using Tuple Unpacking

Python allows variables to be swapped without a temporary variable.

```python
a = 10
b = 20

a, b = b, a

print(a)
print(b)
```

Output:

```
20
10
```

This works because Python uses tuple packing and unpacking.

## 33. Nested Collections

### Definition

A collection inside another collection is called a nested collection.

Examples include:

```
List inside List
Tuple inside List
Dictionary inside List
List inside Dictionary
```

Nested collections are extremely common when representing structured data.

### Nested List

```python
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

print(matrix)
```

### Accessing Nested List

```python
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

print(matrix[0][0])
print(matrix[1][2])
```

Output:

```
1
6
```

***

## 34. Nested List with Data Engineering Example

Suppose we have customer records represented as lists.

```python
customers = [
    [1001, "Arun", "Chennai"],
    [1002, "Rahul", "Mumbai"],
    [1003, "Priya", "Delhi"]
]

print(customers[0])
print(customers[0][1])
```

Output:

```
[1001, 'Arun', 'Chennai']
Arun
```

The structure can be visualized as:

```
customers
    |
    ├── customer 1
    │     ├── ID
    │     ├── Name
    │     └── City
    |
    ├── customer 2
    │     ├── ID
    │     ├── Name
    │     └── City
    |
    └── customer 3
          ├── ID
          ├── Name
          └── City
```

## 35. Nested List with Traversal

```python
customers = [
    [1001, "Arun", "Chennai"],
    [1002, "Rahul", "Mumbai"],
    [1003, "Priya", "Delhi"]
]

for customer in customers:

    for value in customer:
        print(value)

    print()
```

Output:

```
1001
Arun
Chennai

1002
Rahul
Mumbai

1003
Priya
Delhi
```

## 36. List of Tuples

A list can contain tuples.

```python
customers = [
    (1001, "Arun"),
    (1002, "Rahul"),
    (1003, "Priya")
]

print(customers)
```

This structure is common when working with database query results.

Example:

```python
records = [
    (1, "Arun", 5000),
    (2, "Rahul", 7000),
    (3, "Priya", 6000)
]

for customer_id, name, amount in records:
    print(
        f"ID: {customer_id}, "
        f"Name: {name}, "
        f"Amount: {amount}"
    )
```

Output:

```
ID: 1, Name: Arun, Amount: 5000
ID: 2, Name: Rahul, Amount: 7000
ID: 3, Name: Priya, Amount: 6000
```

## 37. List of Dictionaries

A very common Data Engineering structure is a list of dictionaries.

```python
customers = [
    {
        "id": 1001,
        "name": "Arun",
        "city": "Chennai"
    },
    {
        "id": 1002,
        "name": "Rahul",
        "city": "Mumbai"
    },
    {
        "id": 1003,
        "name": "Priya",
        "city": "Delhi"
    }
]
```

Accessing data:

```python
print(customers[0]["name"])
```

Output:

```
Arun
```

Traversing:

```python
for customer in customers:

    print(
        f"ID: {customer['id']}, "
        f"Name: {customer['name']}, "
        f"City: {customer['city']}"
    )
```

Output:

```
ID: 1001, Name: Arun, City: Chennai
ID: 1002, Name: Rahul, City: Mumbai
ID: 1003, Name: Priya, City: Delhi
```

This structure is commonly encountered when working with JSON and API data.

## 38. List Methods Summary

| Method      | Purpose                             |
| ----------- | ----------------------------------- |
| `append()`  | Adds one element at the end         |
| `extend()`  | Adds multiple elements              |
| `insert()`  | Adds an element at a specific index |
| `remove()`  | Removes the first matching value    |
| `pop()`     | Removes and returns an element      |
| `clear()`   | Removes all elements                |
| `index()`   | Returns the index of a value        |
| `count()`   | Counts occurrences of a value       |
| `sort()`    | Sorts the list                      |
| `reverse()` | Reverses the list                   |
| `copy()`    | Creates a shallow copy              |

## 39. Tuple Methods Summary

| Method    | Purpose                       |
| --------- | ----------------------------- |
| `count()` | Counts occurrences of a value |
| `index()` | Returns the index of a value  |

Unlike lists, tuples do not have methods such as:

```
append()
remove()
insert()
pop()
sort()
reverse()
clear()
```

because tuples are immutable.

## 40. List Practice Problems

### Problem 1 - Find Largest and Smallest

Given:

```python
numbers = [45, 12, 78, 34, 89, 23, 56]
```

Write a Python program to find:

1. The largest number
2. The smallest number
3. The total of all numbers
4. The average of the numbers

Expected output:

```
Largest: 89
Smallest: 12
Total: 337
Average: 48.14
```

Try solving it first using Python built-in functions.

Then try solving it without using:

```python
max()
min()
sum()
```

## Problem 2 - Remove Duplicates

Given:

```python
numbers = [10, 20, 10, 30, 20, 40, 50, 30]
```

Create a new list containing only unique values.

Expected result:

```
[10, 20, 30, 40, 50]
```

Do not use:

```python
set()
```

## Problem 3 - Count Occurrences

Given:

```python
numbers = [10, 20, 10, 30, 20, 10, 40, 20]
```

Find how many times each number occurs.

Expected output:

```
10 → 3
20 → 3
30 → 1
40 → 1
```

Try solving this using list methods.

## Problem 4 - Separate Even and Odd Numbers

Given:

```python
numbers = [10, 15, 22, 33, 40, 51, 64, 75]
```

Create two lists:

```
Even numbers
Odd numbers
```

Expected output:

```
Even: [10, 22, 40, 64]
Odd: [15, 33, 51, 75]
```

## Problem 5 - Find Common Elements

Given:

```python
list1 = [10, 20, 30, 40, 50]
list2 = [30, 40, 50, 60, 70]
```

Find the elements that exist in both lists.

Expected output:

```
[30, 40, 50]
```

Try solving it without using:

```python
set()
```

## Problem 6 - Reverse Without reverse()

Given:

```python
numbers = [10, 20, 30, 40, 50]
```

Reverse the list without using:

```python
reverse()
```

or:

```python
[::-1]
```

Expected output:

```
[50, 40, 30, 20, 10]
```

## Problem 7 - Find Second Largest

Given:

```python
numbers = [10, 50, 30, 80, 20, 70]
```

Find the second-largest number.

Expected output:

```
Second Largest: 70
```

Try solving it without using:

```python
sort()
```

as the primary solution.

## Problem 8 - List of Customer Records

Given:

```python
customers = [
    [1001, "Arun", 25000],
    [1002, "Rahul", 35000],
    [1003, "Priya", 45000],
    [1004, "Vijay", 30000]
]
```

Write a program to:

1. Print every customer's name.
2. Calculate the total salary.
3. Find the customer with the highest salary.
4. Find the average salary.

Expected result:

```
Names:
Arun
Rahul
Priya
Vijay

Total Salary: 135000
Highest Salary: 45000
Average Salary: 33750.0
```

## Problem 9 - Nested List

Given:

```python
matrix = [
    [10, 20, 30],
    [40, 50, 60],
    [70, 80, 90]
]
```

Using nested loops:

1. Print every value.
2. Calculate the total of all values.
3. Find the largest value.

Expected output:

```
Total: 450
Largest: 90
```

## Problem 10 - List of Tuples

Given:

```python
sales = [
    ("Chennai", 10000),
    ("Mumbai", 15000),
    ("Delhi", 12000),
    ("Bangalore", 18000)
]
```

Using tuple unpacking:

1. Print each city and its sales.
2. Calculate total sales.
3. Find the city with the highest sales.

Expected output:

```
Chennai: 10000
Mumbai: 15000
Delhi: 12000
Bangalore: 18000

Total Sales: 55000
Highest Sales City: Bangalore
```

## Key Takeaways

1. Data structures organize and store data efficiently.
2. Python provides four major built-in data structures:
   * List
   * Tuple
   * Set
   * Dictionary
3. Lists are ordered and mutable.
4. Tuples are ordered and immutable.
5. Sets store unique values.
6. Dictionaries store key-value pairs.
7. Lists support CRUD operations.
8. Lists provide many useful methods such as `append()`, `extend()`, `insert()`, `remove()`, `pop()`, `sort()`, and `reverse()`.
9. Tuples have only two main methods:
   * `count()`
   * `index()`
10. Tuple packing combines multiple values into a tuple.
11. Tuple unpacking assigns tuple values to multiple variables.
12. Nested collections allow one data structure to contain another.
13. Lists of dictionaries are commonly used when working with JSON and API data.
14. Lists of tuples are commonly encountered when processing database query results.
15. Data structures are fundamental to Data Engineering because they provide the foundation for handling records, files, columns, API responses, and transformed data.

## Set and Dictionary in Python

## Part 1 - Set

### 1. Set - Introduction

#### Definition

A set is an unordered collection of unique elements in Python.

A set automatically removes duplicate values.

#### Real-World Analogy

Imagine a Data Engineering pipeline processing customer city data:

```
Chennai
Mumbai
Chennai
Delhi
Mumbai
Chennai
```

If we only want the unique cities, we can use a set:

```python
cities = {"Chennai", "Mumbai", "Chennai", "Delhi", "Mumbai"}

print(cities)
```

Possible output:

```
{'Chennai', 'Mumbai', 'Delhi'}
```

The duplicate values are automatically removed.

#### Characteristics

```
Ordered       → No
Mutable       → Yes
Duplicates    → Not allowed
Indexing      → No
Slicing       → No
Iterable      → Yes
```

Important:

Although sets are mutable, the individual elements inside a set must be hashable.

## 2. Creating a Set

```python
numbers = {10, 20, 30, 40}

print(numbers)
```

Output:

```
{10, 20, 30, 40}
```

### Empty Set

Be careful when creating an empty set.

This creates an empty dictionary:

```python
data = {}

print(type(data))
```

Output:

```
<class 'dict'>
```

To create an empty set, use:

```python
data = set()

print(type(data))
```

Output:

```
<class 'set'>
```

## 3. Set with Duplicate Values

```python
numbers = {10, 20, 10, 30, 20, 40}

print(numbers)
```

Output:

```
{10, 20, 30, 40}
```

Duplicates are automatically removed.

## 4. Set CRUD Operations

CRUD stands for:

```
C → Create
R → Read
U → Update
D → Delete
```

Sets support these concepts differently from lists because sets do not support indexing.

***

## 5. Create - Set

```python
cities = {"Chennai", "Mumbai", "Delhi"}

print(cities)
```

An empty set:

```python
cities = set()

print(cities)
```

Output:

```
set()
```

## 6. Read - Set

Sets do not support indexing.

This is invalid:

```python
cities = {"Chennai", "Mumbai", "Delhi"}

print(cities[0])
```

This produces an error because sets are not subscriptable.

Instead, use a loop:

```python
cities = {"Chennai", "Mumbai", "Delhi"}

for city in cities:
    print(city)
```

The order of output should not be relied upon.

### Membership Checking

Sets are very useful for membership testing.

```python
cities = {"Chennai", "Mumbai", "Delhi"}

print("Chennai" in cities)
print("Bangalore" in cities)
```

Output:

```
True
False
```

## 7. Update - Set

Sets are mutable, so we can add and remove elements.

### add()

`add()` adds one element to the set.

```python
cities = {"Chennai", "Mumbai"}

cities.add("Delhi")

print(cities)
```

Possible output:

```
{'Chennai', 'Mumbai', 'Delhi'}
```

### update()

`update()` adds multiple elements.

```python
cities = {"Chennai", "Mumbai"}

cities.update(["Delhi", "Bangalore"])

print(cities)
```

Possible output:

```
{'Chennai', 'Mumbai', 'Delhi', 'Bangalore'}
```

### Difference Between add() and update()

`add()` adds one element:

```python
cities.add("Delhi")
```

`update()` adds multiple elements:

```python
cities.update(["Delhi", "Bangalore"])
```

## 8. Delete - Set

### remove()

`remove()` removes a specified value.

```python
cities = {"Chennai", "Mumbai", "Delhi"}

cities.remove("Mumbai")

print(cities)
```

Possible output:

```
{'Chennai', 'Delhi'}
```

If the value does not exist, `remove()` raises a `KeyError`.

### discard()

`discard()` also removes a value.

However, if the value does not exist, it does not raise an error.

```python
cities = {"Chennai", "Mumbai", "Delhi"}

cities.discard("Bangalore")

print(cities)
```

No error occurs.

### pop()

`pop()` removes and returns an arbitrary element from the set.

```python
cities = {"Chennai", "Mumbai", "Delhi"}

value = cities.pop()

print("Removed:", value)
print("Remaining:", cities)
```

Because sets are unordered, we should not assume which element will be removed.

### clear()

`clear()` removes all elements.

```python
cities = {"Chennai", "Mumbai", "Delhi"}

cities.clear()

print(cities)
```

Output:

```
set()
```

## 9. Set Methods

The important set methods are:

```
add()
update()
remove()
discard()
pop()
clear()
copy()
```

Set operations are also extremely important:

```
union()
intersection()
difference()
symmetric_difference()
```

We will cover both.

## 10. add()

```python
numbers = {10, 20, 30}

numbers.add(40)

print(numbers)
```

Possible output:

```
{10, 20, 30, 40}
```

## 11. update()

```python
numbers = {10, 20}

numbers.update([30, 40, 50])

print(numbers)
```

Possible output:

```
{10, 20, 30, 40, 50}
```

It can also accept another set:

```python
numbers = {10, 20}

numbers.update({30, 40})

print(numbers)
```

## 12. remove()

```python
numbers = {10, 20, 30}

numbers.remove(20)

print(numbers)
```

Possible output:

```
{10, 30}
```

## 13. discard()

```python
numbers = {10, 20, 30}

numbers.discard(50)

print(numbers)
```

No error occurs even though `50` is not present.

## 14. pop()

```python
numbers = {10, 20, 30}

removed_value = numbers.pop()

print("Removed:", removed_value)
print("Remaining:", numbers)
```

The removed value can vary because sets are unordered.

## 15. clear()

```python
numbers = {10, 20, 30}

numbers.clear()

print(numbers)
```

Output:

```
set()
```

## 16. copy()

`copy()` creates a shallow copy of the set.

```python
numbers = {10, 20, 30}

new_numbers = numbers.copy()

new_numbers.add(40)

print(numbers)
print(new_numbers)
```

Possible output:

```
{10, 20, 30}
{10, 20, 30, 40}
```

## 17. Set Union

### Definition

Union combines all unique elements from two sets.

### Syntax

```python
set1.union(set2)
```

Example:

```python
set1 = {1, 2, 3}
set2 = {3, 4, 5}

result = set1.union(set2)

print(result)
```

Output:

```
{1, 2, 3, 4, 5}
```

The `|` operator can also be used:

```python
result = set1 | set2

print(result)
```

## 18. Set Intersection

### Definition

Intersection returns elements that exist in both sets.

```python
set1 = {1, 2, 3, 4}
set2 = {3, 4, 5, 6}

result = set1.intersection(set2)

print(result)
```

Output:

```
{3, 4}
```

Using `&`:

```python
result = set1 & set2

print(result)
```

Output:

```
{3, 4}
```

## 19. Set Difference

### Definition

Difference returns elements that exist in the first set but not in the second set.

```python
set1 = {1, 2, 3, 4}
set2 = {3, 4, 5, 6}

result = set1.difference(set2)

print(result)
```

Output:

```
{1, 2}
```

Using `-`:

```python
result = set1 - set2

print(result)
```

Output:

```
{1, 2}
```

## 20. Symmetric Difference

### Definition

Symmetric difference returns elements that exist in either set, but not in both.

```python
set1 = {1, 2, 3, 4}
set2 = {3, 4, 5, 6}

result = set1.symmetric_difference(set2)

print(result)
```

Output:

```
{1, 2, 5, 6}
```

Using `^`:

```python
result = set1 ^ set2

print(result)
```

## 21. Set Relationship Methods

Sets provide methods to check relationships between sets.

Important methods:

```
issubset()
issuperset()
isdisjoint()
```

### issubset()

Checks whether all elements of one set exist in another set.

```python
small = {1, 2}
large = {1, 2, 3, 4}

print(small.issubset(large))
```

Output:

```
True
```

### issuperset()

Checks whether a set contains all elements of another set.

```python
large = {1, 2, 3, 4}
small = {1, 2}

print(large.issuperset(small))
```

Output:

```
True
```

### isdisjoint()

Checks whether two sets have no common elements.

```python
set1 = {1, 2, 3}
set2 = {4, 5, 6}

print(set1.isdisjoint(set2))
```

Output:

```
True
```

## 22. Data Engineering Example - Unique Values

Suppose we receive customer city information.

```python
cities = [
    "Chennai",
    "Mumbai",
    "Chennai",
    "Delhi",
    "Mumbai",
    "Bangalore"
]

unique_cities = set(cities)

print(unique_cities)
```

Possible output:

```
{'Chennai', 'Mumbai', 'Delhi', 'Bangalore'}
```

This is a simple way to identify unique values.

In large-scale Data Engineering, similar operations are often performed using tools such as SQL, Spark, or Pandas rather than Python sets for the entire dataset.

## 23. Data Engineering Example - Comparing Data Sources

Suppose two systems provide customer IDs.

```python
source_a = {1001, 1002, 1003, 1004}
source_b = {1003, 1004, 1005, 1006}
```

Customers present in both systems:

```python
common_customers = source_a & source_b

print(common_customers)
```

Output:

```
{1003, 1004}
```

Customers present only in Source A:

```python
only_source_a = source_a - source_b

print(only_source_a)
```

Output:

```
{1001, 1002}
```

Customers present only in Source B:

```python
only_source_b = source_b - source_a

print(only_source_b)
```

Output:

```
{1005, 1006}
```

This resembles a simple data reconciliation operation.

## Part 2 - Dictionary

## 24. Dictionary - Introduction

### Definition

A dictionary is a mutable collection of key-value pairs.

Each key is used to access its corresponding value.

### Real-World Analogy

Think of a customer record in a database.

Instead of remembering:

```
1001
Arun
Chennai
25000
```

we can associate each value with a meaningful field name:

```
customer_id → 1001
name        → Arun
city        → Chennai
amount      → 25000
```

Python represents this using a dictionary.

```python
customer = {
    "customer_id": 1001,
    "name": "Arun",
    "city": "Chennai",
    "amount": 25000
}
```

This structure is very similar to a JSON object.

### Characteristics

```
Key-Value pairs → Yes
Mutable         → Yes
Ordered         → Yes
Duplicate keys  → No
Duplicate values → Allowed
Access          → Using keys
Indexing        → No
```

## 25. Creating a Dictionary

```python
customer = {
    "id": 1001,
    "name": "Arun",
    "city": "Chennai"
}

print(customer)
```

Output:

```
{'id': 1001, 'name': 'Arun', 'city': 'Chennai'}
```

### Empty Dictionary

```python
customer = {}

print(customer)
```

Output:

```
{}
```

Or:

```python
customer = dict()

print(customer)
```

## 26. Dictionary CRUD Operations

CRUD stands for:

```
C → Create
R → Read
U → Update
D → Delete
```

## 27. Create - Dictionary

```python
customer = {
    "id": 1001,
    "name": "Arun",
    "city": "Chennai"
}

print(customer)
```

We can also create an empty dictionary:

```python
customer = {}

customer["id"] = 1001
customer["name"] = "Arun"

print(customer)
```

Output:

```
{'id': 1001, 'name': 'Arun'}
```

## 28. Read - Dictionary

### Access Using Key

```python
customer = {
    "id": 1001,
    "name": "Arun",
    "city": "Chennai"
}

print(customer["id"])
print(customer["name"])
print(customer["city"])
```

Output:

```
1001
Arun
Chennai
```

### Using get()

`get()` is safer when the key may not exist.

```python
customer = {
    "id": 1001,
    "name": "Arun"
}

print(customer.get("name"))
print(customer.get("email"))
```

Output:

```
Arun
None
```

We can also provide a default value:

```python
print(customer.get("email", "Not Available"))
```

Output:

```
Not Available
```

## 29. Update - Dictionary

Dictionaries are mutable.

We can update an existing value.

```python
customer = {
    "id": 1001,
    "name": "Arun",
    "city": "Chennai"
}

customer["city"] = "Bangalore"

print(customer)
```

Output:

```
{'id': 1001, 'name': 'Arun', 'city': 'Bangalore'}
```

### Adding a New Key

If the key does not exist, assignment creates a new key.

```python
customer = {
    "id": 1001,
    "name": "Arun"
}

customer["city"] = "Chennai"

print(customer)
```

Output:

```
{'id': 1001, 'name': 'Arun', 'city': 'Chennai'}
```

## 30. update()

The `update()` method adds or modifies multiple key-value pairs.

```python
customer = {
    "id": 1001,
    "name": "Arun"
}

customer.update({
    "city": "Chennai",
    "age": 25
})

print(customer)
```

Output:

```
{'id': 1001, 'name': 'Arun', 'city': 'Chennai', 'age': 25}
```

## 31. Delete - Dictionary

### pop()

`pop()` removes a specified key and returns its value.

```python
customer = {
    "id": 1001,
    "name": "Arun",
    "city": "Chennai"
}

removed_value = customer.pop("city")

print(removed_value)
print(customer)
```

Output:

```
Chennai
{'id': 1001, 'name': 'Arun'}
```

### popitem()

`popitem()` removes and returns the last inserted key-value pair.

```python
customer = {
    "id": 1001,
    "name": "Arun",
    "city": "Chennai"
}

item = customer.popitem()

print(item)
print(customer)
```

Output:

```
('city', 'Chennai')
{'id': 1001, 'name': 'Arun'}
```

### del

```python
customer = {
    "id": 1001,
    "name": "Arun",
    "city": "Chennai"
}

del customer["city"]

print(customer)
```

Output:

```
{'id': 1001, 'name': 'Arun'}
```

### clear()

Removes all key-value pairs.

```python
customer = {
    "id": 1001,
    "name": "Arun"
}

customer.clear()

print(customer)
```

Output:

```
{}
```

## 32. Dictionary Methods

Important dictionary methods include:

```
get()
keys()
values()
items()
update()
pop()
popitem()
clear()
copy()
setdefault()
fromkeys()
```

***

## 33. keys()

`keys()` returns a view containing the dictionary keys.

```python
customer = {
    "id": 1001,
    "name": "Arun",
    "city": "Chennai"
}

print(customer.keys())
```

Output:

```
dict_keys(['id', 'name', 'city'])
```

We can traverse the keys:

```python
for key in customer.keys():
    print(key)
```

Output:

```
id
name
city
```

## 34. values()

`values()` returns a view containing the dictionary values.

```python
customer = {
    "id": 1001,
    "name": "Arun",
    "city": "Chennai"
}

print(customer.values())
```

Output:

```
dict_values([1001, 'Arun', 'Chennai'])
```

Traversing:

```python
for value in customer.values():
    print(value)
```

Output:

```
1001
Arun
Chennai
```

## 35. items()

`items()` returns key-value pairs.

```python
customer = {
    "id": 1001,
    "name": "Arun",
    "city": "Chennai"
}

print(customer.items())
```

Output:

```
dict_items([
    ('id', 1001),
    ('name', 'Arun'),
    ('city', 'Chennai')
])
```

The most common way to traverse a dictionary is:

```python
for key, value in customer.items():
    print(key, value)
```

Output:

```
id 1001
name Arun
city Chennai
```

## 36. copy()

Creates a shallow copy of a dictionary.

```python
customer = {
    "id": 1001,
    "name": "Arun"
}

new_customer = customer.copy()

new_customer["city"] = "Chennai"

print(customer)
print(new_customer)
```

Output:

```
{'id': 1001, 'name': 'Arun'}
{'id': 1001, 'name': 'Arun', 'city': 'Chennai'}
```

## 37. setdefault()

`setdefault()` returns the value of a key if it exists.

If the key does not exist, it creates the key with the specified default value.

```python
customer = {
    "id": 1001,
    "name": "Arun"
}

city = customer.setdefault("city", "Chennai")

print(city)
print(customer)
```

Output:

```
Chennai
{'id': 1001, 'name': 'Arun', 'city': 'Chennai'}
```

If the key already exists:

```python
customer = {
    "city": "Mumbai"
}

city = customer.setdefault("city", "Chennai")

print(city)
print(customer)
```

Output:

```
Mumbai
{'city': 'Mumbai'}
```

## 38. fromkeys()

`fromkeys()` creates a dictionary using a sequence of keys.

```python
columns = ["id", "name", "city"]

customer = dict.fromkeys(columns)

print(customer)
```

Output:

```
{'id': None, 'name': None, 'city': None}
```

We can provide a default value:

```python
columns = ["id", "name", "city"]

customer = dict.fromkeys(columns, "Unknown")

print(customer)
```

Output:

```
{'id': 'Unknown', 'name': 'Unknown', 'city': 'Unknown'}
```

## 39. Dictionary Membership

The `in` operator checks dictionary keys by default.

```python
customer = {
    "id": 1001,
    "name": "Arun",
    "city": "Chennai"
}

print("name" in customer)
print("email" in customer)
```

Output:

```
True
False
```

To check values:

```python
print("Arun" in customer.values())
```

Output:

```
True
```

## 40. Dictionary Traversal

### Traversing Keys

```python
customer = {
    "id": 1001,
    "name": "Arun",
    "city": "Chennai"
}

for key in customer:
    print(key)
```

Output:

```
id
name
city
```

### Traversing Values

```python
for value in customer.values():
    print(value)
```

### Traversing Key-Value Pairs

```python
for key, value in customer.items():
    print(f"{key}: {value}")
```

Output:

```
id: 1001
name: Arun
city: Chennai
```

## 41. Nested Dictionary

A dictionary can contain another dictionary.

```python
customer = {
    "id": 1001,
    "name": "Arun",
    "address": {
        "city": "Chennai",
        "state": "Tamil Nadu",
        "pincode": 600001
    }
}

print(customer["address"]["city"])
```

Output:

```
Chennai
```

This type of nested structure is common in JSON data.

## 42. List of Dictionaries

One of the most important structures in Data Engineering is a list of dictionaries.

```python
customers = [
    {
        "id": 1001,
        "name": "Arun",
        "city": "Chennai"
    },
    {
        "id": 1002,
        "name": "Rahul",
        "city": "Mumbai"
    },
    {
        "id": 1003,
        "name": "Priya",
        "city": "Delhi"
    }
]
```

This resembles multiple records from an API or JSON document.

We can traverse it:

```python
for customer in customers:

    print(
        f"ID: {customer['id']}, "
        f"Name: {customer['name']}, "
        f"City: {customer['city']}"
    )
```

Output:

```
ID: 1001, Name: Arun, City: Chennai
ID: 1002, Name: Rahul, City: Mumbai
ID: 1003, Name: Priya, City: Delhi
```

## 43. Dictionary with List

A dictionary can also contain lists.

```python
department = {
    "name": "Data Engineering",
    "employees": [
        "Arun",
        "Rahul",
        "Priya"
    ]
}

print(department["employees"])
```

Output:

```
['Arun', 'Rahul', 'Priya']
```

Access an individual employee:

```python
print(department["employees"][0])
```

Output:

```
Arun
```

## 44. Data Engineering Example - API Response

API responses commonly contain nested dictionaries and lists.

Example:

```python
response = {
    "status": "success",
    "data": [
        {
            "customer_id": 1001,
            "name": "Arun",
            "orders": [101, 102, 103]
        },
        {
            "customer_id": 1002,
            "name": "Rahul",
            "orders": [104, 105]
        }
    ]
}
```

We can access the data:

```python
for customer in response["data"]:

    print(f"Customer: {customer['name']}")
    print(f"Customer ID: {customer['customer_id']}")
    print(f"Orders: {customer['orders']}")
```

Output:

```
Customer: Arun
Customer ID: 1001
Orders: [101, 102, 103]
Customer: Rahul
Customer ID: 1002
Orders: [104, 105]
```

This structure is very similar to the kind of nested data a Data Engineer may receive from a REST API.

## 45. Set vs List

| Feature    | List               | Set           |
| ---------- | ------------------ | ------------- |
| Ordered    | Yes                | No            |
| Mutable    | Yes                | Yes           |
| Duplicates | Allowed            | Not allowed   |
| Indexing   | Yes                | No            |
| Slicing    | Yes                | No            |
| Membership | Supported          | Supported     |
| Main Use   | Ordered collection | Unique values |

Example:

```python
numbers = [10, 20, 10, 30]

print(numbers)
```

Output:

```
[10, 20, 10, 30]
```

Set:

```python
numbers = {10, 20, 10, 30}

print(numbers)
```

Possible output:

```
{10, 20, 30}
```

## 46. Dictionary vs List

| Feature          | List                 | Dictionary         |
| ---------------- | -------------------- | ------------------ |
| Structure        | Values               | Key-value pairs    |
| Access           | Index                | Key                |
| Ordered          | Yes                  | Yes                |
| Mutable          | Yes                  | Yes                |
| Duplicate values | Allowed              | Allowed            |
| Duplicate keys   | Not applicable       | Not allowed        |
| Main Use         | Collection of values | Structured records |

List:

```python
customer = [1001, "Arun", "Chennai"]
```

Dictionary:

```python
customer = {
    "id": 1001,
    "name": "Arun",
    "city": "Chennai"
}
```

The dictionary is generally easier to understand because every value has a meaningful key.

## 47. Dictionary Key Rules

Dictionary keys must be hashable.

Common valid key types include:

```
str
int
float
tuple
```

Example:

```python
data = {
    "name": "Arun",
    101: "Employee",
    (1, 2): "Coordinates"
}

print(data)
```

A list cannot be used as a dictionary key:

```python
data = {
    [1, 2]: "Invalid"
}
```

This raises:

```
TypeError: unhashable type: 'list'
```

Values can be almost any Python object.

```python
data = {
    "numbers": [10, 20, 30],
    "customer": {
        "id": 1001
    }
}

print(data)
```

## 48. Set and Dictionary - Data Engineering Example

Suppose we receive customer records from two different systems.

```python
source_a = [
    {"id": 1001, "name": "Arun"},
    {"id": 1002, "name": "Rahul"},
    {"id": 1003, "name": "Priya"}
]

source_b = [
    {"id": 1002, "name": "Rahul"},
    {"id": 1003, "name": "Priya"},
    {"id": 1004, "name": "Vijay"}
]
```

We can use sets to compare customer IDs.

```python
ids_a = {customer["id"] for customer in source_a}
ids_b = {customer["id"] for customer in source_b}

common_ids = ids_a & ids_b
new_ids = ids_b - ids_a

print("Common IDs:", common_ids)
print("New IDs:", new_ids)
```

Output:

```
Common IDs: {1002, 1003}
New IDs: {1004}
```

This demonstrates how:

```
List → stores records
Dictionary → represents each record
Set → compares unique IDs
```

This combination is very useful for understanding data processing workflows.

## 49. Complete Set Example

```python
# Create a set

cities = {
    "Chennai",
    "Mumbai",
    "Delhi"
}

# Add

cities.add("Bangalore")

# Update

cities.update(["Pune", "Hyderabad"])

print("Cities:", cities)

# Membership

print("Chennai" in cities)

# Remove

cities.discard("Pune")

print("After removal:", cities)

# Unique values

data = [
    "Chennai",
    "Mumbai",
    "Chennai",
    "Delhi",
    "Mumbai"
]

unique_cities = set(data)

print("Unique Cities:", unique_cities)

# Set operations

source_a = {1001, 1002, 1003}
source_b = {1002, 1003, 1004}

print("Common:", source_a & source_b)
print("Only A:", source_a - source_b)
print("Only B:", source_b - source_a)
print("All:", source_a | source_b)
```

Possible output:

```
Cities: {'Chennai', 'Mumbai', 'Delhi', 'Bangalore', 'Pune', 'Hyderabad'}
True
After removal: {'Chennai', 'Mumbai', 'Delhi', 'Bangalore', 'Hyderabad'}
Unique Cities: {'Chennai', 'Mumbai', 'Delhi'}
Common: {1002, 1003}
Only A: {1001}
Only B: {1004}
All: {1001, 1002, 1003, 1004}
```

## 50. Complete Dictionary Example

```python
# Create dictionary

customer = {
    "customer_id": 1001,
    "name": "Arun",
    "city": "Chennai",
    "order_count": 5
}

# Read

print("Customer ID:", customer["customer_id"])
print("Name:", customer["name"])

# Update

customer["city"] = "Bangalore"

# Add new field

customer["total_amount"] = 25000.50

# Traverse

print("\nCustomer Details:")

for key, value in customer.items():
    print(f"{key}: {value}")

# Check key

if "email" in customer:
    print("Email available")
else:
    print("Email not available")

# Get value safely

email = customer.get("email", "Not Available")

print("Email:", email)
```

Output:

```
Customer ID: 1001
Name: Arun

Customer Details:
customer_id: 1001
name: Arun
city: Bangalore
order_count: 5
total_amount: 25000.5

Email not available
Email: Not Available
```

## 51. Practice Problems - Set

### Problem 1 - Find Unique Values

Given:

```python
numbers = [
    10, 20, 10, 30, 40, 20,
    50, 30, 60, 40
]
```

Find all unique numbers using a set.

Expected output:

```
{10, 20, 30, 40, 50, 60}
```

Then find the number of unique values.

## Problem 2 - Find Common Students

Two batches contain student IDs:

```python
batch_1 = {101, 102, 103, 104, 105}
batch_2 = {104, 105, 106, 107, 108}
```

Find:

1. Students present in both batches.
2. Students only in Batch 1.
3. Students only in Batch 2.
4. Students present in either batch.

Expected:

```
Common: {104, 105}
Only Batch 1: {101, 102, 103}
Only Batch 2: {106, 107, 108}
All Students: {101, 102, 103, 104, 105, 106, 107, 108}
```

## Problem 3 - Find Unique Cities

Given:

```python
cities = [
    "Chennai",
    "Mumbai",
    "Chennai",
    "Delhi",
    "Bangalore",
    "Mumbai",
    "Delhi",
    "Pune"
]
```

Find:

1. Unique cities.
2. Number of unique cities.
3. Check whether `"Chennai"` exists.
4. Check whether `"Hyderabad"` exists.

Expected:

```
Unique Cities: ...
Number of Unique Cities: 5
Chennai: True
Hyderabad: False
```

## Problem 4 - Data Source Reconciliation

Two systems contain customer IDs:

```python
crm_customers = {
    1001, 1002, 1003, 1004, 1005
}

billing_customers = {
    1003, 1004, 1005, 1006, 1007
}
```

Find:

1. Customers available in both systems.
2. Customers missing from billing.
3. Customers present only in billing.
4. Total unique customers across both systems.

This represents a basic data reconciliation scenario.

## 52. Practice Problems - Dictionary

### Problem 5 - Customer Record

Create a dictionary containing:

```
customer_id
name
age
city
email
order_count
```

Then:

1. Print the customer name.
2. Update the city.
3. Add `total_amount`.
4. Remove `age`.
5. Print all key-value pairs.

## Problem 6 - Employee Salary

Given:

```python
employees = {
    "Arun": 50000,
    "Rahul": 60000,
    "Priya": 55000,
    "Vijay": 70000
}
```

Write a program to:

1. Print all employee names.
2. Print all salaries.
3. Calculate total salary.
4. Find the highest salary.
5. Find the employee with the highest salary.

## Problem 7 - Product Inventory

Given:

```python
products = {
    "Laptop": 10,
    "Mouse": 50,
    "Keyboard": 25,
    "Monitor": 15
}
```

Write a program to:

1. Print all products.
2. Print products with inventory greater than `20`.
3. Add a new product.
4. Update the laptop inventory.
5. Remove the monitor.

## Problem 8 - Data Engineering Records

Given:

```python
records = [
    {
        "id": 1001,
        "status": "SUCCESS",
        "amount": 5000
    },
    {
        "id": 1002,
        "status": "FAILED",
        "amount": 0
    },
    {
        "id": 1003,
        "status": "SUCCESS",
        "amount": 7500
    },
    {
        "id": 1004,
        "status": "SUCCESS",
        "amount": 3000
    }
]
```

Write a program to:

1. Print all successful records.
2. Count successful records.
3. Calculate total amount for successful records.
4. Find the record with the highest amount.

Expected:

```
Successful Records: 3
Total Successful Amount: 15500
Highest Amount: 7500
```

## 53. Key Takeaways - Set

1. A set is a mutable collection of unique elements.
2. Sets do not support indexing.
3. Sets automatically remove duplicates.
4. Use `set()` to create an empty set.
5. `add()` adds one element.
6. `update()` adds multiple elements.
7. `remove()` removes an element and raises an error if it is absent.
8. `discard()` removes an element without raising an error if it is absent.
9. `pop()` removes an arbitrary element.
10. `clear()` removes all elements.
11. `union()` combines unique elements.
12. `intersection()` finds common elements.
13. `difference()` finds elements present only in one set.
14. `symmetric_difference()` finds elements present in either set but not both.
15. Sets are useful for unique-value processing and membership checking.
16. Set operations are useful for comparing data sources and performing basic data reconciliation.

## 54. Key Takeaways - Dictionary

1. A dictionary stores data as key-value pairs.
2. Dictionaries are mutable.
3. Dictionary keys must be unique.
4. Dictionary values can be duplicated.
5. Dictionaries are accessed using keys rather than indexes.
6. `get()` safely retrieves a value.
7. `keys()` returns dictionary keys.
8. `values()` returns dictionary values.
9. `items()` returns key-value pairs.
10. `update()` adds or modifies multiple values.
11. `pop()` removes a specified key.
12. `popitem()` removes the last inserted key-value pair.
13. `clear()` removes all key-value pairs.
14. `copy()` creates a shallow copy.
15. `setdefault()` gets or creates a key with a default value.
16. `fromkeys()` creates a dictionary from a sequence of keys.
17. Dictionaries are commonly used to represent structured records.
18. Lists of dictionaries are commonly encountered when working with JSON and APIs.
19. Nested dictionaries are useful for representing hierarchical data.
20. Dictionaries are one of the most important Python data structures for Data Engineering because they closely resemble structured records and JSON objects.
