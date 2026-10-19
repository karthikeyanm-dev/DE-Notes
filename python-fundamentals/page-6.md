# Page

## Python

> Imported from [`python/Python.ipynb`](https://github.com/karthikeyanm-dev/DE-Notes/blob/main/python/Python.ipynb). Code outputs are omitted; run the source notebook for live results.

## Day 1

## Variables, Naming Rules, Data Types, Type Conversion, and type()

## 1. Variables

### Definition

A variable is a name used to store or reference a value in Python.

Python does not require us to explicitly declare the data type of a variable. The data type is automatically determined based on the value assigned to it.

### Real-World Analogy

Think of a variable like a labeled storage box in a data warehouse.

For example:

* `customer_name` → stores a customer's name
* `customer_id` → stores a customer's ID
* `order_amount` → stores an order amount
* `order_count` → stores the number of orders

In Python:

```python
customer_name = "Arun"
customer_id = 1001
order_amount = 2500.50
order_count = 5
```

The variable name acts like the label on the box, while the value is the data stored inside it.

### Creating Variables

```python
name = "Rahul"
age = 25
salary = 45000.50

print(name)
print(age)
print(salary)
```

Output:

```
Rahul
25
45000.5
```

### Python Automatically Determines the Data Type

```python
name = "Rahul"
age = 25
salary = 45000.50
```

Python automatically identifies:

* `name` as `str`
* `age` as `int`
* `salary` as `float`

We do not have to specify the data type while creating the variable.

### Assigning Multiple Variables

Python allows multiple variables to be assigned in a single statement.

```python
name, age, city = "Rahul", 25, "Chennai"

print(name)
print(age)
print(city)
```

Output:

```
Rahul
25
Chennai
```

### Assigning the Same Value to Multiple Variables

```python
x = y = z = 100

print(x)
print(y)
print(z)
```

Output:

```
100
100
100
```

### Changing a Variable Value

A variable can be assigned a new value at any time.

```python
age = 25

print(age)

age = 26

print(age)
```

Output:

```
25
26
```

### Data Engineering Example

Consider a simple customer record:

```python
customer_id = 1001
customer_name = "Arun"
customer_city = "Chennai"
order_count = 15
total_spent = 25000.75

print(customer_id)
print(customer_name)
print(customer_city)
print(order_count)
print(total_spent)
```

In a real Data Engineering pipeline, similar variables may temporarily hold values while extracting, transforming, validating, or loading data.

## 2. Naming Rules

### Definition

Naming rules are the rules that must be followed when creating variable names in Python.

Following proper naming conventions makes Data Engineering code easier to read, maintain, and debug.

### Rule 1: Variable names can contain letters

```python
name = "Arun"
customer = "Rahul"
```

### Rule 2: Variable names can contain numbers

Numbers are allowed in variable names, but the variable name cannot start with a number.

Valid:

```python
customer1 = "Arun"
data2026 = "Sales"
employee202 = "Karthik"
```

Invalid:

```python
1customer = "Arun"
2026data = "Sales"
```

### Rule 3: Underscores are allowed

```python
customer_name = "Arun"
total_amount = 5000
order_count = 25
```

Underscores are commonly used to separate words.

### Rule 4: Spaces are not allowed

Invalid:

```python
customer name = "Arun"
```

Correct:

```python
customer_name = "Arun"
```

### Rule 5: Python is case-sensitive

Python treats uppercase and lowercase letters as different.

```python
name = "Arun"
Name = "Rahul"
NAME = "Karthik"

print(name)
print(Name)
print(NAME)
```

Output:

```
Arun
Rahul
Karthik
```

Therefore:

```
name
Name
NAME
```

are three different variable names.

### Rule 6: Python Keywords Cannot Be Used

Python has reserved keywords that have special meanings.

Examples:

```
if
else
for
while
class
def
return
import
try
except
True
False
None
```

Invalid:

```python
class = "Data Engineer"
```

Correct:

```python
class_name = "Data Engineer"
```

### Checking Python Keywords

We can use the `keyword` module to view Python keywords.

```python
import keyword

print(keyword.kwlist)
```

### Recommended Naming Convention

Python commonly follows the `snake_case` naming convention for variables.

Good examples:

```python
customer_name = "Arun"
total_order_amount = 25000
number_of_orders = 15
customer_registration_date = "2026-08-17"
```

Avoid unclear variable names:

```python
x = "Arun"
a = 25000
n = 15
```

Prefer meaningful names:

```python
customer_name = "Arun"
total_amount = 25000
order_count = 15
```

### Data Engineering Example

Suppose we are processing sales data.

Instead of:

```python
x = 1001
y = "Chennai"
z = 50000
```

Use:

```python
customer_id = 1001
customer_city = "Chennai"
total_purchase_amount = 50000
```

Meaningful variable names become especially important when working with large datasets containing hundreds of columns.

## 3. Data Types

### Definition

A data type defines the kind of value stored in a variable.

Python has several built-in data types that are commonly used when working with data.

### Common Python Data Types

| Data Type  | Description                      | Example        |
| ---------- | -------------------------------- | -------------- |
| `int`      | Whole numbers                    | `100`          |
| `float`    | Decimal numbers                  | `99.50`        |
| `str`      | Text                             | `"Chennai"`    |
| `bool`     | True or False                    | `True`         |
| `list`     | Ordered and mutable collection   | `[10, 20, 30]` |
| `tuple`    | Ordered and immutable collection | `(10, 20, 30)` |
| `set`      | Collection of unique values      | `{10, 20, 30}` |
| `dict`     | Key-value pairs                  | `{"id": 101}`  |
| `NoneType` | Represents no value              | `None`         |

### 3.1 Integer - int

An integer is a whole number without a decimal value.

```python
age = 25
employee_count = 500
customer_id = 1001

print(age)
print(employee_count)
print(customer_id)
```

### Real-World Example

The number of records processed by a Data Engineering pipeline is normally an integer.

```python
records_processed = 100000

print(records_processed)
```

### 3.2 Float - float

A float represents a number containing a decimal value.

```python
salary = 45000.50
order_amount = 1599.75
temperature = 32.5

print(salary)
print(order_amount)
print(temperature)
```

### Real-World Example

Financial values such as order amounts, revenue, and transaction values may contain decimal values.

```python
transaction_amount = 1250.75

print(transaction_amount)
```

### 3.3 String - str

A string represents text.

Strings can be created using single quotes or double quotes.

```python
name = "Arun"
city = 'Chennai'

print(name)
print(city)
```

### Real-World Example

Customer names, cities, product names, and email addresses are generally stored as strings.

```python
customer_name = "Arun Kumar"
customer_city = "Chennai"
email = "arun@example.com"

print(customer_name)
print(customer_city)
print(email)
```

### 3.4 Boolean - bool

A Boolean represents one of two values:

```
True
False
```

Example:

```python
is_active = True
is_deleted = False

print(is_active)
print(is_deleted)
```

### Real-World Example

A data pipeline may need to determine whether a record is valid.

```python
is_valid_record = True

print(is_valid_record)
```

### 3.5 List - list

A list stores multiple values in an ordered and mutable collection.

```python
cities = ["Chennai", "Bangalore", "Mumbai"]

print(cities)
```

Individual values can be accessed using an index.

```python
cities = ["Chennai", "Bangalore", "Mumbai"]

print(cities[0])
print(cities[1])
print(cities[2])
```

Output:

```
Chennai
Bangalore
Mumbai
```

### Data Engineering Example

A list can contain multiple files that need to be processed.

```python
files = [
    "customers.csv",
    "orders.csv",
    "products.csv"
]

print(files)
```

### 3.6 Tuple - tuple

A tuple is an ordered collection that cannot be modified after creation.

```python
coordinates = (13.0827, 80.2707)

print(coordinates)
```

### 3.7 Set - set

A set stores unique values.

```python
cities = {"Chennai", "Mumbai", "Chennai", "Delhi"}

print(cities)
```

The duplicate `"Chennai"` will be removed.

### Data Engineering Example

Sets can be useful when finding unique values.

```python
departments = {"IT", "HR", "Finance", "IT", "HR"}

print(departments)
```

### 3.8 Dictionary - dict

A dictionary stores data in key-value pairs.

```python
customer = {
    "id": 1001,
    "name": "Arun",
    "city": "Chennai"
}

print(customer)
```

Individual values can be accessed using their keys.

```python
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

Dictionaries are commonly encountered when working with JSON data and APIs.

### 3.9 NoneType - None

`None` represents the absence of a value.

```python
middle_name = None

print(middle_name)
```

Real-world example:

```python
customer_phone = None
```

This could indicate that the phone number is currently unavailable.

## 4. Type Conversion

### Definition

Type conversion means converting a value from one data type to another.

Type conversion is very important in Data Engineering because data received from files, APIs, databases, and other systems may not always have the required data type.

### Real-World Analogy

Imagine receiving a customer age from a CSV file.

The value may arrive as:

```
"25"
```

Even though it looks like a number, it is actually text.

Before performing mathematical operations, we may need to convert it into an integer.

```python
age = "25"

age = int(age)

print(age)
```

Now `age` is an integer.

### Common Type Conversion Functions

| Function  | Converts To |
| --------- | ----------- |
| `int()`   | Integer     |
| `float()` | Float       |
| `str()`   | String      |
| `bool()`  | Boolean     |
| `list()`  | List        |
| `tuple()` | Tuple       |
| `set()`   | Set         |

### 4.1 Converting String to Integer

```python
age = "25"

age = int(age)

print(age)
print(type(age))
```

Output:

```
25
<class 'int'>
```

### 4.2 Converting Integer to Float

```python
age = 25

age = float(age)

print(age)
print(type(age))
```

Output:

```
25.0
<class 'float'>
```

### 4.3 Converting Float to Integer

When converting a float to an integer, the decimal portion is removed.

```python
amount = 1250.75

amount = int(amount)

print(amount)
```

Output:

```
1250
```

Important:

`int()` does not round the value.

For example:

```python
value = 99.99

print(int(value))
```

Output:

```
99
```

### 4.4 Converting Integer to String

```python
customer_id = 1001

customer_id = str(customer_id)

print(customer_id)
print(type(customer_id))
```

Output:

```
1001
<class 'str'>
```

This is useful when combining numbers with strings.

```python
customer_id = 1001

message = "Customer ID: " + str(customer_id)

print(message)
```

Output:

```
Customer ID: 1001
```

### 4.5 Converting String to Float

```python
price = "1499.50"

price = float(price)

print(price)
print(type(price))
```

Output:

```
1499.5
<class 'float'>
```

### 4.6 Converting List to Tuple

```python
cities = ["Chennai", "Mumbai", "Delhi"]

cities = tuple(cities)

print(cities)
print(type(cities))
```

Output:

```
('Chennai', 'Mumbai', 'Delhi')
<class 'tuple'>
```

### 4.7 Converting Tuple to List

```python
cities = ("Chennai", "Mumbai", "Delhi")

cities = list(cities)

print(cities)
print(type(cities))
```

Output:

```
['Chennai', 'Mumbai', 'Delhi']
<class 'list'>
```

### 4.8 Converting List to Set

A set removes duplicate values.

```python
cities = ["Chennai", "Mumbai", "Chennai", "Delhi"]

unique_cities = set(cities)

print(unique_cities)
```

### Data Engineering Example

Suppose a CSV file contains:

```
"1001"
"1002"
"1003"
```

These values may initially be read as strings.

We can convert them into integers:

```python
customer_id = "1001"

customer_id = int(customer_id)

print(customer_id)
print(type(customer_id))
```

This becomes important when performing calculations, comparisons, joins, filtering, or loading data into a database with a specific column type.

## 5. type()

### Definition

The `type()` function is used to determine the data type of a value or variable.

### Syntax

```python
type(value)
```

### Basic Example

```python
name = "Arun"

print(type(name))
```

Output:

```
<class 'str'>
```

### Checking Different Data Types

```python
age = 25
salary = 45000.50
name = "Arun"
is_active = True

print(type(age))
print(type(salary))
print(type(name))
print(type(is_active))
```

Output:

```
<class 'int'>
<class 'float'>
<class 'str'>
<class 'bool'>
```

### Checking Collection Types

```python
cities = ["Chennai", "Mumbai", "Delhi"]
coordinates = (13.08, 80.27)
unique_cities = {"Chennai", "Mumbai"}
customer = {"id": 1001, "name": "Arun"}

print(type(cities))
print(type(coordinates))
print(type(unique_cities))
print(type(customer))
```

Output:

```
<class 'list'>
<class 'tuple'>
<class 'set'>
<class 'dict'>
```

### Checking None

```python
value = None

print(type(value))
```

Output:

```
<class 'NoneType'>
```

## 6. Data Engineering Example

Consider data coming from a source system:

```python
customer_id = "1001"
customer_name = "Arun"
order_amount = "2500.75"
order_count = "5"
is_active = "True"
```

Before processing the data, we may need to inspect the data types.

```python
print(type(customer_id))
print(type(customer_name))
print(type(order_amount))
print(type(order_count))
print(type(is_active))
```

The output will be:

```
<class 'str'>
<class 'str'>
<class 'str'>
<class 'str'>
<class 'str'>
```

Even values that look like numbers are strings.

We can convert them:

```python
customer_id = int(customer_id)
order_amount = float(order_amount)
order_count = int(order_count)

print(type(customer_id))
print(type(order_amount))
print(type(order_count))
```

Output:

```
<class 'int'>
<class 'float'>
<class 'int'>
```

This is a simple example of **data type handling during a data transformation process**.

## 7. Complete Example

The following example combines variables, naming rules, data types, type conversion, and `type()`.

```python
# Source data

customer_id = "1001"
customer_name = "Arun"
order_amount = "2500.75"
order_count = "5"

# Check original data types

print("Original Data Types:")
print(type(customer_id))
print(type(customer_name))
print(type(order_amount))
print(type(order_count))

# Convert data types

customer_id = int(customer_id)
order_amount = float(order_amount)
order_count = int(order_count)

# Check converted data types

print("\nConverted Data Types:")
print(type(customer_id))
print(type(customer_name))
print(type(order_amount))
print(type(order_count))

# Perform calculation

total_value = order_amount * order_count

print("\nTotal Value:", total_value)
```

Output:

```
Original Data Types:
<class 'str'>
<class 'str'>
<class 'str'>
<class 'str'>

Converted Data Types:
<class 'int'>
<class 'str'>
<class 'float'>
<class 'int'>

Total Value: 12503.75
```

## Key Takeaways

1. A variable is a name used to store or reference a value.
2. Python automatically determines the variable's data type.
3. Variable names should be meaningful and follow Python naming rules.
4. Python is case-sensitive.
5. Variable names cannot start with numbers.
6. Python keywords cannot be used as variable names.
7. Common data types include `int`, `float`, `str`, `bool`, `list`, `tuple`, `set`, and `dict`.
8. Type conversion changes a value from one data type to another.
9. Common conversion functions include `int()`, `float()`, `str()`, `bool()`, `list()`, `tuple()`, and `set()`.
10. The `type()` function is used to identify the data type of a value.
11. Type conversion is an important part of data cleaning and transformation in Data Engineering.
12. Data coming from CSV files, APIs, databases, and other systems may require type conversion before further processing.

## Day - 2

#### input(), print(), Formatted Output, Multiple Inputs

## input(), print(), Formatted Output, and Multiple Inputs

## 1. print()

### Definition

The `print()` function is used to display information on the screen.

It is one of the most commonly used functions in Python for displaying values, messages, results, and processed data.

### Syntax

```python
print(value)
```

### Basic Example

```python
print("Hello Python")
```

Output:

```
Hello Python
```

### Printing Variables

```python
name = "Arun"
age = 25

print(name)
print(age)
```

Output:

```
Arun
25
```

### Printing Multiple Values

Multiple values can be passed to `print()`.

```python
name = "Arun"
age = 25
city = "Chennai"

print(name, age, city)
```

Output:

```
Arun 25 Chennai
```

### Using `sep`

The `sep` parameter defines the separator between multiple values.

By default, Python uses a space.

```python
name = "Arun"
age = 25
city = "Chennai"

print(name, age, city, sep=" | ")
```

Output:

```
Arun | 25 | Chennai
```

#### Data Engineering Example

Suppose a pipeline processes multiple datasets.

```python
customers = 10000
orders = 25000
products = 5000

print("Customers:", customers)
print("Orders:", orders)
print("Products:", products)
```

Output:

```
Customers: 10000
Orders: 25000
Products: 5000
```

This type of output is useful for displaying pipeline processing information and debugging.

## 2. input()

### Definition

The `input()` function is used to receive information from the user through the keyboard.

### Syntax

```python
variable = input("Message")
```

### Basic Example

```python
name = input("Enter your name: ")

print(name)
```

If the user enters:

```
Arun
```

Output:

```
Arun
```

### Important Point

The `input()` function always returns the user's input as a **string**.

For example:

```python
age = input("Enter your age: ")

print(age)
print(type(age))
```

If the user enters:

```
25
```

Output:

```
25
<class 'str'>
```

Even though `25` looks like a number, Python receives it as a string.

### Converting Input to Integer

If we need to perform mathematical operations, we can convert the input into an integer.

```python
age = int(input("Enter your age: "))

print(age)
print(type(age))
```

If the user enters:

```
25
```

Output:

```
25
<class 'int'>
```

### Converting Input to Float

```python
salary = float(input("Enter your salary: "))

print(salary)
print(type(salary))
```

If the user enters:

```
45000.50
```

Output:

```
45000.5
<class 'float'>
```

## 3. Using input() for Calculations

Because `input()` returns a string, numerical inputs should be converted before performing calculations.

### Incorrect Example

```python
num1 = input("Enter first number: ")
num2 = input("Enter second number: ")

result = num1 + num2

print(result)
```

If the user enters:

```
10
20
```

Output:

```
1020
```

This happens because both values are strings, so Python performs string concatenation.

### Correct Example

```python
num1 = int(input("Enter first number: "))
num2 = int(input("Enter second number: "))

result = num1 + num2

print(result)
```

If the user enters:

```
10
20
```

Output:

```
30
```

### Data Engineering Example

Suppose we want to process a specific number of records.

```python
record_count = int(input("Enter number of records: "))

print("Records to process:", record_count)
```

The input can then be used in calculations or processing logic.

## 4. Formatted Output

### Definition

Formatted output means presenting data in a clear and readable format.

This is especially important in Data Engineering when displaying:

* Pipeline status
* Record counts
* Processing times
* File names
* Data quality results
* Error messages
* Summary reports

Python provides several ways to format output.

The most commonly used approaches are:

1. String concatenation
2. `str.format()`
3. f-strings

## 5. String Concatenation

String concatenation means joining strings together using the `+` operator.

```python
name = "Arun"
city = "Chennai"

message = "Customer " + name + " lives in " + city

print(message)
```

Output:

```
Customer Arun lives in Chennai
```

#### Important

When concatenating a number with a string, the number must first be converted into a string.

```python
customer_id = 1001

message = "Customer ID: " + str(customer_id)

print(message)
```

Output:

```
Customer ID: 1001
```

## 6. Using str.format()

The `format()` method allows values to be inserted into a string.

```python
name = "Arun"
age = 25

message = "My name is {} and I am {} years old.".format(name, age)

print(message)
```

Output:

```
My name is Arun and I am 25 years old.
```

### Using Named Placeholders

```python
name = "Arun"
city = "Chennai"

message = "Customer {customer} is from {location}".format(
    customer=name,
    location=city
)

print(message)
```

Output:

```
Customer Arun is from Chennai
```

## 7. f-Strings

### Definition

An f-string is a convenient way to insert variables and expressions directly into a string.

It is one of the most commonly used formatting techniques in modern Python.

### Syntax

```python
f"Text {variable}"
```

### Basic Example

```python
name = "Arun"
age = 25

print(f"My name is {name} and I am {age} years old.")
```

Output:

```
My name is Arun and I am 25 years old.
```

### Multiple Variables

```python
customer_id = 1001
customer_name = "Arun"
order_amount = 2500.75

print(
    f"Customer ID: {customer_id}, "
    f"Name: {customer_name}, "
    f"Order Amount: {order_amount}"
)
```

Output:

```
Customer ID: 1001, Name: Arun, Order Amount: 2500.75
```

### Expressions Inside f-Strings

Python expressions can also be placed inside `{}`.

```python
price = 1000
quantity = 5

print(f"Total Amount: {price * quantity}")
```

Output:

```
Total Amount: 5000
```

### Formatting Decimal Values

We can control the number of decimal places using `.2f`.

```python
amount = 12500.5678

print(f"Amount: {amount:.2f}")
```

Output:

```
Amount: 12500.57
```

#### Data Engineering Example

Suppose a pipeline processes a dataset.

```python
file_name = "orders.csv"
records = 50000
processing_time = 12.4567

print(
    f"File: {file_name} | "
    f"Records: {records} | "
    f"Processing Time: {processing_time:.2f} seconds"
)
```

Output:

```
File: orders.csv | Records: 50000 | Processing Time: 12.46 seconds
```

This is useful for creating readable pipeline logs.

## 8. Multiple Inputs

### Definition

Multiple inputs means accepting more than one value from the user.

There are different ways to achieve this in Python.

### Method 1: Separate input() Statements

```python
name = input("Enter your name: ")
age = int(input("Enter your age: "))
city = input("Enter your city: ")

print(f"Name: {name}")
print(f"Age: {age}")
print(f"City: {city}")
```

Example input:

```
Enter your name: Arun
Enter your age: 25
Enter your city: Chennai
```

Output:

```
Name: Arun
Age: 25
City: Chennai
```

### Method 2: Multiple Values Using split()

The `split()` method divides a string into multiple parts.

```python
name, city = input("Enter name and city: ").split()

print(name)
print(city)
```

Input:

```
Arun Chennai
```

Output:

```
Arun
Chennai
```

### Multiple Integer Inputs

Since `input()` returns strings, we can convert the values using `map()`.

```python
num1, num2 = map(int, input("Enter two numbers: ").split())

print(num1)
print(num2)
```

Input:

```
10 20
```

Output:

```
10
20
```

We can perform calculations:

```python
num1, num2 = map(int, input("Enter two numbers: ").split())

total = num1 + num2

print(f"Total: {total}")
```

Input:

```
10 20
```

Output:

```
Total: 30
```

### Multiple Float Inputs

```python
price1, price2, price3 = map(
    float,
    input("Enter three prices: ").split()
)

print(price1)
print(price2)
print(price3)
```

Input:

```
100.50 250.75 300.25
```

Output:

```
100.5
250.75
300.25
```

## 9. Multiple Inputs Using a List

When the number of inputs is not fixed, we can store the values in a list.

```python
numbers = list(map(int, input("Enter numbers: ").split()))

print(numbers)
```

Input:

```
10 20 30 40 50
```

Output:

```
[10, 20, 30, 40, 50]
```

We can perform operations on the list:

```python
numbers = list(map(int, input("Enter numbers: ").split()))

total = sum(numbers)

print(f"Total: {total}")
```

Input:

```
10 20 30 40 50
```

Output:

```
Total: 150
```

This pattern is useful when processing a variable number of values.

## 10. print() Parameters

The `print()` function has several useful parameters.

### sep

`sep` specifies the separator between multiple values.

```python
print("2026", "08", "17", sep="-")
```

Output:

```
2026-08-17
```

### end

By default, `print()` moves to a new line after displaying the output.

The `end` parameter can change this behavior.

```python
print("Hello", end=" ")
print("Python")
```

Output:

```
Hello Python
```

Another example:

```python
print("Loading", end="...")
print("Completed")
```

Output:

```
Loading...Completed
```

## 11. Data Engineering Example

Imagine a simple data processing pipeline where the user provides a file name and the number of records.

```python
file_name = input("Enter file name: ")
record_count = int(input("Enter number of records: "))

print("\nData Processing Summary")
print("-----------------------")
print(f"File Name      : {file_name}")
print(f"Record Count   : {record_count}")
print(f"Status         : Processing")
```

Example input:

```
Enter file name: customers.csv
Enter number of records: 50000
```

Output:

```
Data Processing Summary
-----------------------
File Name      : customers.csv
Record Count   : 50000
Status         : Processing
```

This demonstrates how `input()` and formatted `print()` can be used to build simple interactive data-processing programs.

## 12. Complete Example

The following example combines:

* `input()`
* `print()`
* Multiple inputs
* Type conversion
* f-strings
* Formatted output

```python
# Get customer information

customer_id = int(input("Enter customer ID: "))
customer_name = input("Enter customer name: ")
city = input("Enter city: ")

# Get order information

order_count = int(input("Enter number of orders: "))
total_amount = float(input("Enter total order amount: "))

# Calculate average order value

average_order_value = total_amount / order_count

# Display formatted output

print("\n" + "=" * 40)
print("CUSTOMER ORDER SUMMARY")
print("=" * 40)

print(f"Customer ID          : {customer_id}")
print(f"Customer Name        : {customer_name}")
print(f"City                 : {city}")
print(f"Number of Orders     : {order_count}")
print(f"Total Order Amount   : ₹{total_amount:.2f}")
print(f"Average Order Value  : ₹{average_order_value:.2f}")

print("=" * 40)
```

Example input:

```
Enter customer ID: 1001
Enter customer name: Arun
Enter city: Chennai
Enter number of orders: 5
Enter total order amount: 12500
```

Output:

```
========================================
CUSTOMER ORDER SUMMARY
========================================
Customer ID          : 1001
Customer Name        : Arun
City                 : Chennai
Number of Orders     : 5
Total Order Amount   : ₹12500.00
Average Order Value  : ₹2500.00
========================================
```

## Key Takeaways

1. `print()` is used to display information.
2. `input()` is used to receive user input.
3. `input()` always returns a string.
4. Use `int()` or `float()` when numerical input is required.
5. `print()` can display multiple values.
6. `sep` controls the separator between printed values.
7. `end` controls what is printed at the end of the output.
8. String concatenation uses the `+` operator.
9. `str.format()` can be used to format output.
10. f-strings provide a simple and readable way to format output.
11. Expressions can be evaluated directly inside f-strings.
12. `.2f` can be used to display a number with two decimal places.
13. `split()` can be used to accept multiple values from one input.
14. `map()` can be combined with `split()` to convert multiple inputs.
15. These concepts are useful for building interactive scripts and simple Data Engineering utilities.

## Bitwise, Identity, and Membership Operators

Python provides different types of operators for performing operations on values.

In this topic, we will learn:

1. Bitwise Operators
2. Identity Operators
3. Membership Operators

These operators are useful for understanding how Python handles binary data, object references, and collections.

***

## 1. Bitwise Operators

### Definition

Bitwise operators perform operations directly on the binary representation of integers.

Computers internally represent numbers using binary values:

```
0 and 1
```

Bitwise operators work at the individual bit level.

### Real-World Analogy

Think of a row of switches.

Each switch can have only two states:

```
OFF = 0
ON  = 1
```

For example:

```
8  = 1000
5  = 0101
```

A bitwise operation compares or manipulates these individual switches.

### Common Bitwise Operators

| Operator | Name        | Description                         |
| -------- | ----------- | ----------------------------------- |
| `&`      | AND         | Sets bit to 1 if both bits are 1    |
| \`       | \`          | OR                                  |
| `^`      | XOR         | Sets bit to 1 if bits are different |
| `~`      | NOT         | Inverts the bits                    |
| `<<`     | Left Shift  | Shifts bits to the left             |
| `>>`     | Right Shift | Shifts bits to the right            |

***

## 2. Bitwise AND `&`

### Definition

The bitwise AND operator compares two numbers bit by bit.

A bit becomes `1` only when both corresponding bits are `1`.

### Truth Table

| A | B | A & B |
| - | - | ----- |
| 0 | 0 | 0     |
| 0 | 1 | 0     |
| 1 | 0 | 0     |
| 1 | 1 | 1     |

### Example

Consider:

```
8 = 1000
5 = 0101
```

Perform AND:

```
  1000
& 0101
------
  0000
```

Therefore:

```python
result = 8 & 5

print(result)
```

Output:

```
0
```

### Another Example

```
12 = 1100
10 = 1010
```

```
  1100
& 1010
------
  1000
```

`1000` in decimal is `8`.

```python
result = 12 & 10

print(result)
```

Output:

```
8
```

### Data Engineering Example

Bitwise AND is commonly used when working with flags or permission values.

For example, suppose different permissions are represented using bits:

```
READ    = 1
WRITE   = 2
DELETE  = 4
```

We can check whether a permission exists using `&`.

```python
READ = 1
WRITE = 2
DELETE = 4

user_permissions = READ | WRITE

if user_permissions & WRITE:
    print("User has write permission")
```

Output:

```
User has write permission
```

This concept is useful when working with compact flags and binary data.

***

## 3. Bitwise OR `|`

### Definition

The bitwise OR operator returns `1` when at least one of the corresponding bits is `1`.

### Truth Table

| A | B | A \| B |
| - | - | ------ |
| 0 | 0 | 0      |
| 0 | 1 | 1      |
| 1 | 0 | 1      |
| 1 | 1 | 1      |

### Example

```
8 = 1000
5 = 0101
```

```
  1000
| 0101
------
  1101
```

`1101` is `13`.

```python
result = 8 | 5

print(result)
```

Output:

```
13
```

### Combining Flags

```python
READ = 1
WRITE = 2
DELETE = 4

permissions = READ | WRITE

print(permissions)
```

Output:

```
3
```

Here:

```
READ  = 001
WRITE = 010

OR

001
010
---
011
```

`011` represents `3`.

***

## 4. Bitwise XOR `^`

### Definition

XOR stands for **Exclusive OR**.

It returns `1` when the two corresponding bits are different.

### Truth Table

| A | B | A ^ B |
| - | - | ----- |
| 0 | 0 | 0     |
| 0 | 1 | 1     |
| 1 | 0 | 1     |
| 1 | 1 | 0     |

### Example

```
8 = 1000
5 = 0101
```

```
  1000
^ 0101
------
  1101
```

Therefore:

```python
result = 8 ^ 5

print(result)
```

Output:

```
13
```

XOR is commonly used in low-level programming, checksums, cryptography-related operations, and data processing algorithms.

***

## 5. Bitwise NOT `~`

### Definition

The bitwise NOT operator `~` inverts the bits of a number.

It changes:

```
0 → 1
1 → 0
```

Example:

```python
x = 5

print(~x)
```

Output:

```
-6
```

Python represents integers using signed integer arithmetic, so the result may look different from a simple visual bit inversion.

A useful rule for Python is:

```
~x = -(x + 1)
```

For example:

```python
x = 5

print(~x)
print(-(x + 1))
```

Output:

```
-6
-6
```

***

## 6. Bitwise Left Shift `<<`

### Definition

The left shift operator moves the bits of a number to the left.

```python
x << n
```

means shift the bits of `x` left by `n` positions.

### Example

```python
x = 5

print(x << 1)
```

Binary representation:

```
5 = 0101
```

Shift left by one:

```
1010
```

`1010` is `10`.

Output:

```
10
```

### Important Concept

For positive integers:

```
x << 1
```

is equivalent to:

```
x * 2
```

Example:

```python
x = 5

print(x << 1)
print(x * 2)
```

Output:

```
10
10
```

***

## 7. Bitwise Right Shift `>>`

### Definition

The right shift operator moves the bits of a number to the right.

```python
x >> n
```

### Example

```python
x = 20

print(x >> 1)
```

Binary representation:

```
20 = 10100
```

Shift right by one:

```
01010
```

This represents `10`.

Output:

```
10
```

For positive integers:

```
x >> 1
```

is approximately equivalent to:

```
x // 2
```

Example:

```python
x = 20

print(x >> 1)
print(x // 2)
```

Output:

```
10
10
```

***

## 8. Identity Operators

### Definition

Identity operators are used to check whether two variables refer to the **same object in memory**.

Python provides two identity operators:

| Operator | Description                                                      |
| -------- | ---------------------------------------------------------------- |
| `is`     | Returns `True` if both variables refer to the same object        |
| `is not` | Returns `True` if both variables do not refer to the same object |

### Real-World Analogy

Imagine two people having the same address.

Two variables may contain the same value, but they may not necessarily refer to the same object.

Think of:

```
Object A → Box 1
Object B → Box 2
```

Both boxes might contain:

```
"Chennai"
```

The values are the same, but the boxes are different.

Identity checks whether they are the **same box/object**, not merely whether they contain the same value.

***

## 9. `is`

### Definition

The `is` operator checks whether two variables refer to the same object.

Example:

```python
x = [1, 2, 3]
y = x

print(x is y)
```

Output:

```
True
```

Both variables refer to the same list object.

### Modifying One Variable

```python
x = [1, 2, 3]
y = x

y.append(4)

print(x)
print(y)
```

Output:

```
[1, 2, 3, 4]
[1, 2, 3, 4]
```

Because `x` and `y` refer to the same object.

***

## 10. `is not`

### Definition

The `is not` operator checks whether two variables do not refer to the same object.

```python
x = [1, 2, 3]
y = [1, 2, 3]

print(x is not y)
```

Output:

```
True
```

Although both lists contain the same values, they are two separate list objects.

***

## 11. `==` vs `is`

This is an important distinction.

`==` checks whether two values are equal.

`is` checks whether two variables refer to the same object.

Example:

```python
x = [1, 2, 3]
y = [1, 2, 3]

print(x == y)
print(x is y)
```

Output:

```
True
False
```

Why?

```
x == y
```

checks:

```
Do they contain the same values?
```

Answer:

```
Yes
```

While:

```
x is y
```

checks:

```
Are they the exact same object?
```

Answer:

```
No
```

### Important Rule

Use:

```python
==
```

when comparing values.

Use:

```python
is
```

when checking object identity.

A common and important Python use of `is` is checking for `None`.

```python
value = None

if value is None:
    print("No value available")
```

Output:

```
No value available
```

***

## 12. Membership Operators

### Definition

Membership operators are used to check whether a value exists inside a collection such as:

* String
* List
* Tuple
* Set
* Dictionary

Python provides two membership operators:

| Operator | Description                              |
| -------- | ---------------------------------------- |
| `in`     | Returns `True` if a value exists         |
| `not in` | Returns `True` if a value does not exist |

### Real-World Analogy

Imagine checking a warehouse inventory.

You want to know:

```
Is product "Laptop" available in the inventory?
```

Instead of manually checking every product, you can use a membership operation.

***

## 13. `in`

### Definition

The `in` operator checks whether a value exists inside a collection.

### List Example

```python
cities = ["Chennai", "Mumbai", "Delhi"]

print("Chennai" in cities)
```

Output:

```
True
```

Example:

```python
cities = ["Chennai", "Mumbai", "Delhi"]

print("Bangalore" in cities)
```

Output:

```
False
```

### String Example

The `in` operator can also check whether a substring exists inside a string.

```python
message = "Welcome to Data Engineering"

print("Data" in message)
```

Output:

```
True
```

Another example:

```python
email = "user@gmail.com"

print("@gmail.com" in email)
```

Output:

```
True
```

***

## 14. `not in`

### Definition

The `not in` operator returns `True` when a value does not exist in a collection.

```python
cities = ["Chennai", "Mumbai", "Delhi"]

print("Bangalore" not in cities)
```

Output:

```
True
```

Because `"Bangalore"` is not present in the list.

***

## 15. Membership with Tuples

```python
departments = ("IT", "HR", "Finance", "Sales")

print("IT" in departments)
print("Marketing" in departments)
```

Output:

```
True
False
```

***

## 16. Membership with Sets

Sets are particularly useful when checking membership because they are designed for fast membership testing.

```python
cities = {"Chennai", "Mumbai", "Delhi", "Bangalore"}

print("Chennai" in cities)
print("Pune" in cities)
```

Output:

```
True
False
```

### Data Engineering Example

Suppose we have a set of valid file extensions.

```python
valid_extensions = {".csv", ".json", ".parquet"}

file_extension = ".csv"

if file_extension in valid_extensions:
    print("Valid file format")
else:
    print("Invalid file format")
```

Output:

```
Valid file format
```

This type of check can be useful when validating incoming files in a data pipeline.

***

## 17. Membership with Dictionaries

When using `in` with a dictionary, Python checks the **keys** by default.

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

To check dictionary values:

```python
customer = {
    "id": 1001,
    "name": "Arun",
    "city": "Chennai"
}

print("Arun" in customer.values())
```

Output:

```
True
```

***

## 18. Data Engineering Example - Validating Columns

Suppose we receive a dataset and need to verify whether required columns exist.

```python
columns = ["customer_id", "customer_name", "email", "order_amount"]

required_column = "customer_id"

if required_column in columns:
    print("Required column is available")
else:
    print("Required column is missing")
```

Output:

```
Required column is available
```

We can check multiple required columns:

```python
columns = [
    "customer_id",
    "customer_name",
    "email",
    "order_amount"
]

required_columns = [
    "customer_id",
    "customer_name",
    "order_amount"
]

for column in required_columns:
    if column in columns:
        print(f"{column}: Available")
    else:
        print(f"{column}: Missing")
```

Output:

```
customer_id: Available
customer_name: Available
order_amount: Available
```

This is a simple example of **schema validation**, which is an important concept in Data Engineering.

***

## 19. Data Engineering Example - Checking File Names

Suppose a pipeline expects files containing specific keywords.

```python
file_name = "customer_orders_2026.csv"

if "orders" in file_name:
    print("Order file detected")
else:
    print("Unknown file")
```

Output:

```
Order file detected
```

We can also validate file extensions:

```python
file_name = "customer_orders_2026.csv"

if file_name.endswith(".csv"):
    print("CSV file detected")
else:
    print("Unsupported file format")
```

Membership operators and string methods can work together when validating incoming data files.

## 20. Comparison of the Three Concepts

| Concept    | Operators      | Purpose                                      |
| ---------- | -------------- | -------------------------------------------- |
| Bitwise    | `&`, \`        | `,` ^`,` \~`,` <<`,` >>\`                    |
| Identity   | `is`, `is not` | Check whether objects are the same           |
| Membership | `in`, `not in` | Check whether a value exists in a collection |

***

## 21. Complete Example

The following example combines all three concepts.

```python
# Bitwise Operators

a = 12
b = 10

print("Bitwise AND:", a & b)
print("Bitwise OR :", a | b)
print("Bitwise XOR:", a ^ b)


# Identity Operators

data = [1, 2, 3]

data_copy = data
data_new = [1, 2, 3]

print("\nIdentity Checks:")

print(data is data_copy)
print(data is data_new)


# Membership Operators

cities = ["Chennai", "Mumbai", "Delhi"]

print("\nMembership Checks:")

print("Chennai" in cities)
print("Bangalore" in cities)
print("Bangalore" not in cities)
```

Output:

```
Bitwise AND: 8
Bitwise OR : 14
Bitwise XOR: 6

Identity Checks:
True
False

Membership Checks:
True
False
True
```

## Key Takeaways

1. Bitwise operators work on the binary representation of integers.
2. `&` performs bitwise AND.
3. `|` performs bitwise OR.
4. `^` performs bitwise XOR.
5. `~` performs bitwise NOT.
6. `<<` shifts bits to the left.
7. `>>` shifts bits to the right.
8. Identity operators are `is` and `is not`.
9. `is` checks whether two variables refer to the same object.
10. `==` checks value equality, while `is` checks object identity.
11. Use `is None` when checking whether a variable contains `None`.
12. Membership operators are `in` and `not in`.
13. `in` checks whether a value exists in a collection.
14. `not in` checks whether a value does not exist in a collection.
15. Membership operators are useful for validating lists, sets, strings, dictionary keys, file names, and dataset columns.
16. These concepts become useful in Data Engineering for permission flags, binary data, object handling, schema validation, and data/file validation.

## Conditional Statements - if, if-else, elif, Nested if, and Ternary Operator

### Definition

Conditional statements are used to make decisions in a Python program.

They allow a program to execute different blocks of code depending on whether a condition is `True` or `False`.

### Real-World Analogy

Think about a Data Engineering pipeline that receives a file.

The pipeline may need to make decisions:

```
If the file is valid
    → Process the file

Otherwise
    → Reject the file
```

Another example:

```
If record count > 0
    → Continue processing

Otherwise
    → Stop the pipeline
```

Conditional statements allow Python programs to make these kinds of decisions.

## 1. if Statement

### Definition

The `if` statement executes a block of code only when a specified condition is `True`.

### Syntax

```python
if condition:
    # code to execute
```

Python uses indentation to identify the block of code belonging to the `if` statement.

### Basic Example

```python
age = 25

if age >= 18:
    print("Eligible to vote")
```

Output:

```
Eligible to vote
```

If the condition is `False`, the code inside the `if` block will not execute.

```python
age = 15

if age >= 18:
    print("Eligible to vote")
```

There will be no output because:

```
15 >= 18
```

is `False`.

### Data Engineering Example

Suppose we need to process a file only when it contains records.

```python
record_count = 5000

if record_count > 0:
    print("Records found. Start processing.")
```

Output:

```
Records found. Start processing.
```

This is a simple example of conditional processing in a data pipeline.

## 2. if-else Statement

### Definition

The `if-else` statement provides two possible execution paths.

* If the condition is `True`, the `if` block executes.
* If the condition is `False`, the `else` block executes.

### Syntax

```python
if condition:
    # code when condition is True
else:
    # code when condition is False
```

### Example

```python
age = 16

if age >= 18:
    print("Adult")
else:
    print("Minor")
```

Output:

```
Minor
```

### Data Engineering Example

Suppose a pipeline checks whether a file contains data.

```python
record_count = 0

if record_count > 0:
    print("File contains data. Processing started.")
else:
    print("File is empty. Processing skipped.")
```

Output:

```
File is empty. Processing skipped.
```

This type of condition is commonly used for basic data validation.

## 3. elif Statement

### Definition

The `elif` statement means **else if**.

It is used when there are multiple conditions to check.

Python checks the conditions from top to bottom.

Once a condition becomes `True`, its block is executed and the remaining conditions are skipped.

### Syntax

```python
if condition1:
    # code
elif condition2:
    # code
elif condition3:
    # code
else:
    # code
```

### Example

```python
marks = 75

if marks >= 90:
    print("Grade A")
elif marks >= 75:
    print("Grade B")
elif marks >= 50:
    print("Grade C")
else:
    print("Fail")
```

Output:

```
Grade B
```

### How Python Evaluates It

For:

```python
marks = 75
```

Python checks:

```
75 >= 90 → False
75 >= 75 → True
```

Therefore:

```
Grade B
```

is printed.

The remaining conditions are not checked.

### Data Engineering Example

Suppose we classify a data pipeline based on the number of records processed.

```python
record_count = 75000

if record_count >= 100000:
    print("Large dataset")
elif record_count >= 50000:
    print("Medium dataset")
elif record_count > 0:
    print("Small dataset")
else:
    print("Empty dataset")
```

Output:

```
Medium dataset
```

This can be useful when applying different processing strategies based on data volume.

## 4. Multiple elif Conditions

We can use multiple `elif` statements when there are several possible conditions.

```python
temperature = 35

if temperature >= 40:
    print("Very Hot")
elif temperature >= 30:
    print("Hot")
elif temperature >= 20:
    print("Normal")
elif temperature >= 10:
    print("Cold")
else:
    print("Very Cold")
```

Output:

```
Hot
```

Only one block from the complete `if-elif-else` chain is executed.

## 5. Nested if

### Definition

A nested `if` means placing one `if` statement inside another `if` statement.

It is useful when one decision depends on another decision.

### Real-World Analogy

Imagine a data pipeline receiving a file.

First:

```
Is the file valid?
```

If the answer is yes, then:

```
Does the file contain records?
```

If that is also yes:

```
Process the file.
```

This is a nested decision.

### Syntax

```python
if condition1:
    if condition2:
        # code
```

### Basic Example

```python
age = 25
has_id = True

if age >= 18:
    if has_id:
        print("Entry allowed")
```

Output:

```
Entry allowed
```

The inner `if` is checked only when the outer `if` condition is `True`.

### Example Where Outer Condition Is False

```python
age = 15
has_id = True

if age >= 18:
    if has_id:
        print("Entry allowed")
```

There is no output because the outer condition is `False`.

The inner condition is never evaluated.

## 6. Nested if with else

```python
age = 25
has_id = False

if age >= 18:
    if has_id:
        print("Entry allowed")
    else:
        print("ID is required")
else:
    print("Not eligible")
```

Output:

```
ID is required
```

### Data Engineering Example

Consider a file-processing pipeline.

```python
file_exists = True
record_count = 5000

if file_exists:
    if record_count > 0:
        print("File found and contains records. Start processing.")
    else:
        print("File found but contains no records.")
else:
    print("File not found.")
```

Output:

```
File found and contains records. Start processing.
```

This represents a simple validation workflow:

```
File Exists?
    |
    YES
    |
Records Available?
    |
    YES
    |
Process Data
```

## 7. Nested if with Multiple Conditions

Nested conditions can be used for more complex validation.

```python
file_exists = True
file_format = "csv"
record_count = 10000

if file_exists:
    if file_format == "csv":
        if record_count > 0:
            print("Valid file. Start processing.")
        else:
            print("File is empty.")
    else:
        print("Unsupported file format.")
else:
    print("File does not exist.")
```

Output:

```
Valid file. Start processing.
```

This is similar to the initial validation stages of a data pipeline.

## 8. Ternary Operator

### Definition

The ternary operator is a short form of an `if-else` statement.

It is useful when there are only two possible outcomes.

### Normal if-else

```python
age = 20

if age >= 18:
    status = "Adult"
else:
    status = "Minor"

print(status)
```

Output:

```
Adult
```

### Ternary Version

The same logic can be written in one line:

```python
age = 20

status = "Adult" if age >= 18 else "Minor"

print(status)
```

Output:

```
Adult
```

### Syntax

```python
value_if_true if condition else value_if_false
```

Think of it as:

```
If condition is True
    → use value_if_true

Otherwise
    → use value_if_false
```

### Example

```python
number = 10

result = "Even" if number % 2 == 0 else "Odd"

print(result)
```

Output:

```
Even
```

### Data Engineering Example

We can classify a data load based on the record count.

```python
record_count = 5000

status = "Data Available" if record_count > 0 else "No Data"

print(status)
```

Output:

```
Data Available
```

Another example:

```python
records_processed = 100000

status = "Success" if records_processed > 0 else "Failed"

print(status)
```

Output:

```
Success
```

## 9. Ternary Operator with Data Quality

Suppose we calculate whether a dataset passed a quality check.

```python
invalid_records = 0

quality_status = "Passed" if invalid_records == 0 else "Failed"

print(f"Data Quality: {quality_status}")
```

Output:

```
Data Quality: Passed
```

If:

```python
invalid_records = 25
```

The result becomes:

```
Data Quality: Failed
```

## 10. Comparing if-else and Ternary Operator

### Traditional if-else

```python
record_count = 1000

if record_count > 0:
    status = "Available"
else:
    status = "Empty"

print(status)
```

### Ternary Operator

```python
record_count = 1000

status = "Available" if record_count > 0 else "Empty"

print(status)
```

Both produce:

```
Available
```

### When to Use

Use traditional `if-else` when the logic contains multiple statements or is complex.

Use the ternary operator when the logic is simple and easy to understand.

Good:

```python
status = "Success" if records > 0 else "Failed"
```

Avoid overly complicated ternary expressions because they can make the code difficult to read.

## 11. Combining if, elif, and else

A common pattern is:

```python
if condition1:
    # code
elif condition2:
    # code
elif condition3:
    # code
else:
    # default code
```

Example:

```python
data_quality_score = 92

if data_quality_score >= 90:
    print("Excellent")
elif data_quality_score >= 75:
    print("Good")
elif data_quality_score >= 50:
    print("Needs Improvement")
else:
    print("Poor")
```

Output:

```
Excellent
```

## 12. Data Engineering Example - Data Quality Validation

Consider a dataset validation process.

```python
record_count = 10000
invalid_records = 25
duplicate_records = 10

if record_count == 0:
    print("Dataset is empty.")
elif invalid_records > 100:
    print("Too many invalid records. Reject dataset.")
elif duplicate_records > 50:
    print("Too many duplicate records. Reject dataset.")
else:
    print("Dataset passed validation.")
```

Output:

```
Dataset passed validation.
```

This demonstrates how conditional statements can be used to implement basic data quality rules.

## 13. Data Engineering Example - File Processing Decision

```python
file_exists = True
file_extension = ".csv"
record_count = 25000

if not file_exists:
    print("File not found.")
elif file_extension != ".csv":
    print("Invalid file format.")
elif record_count == 0:
    print("File is empty.")
else:
    print("File is valid. Start processing.")
```

Output:

```
File is valid. Start processing.
```

This represents a simple decision-making process that can occur before processing an incoming file.

## 14. Complete Example

The following example combines:

* `if`
* `if-else`
* `elif`
* Nested `if`
* Ternary operator

```python
# Dataset information

file_exists = True
file_format = "csv"
record_count = 50000
invalid_records = 10

# Main validation

if file_exists:

    if file_format == "csv":

        if record_count > 0:

            if invalid_records == 0:
                quality_status = "Passed"
            else:
                quality_status = "Passed with Warnings"

            print("File found.")
            print("File format is valid.")
            print(f"Records found: {record_count}")
            print(f"Data Quality: {quality_status}")

        else:
            print("File is empty.")

    elif file_format == "json":
        print("JSON file detected.")

    else:
        print("Unsupported file format.")

else:
    print("File does not exist.")


# Ternary operator

load_status = "Ready for Processing" if record_count > 0 else "No Data"

print(f"Load Status: {load_status}")
```

Output:

```
File found.
File format is valid.
Records found: 50000
Data Quality: Passed with Warnings
Load Status: Ready for Processing
```

## 15. Key Takeaways

1. Conditional statements allow Python programs to make decisions.
2. `if` executes code when a condition is `True`.
3. `if-else` provides two possible execution paths.
4. `elif` is used to check multiple conditions.
5. Python checks `if` and `elif` conditions from top to bottom.
6. Once a condition is `True`, the remaining conditions in that chain are skipped.
7. `else` executes when none of the previous conditions are `True`.
8. A nested `if` is an `if` statement inside another `if` statement.
9. Nested conditions are useful when one decision depends on another.
10. The ternary operator is a short form of a simple `if-else`.
11. Ternary syntax is:

```python
value_if_true if condition else value_if_false
```

12. Conditional statements are widely used in Data Engineering for:
    * File validation
    * Data quality checks
    * Schema validation
    * Record count validation
    * Pipeline decisions
    * Error handling
    * Data classification
    * Load status checks

## Loops in Python - for, while, continue, break, pass, and Nested Loops

### Definition

A loop is used to execute a block of code repeatedly.

Instead of writing the same code multiple times, we can use a loop to execute it automatically.

Loops are extremely important in Data Engineering because data often contains thousands or millions of records that need to be processed repeatedly.

### Real-World Analogy

Imagine a Data Engineer receives 5,000 CSV files.

Instead of writing:

```
Process file 1
Process file 2
Process file 3
...
Process file 5000
```

we can use a loop:

```
For each file:
    Read the file
    Validate the file
    Process the data
```

Python mainly provides two types of loops:

1. `for` loop
2. `while` loop

## 1. for Loop

### Definition

A `for` loop is used to iterate over a sequence or collection of values.

It is commonly used when we know what we want to iterate over.

### Syntax

```python
for variable in sequence:
    # code to execute
```

### Basic Example

```python
for number in [1, 2, 3, 4, 5]:
    print(number)
```

Output:

```
1
2
3
4
5
```

The loop takes one value at a time from the list and assigns it to `number`.

### Real-World Analogy

Think of a warehouse with five boxes.

The worker does:

```
Take Box 1 → Process
Take Box 2 → Process
Take Box 3 → Process
Take Box 4 → Process
Take Box 5 → Process
```

The `for` loop performs the same operation automatically.

## 2. for Loop with range()

`range()` is commonly used with `for` loops when we want to generate a sequence of numbers.

### Example

```python
for i in range(5):
    print(i)
```

Output:

```
0
1
2
3
4
```

Notice that `range(5)` starts from `0` and stops before `5`.

### range(start, stop)

```python
for i in range(1, 6):
    print(i)
```

Output:

```
1
2
3
4
5
```

### range(start, stop, step)

```python
for i in range(1, 11, 2):
    print(i)
```

Output:

```
1
3
5
7
9
```

Here:

```
start = 1
stop  = 11
step  = 2
```

### Reverse Loop

```python
for i in range(10, 0, -1):
    print(i)
```

Output:

```
10
9
8
7
6
5
4
3
2
1
```

## 3. for Loop with Strings

A string is also iterable.

```python
name = "Python"

for character in name:
    print(character)
```

Output:

```
P
y
t
h
o
n
```

Each character is processed one at a time.

## 4. for Loop with List

```python
cities = ["Chennai", "Mumbai", "Delhi", "Bangalore"]

for city in cities:
    print(city)
```

Output:

```
Chennai
Mumbai
Delhi
Bangalore
```

### Data Engineering Example

Suppose a pipeline needs to process multiple files.

```python
files = [
    "customers.csv",
    "orders.csv",
    "products.csv",
    "payments.csv"
]

for file in files:
    print(f"Processing file: {file}")
```

Output:

```
Processing file: customers.csv
Processing file: orders.csv
Processing file: products.csv
Processing file: payments.csv
```

This is a simple example of iterating through input files.

## 5. while Loop

### Definition

A `while` loop repeatedly executes a block of code as long as a condition remains `True`.

### Syntax

```python
while condition:
    # code to execute
```

### Basic Example

```python
count = 1

while count <= 5:
    print(count)
    count += 1
```

Output:

```
1
2
3
4
5
```

### How It Works

Initially:

```
count = 1
```

Python checks:

```
1 <= 5 → True
```

So it executes the loop.

Then:

```
count = 2
```

The condition is checked again.

This continues until:

```
count = 6
```

At that point:

```
6 <= 5 → False
```

The loop stops.

## 6. Real-World Example of while Loop

Imagine a data pipeline waiting until all records are processed.

```python
processed_records = 0
total_records = 5

while processed_records < total_records:
    processed_records += 1
    print(f"Processed record {processed_records}")
```

Output:

```
Processed record 1
Processed record 2
Processed record 3
Processed record 4
Processed record 5
```

A `while` loop is useful when the number of iterations depends on a condition.

## 7. for vs while

| `for` Loop                                                      | `while` Loop                              |
| --------------------------------------------------------------- | ----------------------------------------- |
| Used to iterate over a sequence                                 | Runs while a condition is True            |
| Common when iteration count/collection is known                 | Useful when stopping condition is dynamic |
| Works naturally with lists, strings, tuples, dictionaries, etc. | Works based on a Boolean condition        |
| Less likely to create an infinite loop                          | Requires careful condition management     |

Example:

```python
for i in range(5):
    print(i)
```

Compared with:

```python
i = 0

while i < 5:
    print(i)
    i += 1
```

## 8. break

### Definition

The `break` statement immediately terminates the loop.

Once Python encounters `break`, it exits the current loop.

### Example

```python
for number in range(1, 11):

    if number == 5:
        break

    print(number)
```

Output:

```
1
2
3
4
```

When:

```
number == 5
```

becomes `True`, the loop stops.

### Real-World Analogy

Imagine searching through customer records for a specific customer.

Once the customer is found, there is no need to continue searching.

```python
customers = ["Arun", "Rahul", "Karthik", "Priya", "Vijay"]

for customer in customers:

    if customer == "Karthik":
        print("Customer found")
        break

    print(f"Checking: {customer}")
```

Output:

```
Checking: Arun
Checking: Rahul
Customer found
```

The loop stops immediately after finding the customer.

## 9. continue

### Definition

The `continue` statement skips the current iteration and moves to the next iteration of the loop.

### Example

```python
for number in range(1, 6):

    if number == 3:
        continue

    print(number)
```

Output:

```
1
2
4
5
```

When the value is `3`, Python skips that iteration.

### Real-World Data Engineering Example

Suppose we have records and want to skip invalid records.

```python
records = [10, 20, -5, 30, -10, 40]

for record in records:

    if record < 0:
        continue

    print(f"Processing record value: {record}")
```

Output:

```
Processing record value: 10
Processing record value: 20
Processing record value: 30
Processing record value: 40
```

The negative values are skipped.

## 10. break vs continue

| `break`                                     | `continue`                                     |
| ------------------------------------------- | ---------------------------------------------- |
| Stops the entire loop                       | Skips only the current iteration               |
| Execution exits the loop                    | Execution continues with the next iteration    |
| Used when no further processing is required | Used when a particular value should be skipped |

Example:

```python
for number in range(1, 6):

    if number == 3:
        break

    print(number)
```

Output:

```
1
2
```

Using `continue`:

```python
for number in range(1, 6):

    if number == 3:
        continue

    print(number)
```

Output:

```
1
2
4
5
```

## 11. pass

### Definition

The `pass` statement does nothing.

It is used as a placeholder when Python requires a statement but we do not want to execute anything yet.

### Example

```python
for number in range(1, 6):

    if number == 3:
        pass

    print(number)
```

Output:

```
1
2
3
4
5
```

Unlike `continue`, `pass` does not skip the iteration.

### pass vs continue

This is an important difference.

#### pass

```python
for number in range(1, 6):

    if number == 3:
        pass

    print(number)
```

Output:

```
1
2
3
4
5
```

#### continue

```python
for number in range(1, 6):

    if number == 3:
        continue

    print(number)
```

Output:

```
1
2
4
5
```

### When is pass useful?

`pass` is commonly used when designing code that will be implemented later.

```python
def process_data():
    pass
```

The function is syntactically valid even though no implementation has been added yet.

## 12. Nested Loops

### Definition

A nested loop is a loop inside another loop.

The outer loop executes once, and for each iteration of the outer loop, the inner loop completes all its iterations.

### Syntax

```python
for outer_variable in outer_sequence:

    for inner_variable in inner_sequence:
        # code
```

### Basic Example

```python
for i in range(3):

    for j in range(3):
        print(i, j)
```

Output:

```
0 0
0 1
0 2
1 0
1 1
1 2
2 0
2 1
2 2
```

### How It Works

For:

```python
for i in range(3):
```

When:

```
i = 0
```

the inner loop runs completely:

```
0 0
0 1
0 2
```

Then:

```
i = 1
```

The inner loop runs again:

```
1 0
1 1
1 2
```

Finally:

```
i = 2
```

The inner loop runs:

```
2 0
2 1
2 2
```

## 13. Nested Loop with Lists

```python
departments = ["IT", "HR", "Finance"]

employees = ["Arun", "Rahul"]

for department in departments:

    for employee in employees:
        print(department, employee)
```

Output:

```
IT Arun
IT Rahul
HR Arun
HR Rahul
Finance Arun
Finance Rahul
```

The inner loop runs completely for every department.

## 14. Data Engineering Example - Multiple Tables and Columns

Suppose we have multiple datasets and want to inspect their columns.

```python
datasets = {
    "customers": ["customer_id", "name", "city"],
    "orders": ["order_id", "customer_id", "amount"],
    "products": ["product_id", "name", "price"]
}

for table, columns in datasets.items():

    print(f"\nTable: {table}")

    for column in columns:
        print(f"  Column: {column}")
```

Output:

```
Table: customers
  Column: customer_id
  Column: name
  Column: city

Table: orders
  Column: order_id
  Column: customer_id
  Column: amount

Table: products
  Column: product_id
  Column: name
  Column: price
```

This is conceptually similar to iterating through tables and their schemas.

## 15. Nested Loop with Matrix

A common example of nested loops is processing a matrix.

```python
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

for row in matrix:

    for value in row:
        print(value)
```

Output:

```
1
2
3
4
5
6
7
8
9
```

The outer loop processes each row.

The inner loop processes each value inside that row.

## 16. Nested Loop for a Table

```python
for i in range(1, 4):

    for j in range(1, 4):
        print(i * j, end=" ")

    print()
```

Output:

```
1 2 3
2 4 6
3 6 9
```

The inner loop creates each column while the outer loop creates each row.

## 17. break Inside a Nested Loop

When `break` is used inside the inner loop, it only breaks the inner loop.

```python
for i in range(3):

    for j in range(5):

        if j == 2:
            break

        print(i, j)
```

Output:

```
0 0
0 1
1 0
1 1
2 0
2 1
```

The inner loop stops when `j == 2`, but the outer loop continues.

## 18. continue Inside a Nested Loop

```python
for i in range(3):

    for j in range(5):

        if j == 2:
            continue

        print(i, j)
```

Output:

```
0 0
0 1
0 3
0 4
1 0
1 1
1 3
1 4
2 0
2 1
2 3
2 4
```

Only the current iteration of the inner loop is skipped.

## 19. Important Difference Between break, continue, and pass

| Statement  | Behavior                |
| ---------- | ----------------------- |
| `break`    | Terminates the loop     |
| `continue` | Skips current iteration |
| `pass`     | Does nothing            |

Example:

```python
for number in range(1, 6):

    if number == 2:
        pass

    elif number == 3:
        continue

    elif number == 5:
        break

    print(number)
```

Output:

```
1
2
4
```

Explanation:

```
1 → printed
2 → pass → printed
3 → continue → skipped
4 → printed
5 → break → loop stops
```

## 20. Data Engineering Example - Processing Records

Consider a list of records.

```python
records = [
    {"id": 1, "amount": 500},
    {"id": 2, "amount": -100},
    {"id": 3, "amount": 1000},
    {"id": 4, "amount": -50},
    {"id": 5, "amount": 2000}
]

for record in records:

    if record["amount"] < 0:
        continue

    print(
        f"Processing ID: {record['id']} "
        f"Amount: {record['amount']}"
    )
```

Output:

```
Processing ID: 1 Amount: 500
Processing ID: 3 Amount: 1000
Processing ID: 5 Amount: 2000
```

Here, `continue` allows the pipeline to skip invalid records while continuing to process the remaining records.

## 21. Data Engineering Example - Stop Processing on Critical Error

```python
files = [
    "customers.csv",
    "orders.csv",
    "payments.csv",
    "critical_error.csv",
    "products.csv"
]

for file in files:

    if file == "critical_error.csv":
        print("Critical error found. Stopping pipeline.")
        break

    print(f"Processing: {file}")
```

Output:

```
Processing: customers.csv
Processing: orders.csv
Processing: payments.csv
Critical error found. Stopping pipeline.
```

The pipeline stops immediately when the critical file is detected.

## 22. Important Concept - Infinite while Loop

A `while` loop must eventually make its condition `False`.

Incorrect:

```python
count = 1

while count <= 5:
    print(count)
```

This creates an infinite loop because `count` never changes.

Correct:

```python
count = 1

while count <= 5:
    print(count)
    count += 1
```

Always ensure that a `while` loop has a path toward terminating its condition.

## 23. Complete Example

The following example combines:

* `for`
* `while`
* `continue`
* `break`
* `pass`
* Nested loops

```python
# for loop

files = [
    "customers.csv",
    "orders.csv",
    "products.csv"
]

for file in files:
    print(f"Processing: {file}")


# while loop

count = 1

while count <= 3:
    print(f"Batch {count} completed")
    count += 1


# continue

numbers = [10, -5, 20, -10, 30]

print("\nValid Records:")

for number in numbers:

    if number < 0:
        continue

    print(number)


# break

print("\nSearching:")

for number in range(1, 10):

    if number == 5:
        print("Target found")
        break

    print(f"Checking {number}")


# pass

print("\nPass Example:")

for number in range(1, 4):

    if number == 2:
        pass

    print(number)


# Nested loop

print("\nNested Loop:")

datasets = {
    "customers": ["id", "name"],
    "orders": ["id", "amount"]
}

for dataset, columns in datasets.items():

    print(f"Dataset: {dataset}")

    for column in columns:
        print(f"  Column: {column}")
```

Output:

```
Processing: customers.csv
Processing: orders.csv
Processing: products.csv
Batch 1 completed
Batch 2 completed
Batch 3 completed

Valid Records:
10
20
30

Searching:
Checking 1
Checking 2
Checking 3
Checking 4
Target found

Pass Example:
1
2
3

Nested Loop:
Dataset: customers
  Column: id
  Column: name
Dataset: orders
  Column: id
  Column: amount
```

## 24. Practice Questions - Nested Loops

### Question 1 - Find All Pairs

Write a Python program using nested loops to print all possible pairs from the list:

```python
numbers = [1, 2, 3, 4]
```

Expected output:

```
1 1
1 2
1 3
1 4
2 1
2 2
2 3
2 4
3 1
3 2
3 3
3 4
4 1
4 2
4 3
4 4
```

#### Requirement

Use two `for` loops.

Do not use any library.

## Question 2 - Find Common Values

Given two lists:

```python
list1 = [10, 20, 30, 40, 50]
list2 = [30, 40, 50, 60, 70]
```

Use nested loops to find and print the values that exist in both lists.

Expected output:

```
30
40
50
```

#### Requirement

Use nested `for` loops.

Do not use:

```python
set()
```

Do not use:

```python
intersection()
```

## Question 3 - Process Department Employees

You are given the following data:

```python
departments = {
    "IT": ["Arun", "Rahul", "Karthik"],
    "HR": ["Priya", "Divya"],
    "Finance": ["Vijay", "Suresh", "Meena"]
}
```

Write a Python program using nested loops to display every department and its employees.

Expected output:

```
Department: IT
Employee: Arun
Employee: Rahul
Employee: Karthik

Department: HR
Employee: Priya
Employee: Divya

Department: Finance
Employee: Vijay
Employee: Suresh
Employee: Meena
```

#### Requirement

Use:

```python
for
```

inside another:

```python
for
```

Do not manually print each employee.

## Key Takeaways

1. Loops allow us to execute code repeatedly.
2. `for` is commonly used to iterate over collections and sequences.
3. `range()` is commonly used with `for` loops for generating number sequences.
4. `while` executes repeatedly while a condition remains `True`.
5. `break` immediately terminates the current loop.
6. `continue` skips the current iteration and moves to the next iteration.
7. `pass` does nothing and acts as a placeholder.
8. A nested loop is a loop inside another loop.
9. The inner loop completes its iterations for every iteration of the outer loop.
10. `break` inside an inner loop only breaks that inner loop.
11. `continue` inside an inner loop only affects the current inner-loop iteration.
12. `while` loops must have a condition that can eventually become `False`.
13. Loops are fundamental in Data Engineering for processing files, records, datasets, columns, validation rules, and batches.
