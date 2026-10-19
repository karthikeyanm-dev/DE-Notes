# Page

## Functions

> Imported from [`python/Functions.ipynb`](https://github.com/karthikeyanm-dev/DE-Notes/blob/main/python/Functions.ipynb). Code outputs are omitted; run the source notebook for live results.

## Dictionary Comprehension and Functions in Python

## Part 1 - Dictionary Comprehension

### 1. Introduction

Dictionary comprehension is a concise way to create a dictionary using an iterable.

It is similar to list comprehension, but instead of creating a list, it creates a dictionary containing:

```
key : value
```

### Syntax

```python
{key: value for item in iterable}
```

### Real-World Analogy

Suppose we have a list of employee names:

```
Arun
Rahul
Priya
Vijay
```

We want to create a dictionary where:

```
Name → Length of Name
```

Instead of manually creating:

```python
{
    "Arun": 4,
    "Rahul": 5,
    "Priya": 5,
    "Vijay": 5
}
```

we can generate it automatically using dictionary comprehension.

## 2. Basic Dictionary Comprehension

```python
numbers = [1, 2, 3, 4, 5]

squares = {
    number: number ** 2
    for number in numbers
}

print(squares)
```

Output:

```
{1: 1, 2: 4, 3: 9, 4: 16, 5: 25}
```

The structure is:

```python
{
    key: value
    for item in iterable
}
```

In this example:

```
key   → number
value → number ** 2
```

## 3. Dictionary from Two Lists

Suppose we have:

```python
names = ["Arun", "Rahul", "Priya"]
ages = [25, 30, 28]
```

We can create a dictionary using `zip()`.

```python
names = ["Arun", "Rahul", "Priya"]
ages = [25, 30, 28]

result = {
    name: age
    for name, age in zip(names, ages)
}

print(result)
```

Output:

```
{'Arun': 25, 'Rahul': 30, 'Priya': 28}
```

## 4. Dictionary Comprehension with Transformation

Suppose we have employee salaries.

```python
salaries = {
    "Arun": 50000,
    "Rahul": 60000,
    "Priya": 55000
}
```

We want to increase every salary by `10%`.

```python
updated_salaries = {
    name: salary * 1.10
    for name, salary in salaries.items()
}

print(updated_salaries)
```

Output:

```
{
    'Arun': 55000.0,
    'Rahul': 66000.0,
    'Priya': 60500.00000000001
}
```

The original dictionary is not modified.

## 5. Conditional Dictionary Comprehension

We can filter dictionary elements using `if`.

### Syntax

```python
{
    key: value
    for item in iterable
    if condition
}
```

Example:

```python
salaries = {
    "Arun": 50000,
    "Rahul": 60000,
    "Priya": 45000,
    "Vijay": 70000
}

high_salary = {
    name: salary
    for name, salary in salaries.items()
    if salary >= 60000
}

print(high_salary)
```

Output:

```
{
    'Rahul': 60000,
    'Vijay': 70000
}
```

## 6. Dictionary Comprehension with if-else

We can also use an `if-else` expression.

```python
numbers = [1, 2, 3, 4, 5]

result = {
    number: "Even" if number % 2 == 0 else "Odd"
    for number in numbers
}

print(result)
```

Output:

```
{
    1: 'Odd',
    2: 'Even',
    3: 'Odd',
    4: 'Even',
    5: 'Odd'
}
```

## 7. Data Engineering Example - File Classification

Suppose we have files:

```python
files = [
    "customers.csv",
    "orders.json",
    "products.parquet",
    "sales.txt"
]
```

Create a dictionary where each file maps to its extension.

```python
file_types = {
    file: file.split(".")[-1]
    for file in files
}

print(file_types)
```

Output:

```
{
    'customers.csv': 'csv',
    'orders.json': 'json',
    'products.parquet': 'parquet',
    'sales.txt': 'txt'
}
```

## 8. Data Engineering Example - Customer Records

Suppose we have customer records:

```python
customers = [
    {"id": 1001, "name": "Arun", "city": "Chennai"},
    {"id": 1002, "name": "Rahul", "city": "Mumbai"},
    {"id": 1003, "name": "Priya", "city": "Delhi"}
]
```

Create a dictionary where customer ID is the key and name is the value.

```python
customer_lookup = {
    customer["id"]: customer["name"]
    for customer in customers
}

print(customer_lookup)
```

Output:

```
{
    1001: 'Arun',
    1002: 'Rahul',
    1003: 'Priya'
}
```

This type of lookup structure is useful when we frequently need to find information using an ID.

## 9. Dictionary Comprehension from an Existing Dictionary

```python
prices = {
    "Laptop": 50000,
    "Mouse": 1000,
    "Keyboard": 2000
}

discounted_prices = {
    product: price * 0.90
    for product, price in prices.items()
}

print(discounted_prices)
```

Output:

```
{
    'Laptop': 45000.0,
    'Mouse': 900.0,
    'Keyboard': 1800.0
}
```

## 10. Dictionary Comprehension - Key Transformation

We can also transform keys.

```python
data = {
    "name": "Arun",
    "city": "Chennai",
    "department": "Data Engineering"
}

uppercase_keys = {
    key.upper(): value
    for key, value in data.items()
}

print(uppercase_keys)
```

Output:

```
{
    'NAME': 'Arun',
    'CITY': 'Chennai',
    'DEPARTMENT': 'Data Engineering'
}
```

## 11. Dictionary Comprehension - Key and Value Transformation

```python
data = {
    "name": "Arun",
    "city": "Chennai"
}

result = {
    key.upper(): value.upper()
    for key, value in data.items()
}

print(result)
```

Output:

```
{
    'NAME': 'ARUN',
    'CITY': 'CHENNAI'
}
```

## 12. Dictionary Comprehension vs List Comprehension

List comprehension:

```python
numbers = [1, 2, 3, 4]

squares = [
    number ** 2
    for number in numbers
]

print(squares)
```

Output:

```
[1, 4, 9, 16]
```

Dictionary comprehension:

```python
numbers = [1, 2, 3, 4]

squares = {
    number: number ** 2
    for number in numbers
}

print(squares)
```

Output:

```
{1: 1, 2: 4, 3: 9, 4: 16}
```

The key difference is:

```
List comprehension
→ [value]

Dictionary comprehension
→ {key: value}
```

## 13. Practice Problems - Dictionary Comprehension

### Problem 1

Given:

```python
numbers = [1, 2, 3, 4, 5]
```

Create:

```
{
    1: 1,
    2: 4,
    3: 9,
    4: 16,
    5: 25
}
```

using dictionary comprehension.

### Problem 2

Given:

```python
numbers = [10, 20, 30, 40, 50]
```

Create a dictionary containing only numbers greater than `25`.

Expected:

```
{
    30: 30,
    40: 40,
    50: 50
}
```

### Problem 3

Given:

```python
names = ["Arun", "Rahul", "Priya", "Vijay"]
```

Create a dictionary where:

```
key   → name
value → length of name
```

Expected:

```
{
    "Arun": 4,
    "Rahul": 5,
    "Priya": 5,
    "Vijay": 5
}
```

### Problem 4

Given:

```python
employees = {
    "Arun": 45000,
    "Rahul": 65000,
    "Priya": 55000,
    "Vijay": 75000
}
```

Create a new dictionary containing only employees whose salary is greater than or equal to `60000`.

### Problem 5

Given:

```python
files = [
    "customers.csv",
    "orders.json",
    "sales.parquet"
]
```

Create:

```
{
    "customers.csv": "csv",
    "orders.json": "json",
    "sales.parquet": "parquet"
}
```

using dictionary comprehension.

## Part 2 - Functions in Python

## 14. What is a Function?

### Definition

A function is a reusable block of code designed to perform a specific task.

Instead of writing the same logic repeatedly, we can write it once inside a function and call it whenever needed.

### Real-World Analogy

Think of a washing machine.

You don't manually perform every step:

```
Fill water
Add detergent
Wash clothes
Rinse
Drain
Spin
```

You select a mode and press a button.

The machine performs the predefined process.

A function works similarly:

```
Function
   ↓
Give input
   ↓
Perform task
   ↓
Get output
```

For example:

```python
def calculate_square(number):
    return number ** 2
```

We can reuse it:

```python
print(calculate_square(5))
print(calculate_square(10))
print(calculate_square(20))
```

## 15. Why Functions Are Important

Functions provide:

```
Reusability
Readability
Maintainability
Modularity
Testing
Abstraction
```

In Data Engineering, functions are heavily used for:

* Data cleaning
* File processing
* API calls
* Data validation
* Transformation logic
* Database operations
* Logging
* ETL pipelines

## 16. Creating a Function

### Syntax

```python
def function_name():
    # function body
```

Example:

```python
def greet():
    print("Hello Data Engineer")
```

The function does not execute when it is defined.

We need to call it:

```python
greet()
```

Output:

```
Hello Data Engineer
```

## 17. Function Execution Flow

```python
def greet():
    print("Hello")

print("Before function")

greet()

print("After function")
```

Output:

```
Before function
Hello
After function
```

Python executes the function body only when the function is called.

## 18. Function with Parameters

A parameter is a variable defined in the function definition.

```python
def greet(name):
    print(f"Hello {name}")
```

Calling the function:

```python
greet("Arun")
greet("Rahul")
```

Output:

```
Hello Arun
Hello Rahul
```

Here:

```python
name
```

is the parameter.

```python
"Arun"
```

is an argument.

## 19. Parameter vs Argument

Parameter:

```python
def greet(name):
    print(name)
```

`name` is a parameter.

Argument:

```python
greet("Arun")
```

`"Arun"` is an argument.

Think:

```
Parameter → Placeholder
Argument  → Actual value
```

## 20. Multiple Parameters

A function can accept multiple parameters.

```python
def add(a, b):
    print(a + b)

add(10, 20)
```

Output:

```
30
```

Another example:

```python
def employee_details(name, department, salary):

    print("Name:", name)
    print("Department:", department)
    print("Salary:", salary)

employee_details(
    "Arun",
    "Data Engineering",
    60000
)
```

Output:

```
Name: Arun
Department: Data Engineering
Salary: 60000
```

## 21. return Statement

### Definition

`return` sends a value back to the code that called the function.

Example:

```python
def add(a, b):
    return a + b

result = add(10, 20)

print(result)
```

Output:

```
30
```

The important difference:

```python
print()
```

displays a value.

```python
return
```

sends a value back from the function.

## 22. print() vs return()

Example using `print()`:

```python
def add(a, b):
    print(a + b)

result = add(10, 20)

print(result)
```

Output:

```
30
None
```

The function displayed the result but did not return it.

Using `return`:

```python
def add(a, b):
    return a + b

result = add(10, 20)

print(result)
```

Output:

```
30
```

Returned values can be stored and reused.

## 23. Function Returning Multiple Values

Python allows a function to return multiple values.

```python
def calculate(a, b):

    total = a + b
    difference = a - b
    product = a * b

    return total, difference, product
```

Call:

```python
result = calculate(10, 5)

print(result)
```

Output:

```
(15, 5, 50)
```

The function actually returns a tuple.

## 24. Unpacking Function Results

```python
def calculate(a, b):

    return a + b, a - b, a * b

total, difference, product = calculate(10, 5)

print(total)
print(difference)
print(product)
```

Output:

```
15
5
50
```

This uses tuple unpacking.

## 25. Default Parameters

A function can have default values for parameters.

```python
def greet(name="Guest"):
    print(f"Hello {name}")
```

Calling without an argument:

```python
greet()
```

Output:

```
Hello Guest
```

Calling with an argument:

```python
greet("Arun")
```

Output:

```
Hello Arun
```

## 26. Multiple Default Parameters

```python
def employee(name, department="IT", location="Chennai"):

    print(name)
    print(department)
    print(location)

employee("Arun")
```

Output:

```
Arun
IT
Chennai
```

We can override defaults:

```python
employee(
    "Rahul",
    "Data Engineering",
    "Bangalore"
)
```

## 27. Positional Arguments

Arguments are assigned according to their position.

```python
def employee(name, department, salary):

    print(name)
    print(department)
    print(salary)

employee(
    "Arun",
    "Data Engineering",
    60000
)
```

The mapping is:

```
name       → Arun
department → Data Engineering
salary     → 60000
```

## 28. Keyword Arguments

We can specify the parameter names.

```python
def employee(name, department, salary):

    print(name)
    print(department)
    print(salary)

employee(
    name="Arun",
    salary=60000,
    department="Data Engineering"
)
```

The order no longer matters because the parameter names are explicitly provided.

## 29. Positional and Keyword Arguments

We can combine them.

```python
def employee(name, department, salary):

    print(name)
    print(department)
    print(salary)

employee(
    "Arun",
    salary=60000,
    department="Data Engineering"
)
```

Important rule:

Positional arguments must come before keyword arguments.

## 30. \*args

### Definition

`*args` allows a function to accept any number of positional arguments.

Example:

```python
def add_numbers(*numbers):

    total = 0

    for number in numbers:
        total += number

    return total
```

Now we can pass different numbers of arguments:

```python
print(add_numbers(10, 20))
print(add_numbers(10, 20, 30))
print(add_numbers(10, 20, 30, 40, 50))
```

Output:

```
30
60
150
```

Inside the function:

```python
numbers
```

is a tuple.

## 31. \*args Example

```python
def show_values(*values):

    print(values)
    print(type(values))

show_values(10, 20, 30)
```

Output:

```
(10, 20, 30)
<class 'tuple'>
```

## 32. \*\*kwargs

### Definition

`**kwargs` allows a function to accept any number of keyword arguments.

Inside the function, `kwargs` is a dictionary.

```python
def show_details(**details):

    print(details)
    print(type(details))

show_details(
    name="Arun",
    city="Chennai",
    department="Data Engineering"
)
```

Output:

```
{
    'name': 'Arun',
    'city': 'Chennai',
    'department': 'Data Engineering'
}
<class 'dict'>
```

## 33. \*args and \*\*kwargs Together

```python
def example(*args, **kwargs):

    print("Positional:", args)
    print("Keyword:", kwargs)

example(
    10,
    20,
    30,
    name="Arun",
    city="Chennai"
)
```

Output:

```
Positional: (10, 20, 30)
Keyword: {'name': 'Arun', 'city': 'Chennai'}
```

## 34. Function with List Parameter

Functions can receive collections.

```python
def calculate_total(numbers):

    return sum(numbers)

numbers = [10, 20, 30, 40]

result = calculate_total(numbers)

print(result)
```

Output:

```
100
```

## 35. Function with Dictionary Parameter

```python
def display_customer(customer):

    print("ID:", customer["id"])
    print("Name:", customer["name"])
    print("City:", customer["city"])

customer = {
    "id": 1001,
    "name": "Arun",
    "city": "Chennai"
}

display_customer(customer)
```

Output:

```
ID: 1001
Name: Arun
City: Chennai
```

## 36. Function Returning a List

```python
def get_even_numbers(numbers):

    return [
        number
        for number in numbers
        if number % 2 == 0
    ]

numbers = [1, 2, 3, 4, 5, 6]

result = get_even_numbers(numbers)

print(result)
```

Output:

```
[2, 4, 6]
```

## 37. Function Returning a Dictionary

```python
def create_customer(customer_id, name, city):

    return {
        "id": customer_id,
        "name": name,
        "city": city
    }

customer = create_customer(
    1001,
    "Arun",
    "Chennai"
)

print(customer)
```

Output:

```
{
    'id': 1001,
    'name': 'Arun',
    'city': 'Chennai'
}
```

## 38. Scope of Variables

### Local Variable

A variable created inside a function is generally local to that function.

```python
def calculate():

    total = 100

    print(total)

calculate()
```

This works.

But:

```python
print(total)
```

outside the function causes an error because `total` is local to the function.

## 39. Global Variable

A variable defined outside a function is global to that module.

```python
company = "ABC Technologies"

def display_company():

    print(company)

display_company()
```

Output:

```
ABC Technologies
```

## 40. Local vs Global

```python
company = "ABC"

def show():

    department = "Data Engineering"

    print(company)
    print(department)

show()
```

Here:

```
company
→ Global variable

department
→ Local variable
```

## 41. Modifying Global Variables

Use the `global` keyword when you intentionally need to rebind a global variable inside a function.

```python
count = 0

def increment():

    global count

    count += 1

increment()

print(count)
```

Output:

```
1
```

In general, avoid excessive use of global mutable state. Passing values into functions and returning results usually makes code easier to test and maintain.

## 42. Function Documentation

A function can have a docstring that explains its purpose.

```python
def calculate_total(numbers):
    """
    Calculate the total of all numbers.
    """

    return sum(numbers)
```

We can inspect the documentation:

```python
print(calculate_total.__doc__)
```

Output:

```
Calculate the total of all numbers.
```

## 43. Type Hints

Type hints describe the expected input and output types.

```python
def add(a: int, b: int) -> int:

    return a + b
```

Calling:

```python
result = add(10, 20)

print(result)
```

Type hints improve readability and tooling, but Python does not automatically enforce them at runtime.

## 44. Function with Type Hints and Docstring

```python
def calculate_total(
    numbers: list[int]
) -> int:
    """
    Calculate the total of a list of integers.
    """

    return sum(numbers)


numbers = [10, 20, 30, 40]

print(calculate_total(numbers))
```

Output:

```
100
```

## 45. Lambda Functions

### Definition

A lambda is a small anonymous function written as an expression.

### Syntax

```python
lambda arguments: expression
```

Example:

```python
square = lambda number: number ** 2

print(square(5))
```

Output:

```
25
```

For simple operations, however, a normal `def` function is often clearer.

## 46. Lambda with sorted()

```python
employees = [
    {"name": "Arun", "salary": 50000},
    {"name": "Rahul", "salary": 70000},
    {"name": "Priya", "salary": 60000}
]

sorted_employees = sorted(
    employees,
    key=lambda employee: employee["salary"]
)

print(sorted_employees)
```

Output:

```
[
    {'name': 'Arun', 'salary': 50000},
    {'name': 'Priya', 'salary': 60000},
    {'name': 'Rahul', 'salary': 70000}
]
```

This is a common pattern when sorting structured data.

## 47. Functions Calling Other Functions

Functions can call other functions.

```python
def clean_name(name):

    return name.strip().title()


def create_customer(name, city):

    return {
        "name": clean_name(name),
        "city": city.strip().title()
    }


customer = create_customer(
    "  arun ",
    " chennai "
)

print(customer)
```

Output:

```
{'name': 'Arun', 'city': 'Chennai'}
```

This allows us to break a larger process into smaller reusable functions.

## 48. Data Engineering Example - Data Cleaning Function

```python
def clean_customer(customer):

    return {
        "id": customer["id"],
        "name": customer["name"].strip().title(),
        "city": customer["city"].strip().title(),
        "email": customer["email"].strip().lower()
    }


customer = {
    "id": 1001,
    "name": "  ARUN KUMAR ",
    "city": " CHENNAI ",
    "email": " ARUN@GMAIL.COM "
}

cleaned_customer = clean_customer(customer)

print(cleaned_customer)
```

Output:

```
{
    'id': 1001,
    'name': 'Arun Kumar',
    'city': 'Chennai',
    'email': 'arun@gmail.com'
}
```

This is similar to a transformation function that might be used in an ETL pipeline.

## 49. Data Engineering Example - Validate Record

```python
def validate_customer(customer):

    required_fields = [
        "id",
        "name",
        "email"
    ]

    for field in required_fields:

        if field not in customer:
            return False

    return True


customer = {
    "id": 1001,
    "name": "Arun",
    "email": "arun@gmail.com"
}

print(validate_customer(customer))
```

Output:

```
True
```

Another record:

```python
customer = {
    "id": 1002,
    "name": "Rahul"
}

print(validate_customer(customer))
```

Output:

```
False
```

## 50. Function Composition

We can create a pipeline using multiple functions.

```python
def clean_name(name):

    return name.strip().title()


def clean_email(email):

    return email.strip().lower()


def create_customer(customer_id, name, email):

    return {
        "id": customer_id,
        "name": clean_name(name),
        "email": clean_email(email)
    }


customer = create_customer(
    1001,
    "  ARUN KUMAR ",
    " ARUN@GMAIL.COM "
)

print(customer)
```

Output:

```
{
    'id': 1001,
    'name': 'Arun Kumar',
    'email': 'arun@gmail.com'
}
```

The flow is:

```
Raw Data
   ↓
clean_name()
   ↓
clean_email()
   ↓
create_customer()
   ↓
Clean Record
```

## 51. Functions as Reusable ETL Components

A simple ETL-style workflow can be represented using functions:

```python
def extract_data():

    return [
        "1001,Arun,Chennai",
        "1002,Rahul,Mumbai",
        "1003,Priya,Delhi"
    ]


def transform_data(records):

    transformed = []

    for record in records:

        fields = record.split(",")

        customer = {
            "id": int(fields[0]),
            "name": fields[1].strip(),
            "city": fields[2].strip()
        }

        transformed.append(customer)

    return transformed


def load_data(records):

    for record in records:
        print("Loading:", record)


raw_data = extract_data()

clean_data = transform_data(raw_data)

load_data(clean_data)
```

Output:

```
Loading: {'id': 1001, 'name': 'Arun', 'city': 'Chennai'}
Loading: {'id': 1002, 'name': 'Rahul', 'city': 'Mumbai'}
Loading: {'id': 1003, 'name': 'Priya', 'city': 'Delhi'}
```

This illustrates an important Data Engineering concept:

```
Extract
   ↓
Transform
   ↓
Load
```

Each stage is separated into a reusable function.

## 52. Function Execution Flow

Consider:

```python
def calculate_total(numbers):

    total = 0

    for number in numbers:
        total += number

    return total


numbers = [10, 20, 30]

result = calculate_total(numbers)

print(result)
```

Execution flow:

```
Program starts
      ↓
Function is defined
      ↓
numbers is created
      ↓
calculate_total(numbers)
      ↓
Function starts
      ↓
total = 0
      ↓
Loop processes values
      ↓
return total
      ↓
result receives returned value
      ↓
print(result)
```

## 53. Common Function Mistakes

### Mistake 1 - Forgetting to Call the Function

```python
def greet():
    print("Hello")
```

Nothing is printed because the function was only defined.

Correct:

```python
greet()
```

### Mistake 2 - Missing Required Argument

```python
def greet(name):

    print(name)


greet()
```

This causes a `TypeError` because `name` is required.

### Mistake 3 - Confusing print() and return

```python
def add(a, b):

    print(a + b)

result = add(10, 20)

print(result)
```

Output:

```
30
None
```

If the result needs to be reused, use:

```python
def add(a, b):

    return a + b
```

## 54. Function Best Practices

A good function should generally:

```
Have one clear responsibility
Have a meaningful name
Accept required inputs as parameters
Return useful results
Avoid unnecessary global state
Be easy to test
Be easy to reuse
```

Instead of:

```python
def process():
    # 200 lines of unrelated logic
    ...
```

prefer smaller functions:

```python
def extract_data():
    ...


def clean_data():
    ...


def validate_data():
    ...


def load_data():
    ...
```

## 55. Complete Data Engineering Example

```python
def extract_records():

    return [
        "1001,  Arun Kumar , CHENNAI , ARUN@GMAIL.COM",
        "1002, Rahul , MUMBAI , RAHUL@GMAIL.COM",
        "1003,  Priya , DELHI , PRIYA@GMAIL.COM"
    ]


def transform_record(record):

    fields = record.split(",")

    return {
        "id": int(fields[0].strip()),
        "name": fields[1].strip().title(),
        "city": fields[2].strip().title(),
        "email": fields[3].strip().lower()
    }


def transform_data(records):

    return [
        transform_record(record)
        for record in records
    ]


def validate_record(record):

    required_fields = [
        "id",
        "name",
        "city",
        "email"
    ]

    return all(
        field in record
        for field in required_fields
    )


def load_data(records):

    for record in records:

        if validate_record(record):
            print("Loading:", record)
        else:
            print("Invalid:", record)


raw_data = extract_records()

transformed_data = transform_data(raw_data)

load_data(transformed_data)
```

Output:

```
Loading: {'id': 1001, 'name': 'Arun Kumar', 'city': 'Chennai', 'email': 'arun@gmail.com'}
Loading: {'id': 1002, 'name': 'Rahul', 'city': 'Mumbai', 'email': 'rahul@gmail.com'}
Loading: {'id': 1003, 'name': 'Priya', 'city': 'Delhi', 'email': 'priya@gmail.com'}
```

This example combines:

```
Functions
Parameters
Return values
List comprehension
Dictionary creation
String methods
Data cleaning
Validation
ETL concepts
```

## 56. Function Practice Problems

### Problem 1 - Calculate Statistics

Create a function:

```python
calculate_statistics(numbers)
```

The function should return:

```
Minimum
Maximum
Total
Average
```

Example:

```python
numbers = [10, 20, 30, 40, 50]
```

Expected result:

```
Minimum: 10
Maximum: 50
Total: 150
Average: 30.0
```

## Problem 2 - Clean Names

Create a function:

```python
clean_names(names)
```

Input:

```python
names = [
    "  Arun ",
    " RAHUL",
    "Priya ",
    "  vijay  "
]
```

Expected output:

```
['Arun', 'Rahul', 'Priya', 'Vijay']
```

Use:

```python
strip()
title()
```

## Problem 3 - Filter High-Value Transactions

Create:

```python
filter_transactions(transactions, threshold)
```

Input:

```python
transactions = [500, 1500, 2500, 800, 4000]
```

Call:

```python
filter_transactions(transactions, 1000)
```

Expected:

```
[1500, 2500, 4000]
```

## Problem 4 - Create Customer Dictionary

Create:

```python
create_customer(customer_id, name, city, email)
```

The function should return:

```python
{
    "id": customer_id,
    "name": name,
    "city": city,
    "email": email
}
```

Also clean:

```
name → strip + title
city → strip + title
email → strip + lower
```

## Problem 5 - Validate Customer

Create:

```python
validate_customer(customer)
```

A customer is valid only if it contains:

```
id
name
email
city
```

Return:

```
True
```

if all fields exist.

Otherwise:

```
False
```

## Problem 6 - Function with \*args

Create:

```python
calculate_total(*numbers)
```

The function should accept any number of numbers and return their total.

Examples:

```python
calculate_total(10, 20)
```

Expected:

```
30
```

```python
calculate_total(10, 20, 30, 40)
```

Expected:

```
100
```

## Problem 7 - Function with \*\*kwargs

Create:

```python
create_employee(**details)
```

Call:

```python
create_employee(
    id=1001,
    name="Arun",
    department="Data Engineering",
    salary=60000
)
```

The function should return a dictionary containing all the supplied details.

## Problem 8 - Mini ETL Pipeline

Create four functions:

```python
extract_data()
transform_data()
validate_data()
load_data()
```

Input records:

```
1001, Arun , Chennai
1002, Rahul , Mumbai
1003, Priya , Delhi
```

Expected pipeline:

```
Extract
   ↓
Transform
   ↓
Validate
   ↓
Load
```

The final records should look like:

```python
[
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

## 57. Key Takeaways - Dictionary Comprehension

1. Dictionary comprehension provides a concise way to create dictionaries.
2. Basic syntax:

```python
{key: value for item in iterable}
```

3. Conditions can be added:

```python
{key: value for item in iterable if condition}
```

4. `if-else` can be used for conditional transformation.
5. Dictionary comprehensions are useful for filtering and transforming existing dictionaries.
6. `items()` is commonly used when iterating over dictionaries.
7. `zip()` can be used to create dictionaries from two lists.
8. Dictionary comprehensions are useful for creating lookup structures.
9. They are especially useful when transforming structured data in memory.

## 58. Key Takeaways - Functions

1. A function is a reusable block of code.
2. Functions are created using `def`.
3. Functions can accept parameters.
4. Arguments are the actual values passed to parameters.
5. `return` sends a result back to the caller.
6. `print()` displays a result but does not return it.
7. Functions can have multiple parameters.
8. Parameters can have default values.
9. Positional arguments are matched by position.
10. Keyword arguments are matched by parameter name.
11. `*args` accepts variable-length positional arguments.
12. `**kwargs` accepts variable-length keyword arguments.
13. `*args` is stored as a tuple.
14. `**kwargs` is stored as a dictionary.
15. Functions can accept and return lists, tuples, sets, and dictionaries.
16. Variables created inside functions are generally local.
17. Global variables exist outside function scope.
18. Docstrings document what a function does.
19. Type hints improve readability and tooling.
20. Lambda functions are useful for small expressions.
21. Functions can call other functions.
22. Multiple functions can be combined to create an ETL workflow.
23. Good functions should have a clear and focused responsibility.
24. Functions are one of the most important concepts for writing reusable Data Engineering code.

## Lambda, Recursion, Variable-Length Arguments, `__init__()` and Parameterized Constructor

## Part 1 - Lambda Functions

### 1. What is a Lambda Function?

#### Definition

A lambda function is a small anonymous function in Python.

Anonymous means that the function can be created without defining it using the normal `def` syntax.

A lambda function can:

* Accept any number of arguments
* Contain only one expression
* Automatically return the result of that expression

### Syntax

```python
lambda arguments: expression
```

Example:

```python
square = lambda number: number ** 2

print(square(5))
```

Output:

```
25
```

***

## 2. Real-World Analogy

Think of a normal function as a permanent machine in a factory.

You design it, give it a name, and reuse it many times.

```python
def square(number):
    return number ** 2
```

A lambda is more like a small temporary tool created for a quick operation.

```python
lambda number: number ** 2
```

In Data Engineering, lambdas are commonly useful when we need a small function for operations such as:

```
Sorting
Filtering
Mapping
Simple transformations
```

***

## 3. Normal Function vs Lambda

Normal function:

```python
def square(number):
    return number ** 2

print(square(5))
```

Lambda:

```python
square = lambda number: number ** 2

print(square(5))
```

Both produce:

```
25
```

***

## 4. Lambda with One Argument

```python
double = lambda number: number * 2

print(double(10))
```

Output:

```
20
```

Another example:

```python
upper_name = lambda name: name.upper()

print(upper_name("arun"))
```

Output:

```
ARUN
```

***

## 5. Lambda with Multiple Arguments

A lambda can accept multiple arguments.

```python
add = lambda a, b: a + b

print(add(10, 20))
```

Output:

```
30
```

Another example:

```python
multiply = lambda a, b, c: a * b * c

print(multiply(2, 3, 4))
```

Output:

```
24
```

***

## 6. Lambda with Conditional Expression

A lambda can contain a conditional expression.

```python
check_even = lambda number: "Even" if number % 2 == 0 else "Odd"

print(check_even(10))
print(check_even(7))
```

Output:

```
Even
Odd
```

Another example:

```python
status = lambda amount: "Valid" if amount > 0 else "Invalid"

print(status(500))
print(status(0))
```

Output:

```
Valid
Invalid
```

***

## 7. Lambda with sorted()

One of the most useful applications of lambda functions is providing a sorting key.

Suppose we have:

```python
employees = [
    {"name": "Arun", "salary": 50000},
    {"name": "Rahul", "salary": 70000},
    {"name": "Priya", "salary": 60000}
]
```

Sort based on salary:

```python
result = sorted(
    employees,
    key=lambda employee: employee["salary"]
)

print(result)
```

Output:

```
[
    {'name': 'Arun', 'salary': 50000},
    {'name': 'Priya', 'salary': 60000},
    {'name': 'Rahul', 'salary': 70000}
]
```

Descending order:

```python
result = sorted(
    employees,
    key=lambda employee: employee["salary"],
    reverse=True
)

print(result)
```

***

## 8. Lambda with map()

### map()

`map()` applies a function to every element of an iterable.

Syntax:

```python
map(function, iterable)
```

Example:

```python
numbers = [1, 2, 3, 4, 5]

result = map(
    lambda number: number ** 2,
    numbers
)

print(list(result))
```

Output:

```
[1, 4, 9, 16, 25]
```

Conceptually:

```
1 → square → 1
2 → square → 4
3 → square → 9
4 → square → 16
5 → square → 25
```

***

## 9. Lambda with filter()

### filter()

`filter()` keeps elements for which a function returns `True`.

Syntax:

```python
filter(function, iterable)
```

Example:

```python
numbers = [1, 2, 3, 4, 5, 6]

result = filter(
    lambda number: number % 2 == 0,
    numbers
)

print(list(result))
```

Output:

```
[2, 4, 6]
```

***

## 10. Data Engineering Example - Cleaning Names

```python
names = [
    "  Arun ",
    " RAHUL ",
    " priya"
]

cleaned_names = list(
    map(
        lambda name: name.strip().title(),
        names
    )
)

print(cleaned_names)
```

Output:

```
['Arun', 'Rahul', 'Priya']
```

***

## 11. Data Engineering Example - Filtering Files

```python
files = [
    "customers.csv",
    "orders.json",
    "products.csv",
    "sales.txt"
]

csv_files = list(
    filter(
        lambda file: file.endswith(".csv"),
        files
    )
)

print(csv_files)
```

Output:

```
['customers.csv', 'products.csv']
```

***

## 12. Lambda vs List Comprehension

Using `map()` and lambda:

```python
numbers = [1, 2, 3, 4]

result = list(
    map(
        lambda number: number ** 2,
        numbers
    )
)

print(result)
```

Using list comprehension:

```python
numbers = [1, 2, 3, 4]

result = [
    number ** 2
    for number in numbers
]

print(result)
```

Both produce:

```
[1, 4, 9, 16]
```

For many simple transformations and filters, list comprehensions are often easier to read.

Lambda is particularly useful when a function needs another small function as an argument.

Example:

```python
sorted(
    employees,
    key=lambda employee: employee["salary"]
)
```

***

## Part 2 - Recursion

## 13. What is Recursion?

### Definition

Recursion is a programming technique where a function calls itself to solve a smaller version of the same problem.

A recursive function normally contains:

```
Base Case
Recursive Case
```

The base case stops the recursion.

The recursive case calls the function again with a smaller or simpler input.

***

## 14. Real-World Analogy

Imagine opening nested folders:

```
Main Folder
    |
    └── Folder A
            |
            └── Folder B
                    |
                    └── Folder C
```

To search all folders:

```
Open folder
Check files
Find another folder
Open that folder
Repeat
```

This is naturally recursive because a folder can contain another folder with the same structure.

Recursive ideas appear in Data Engineering when dealing with:

```
Nested JSON
Directory structures
Hierarchical data
Trees
Parent-child relationships
```

***

## 15. Simple Recursion Example

```python
def countdown(number):

    if number == 0:
        print("Completed")
        return

    print(number)

    countdown(number - 1)


countdown(5)
```

Output:

```
5
4
3
2
1
Completed
```

Execution:

```
countdown(5)
    ↓
countdown(4)
    ↓
countdown(3)
    ↓
countdown(2)
    ↓
countdown(1)
    ↓
countdown(0)
    ↓
STOP
```

***

## 16. Base Case

The base case tells Python when the recursive calls should stop.

Example:

```python
if number == 0:
    return
```

Without a proper base case, the function may keep calling itself until Python raises:

```
RecursionError
```

***

## 17. Recursive Case

The recursive case is where the function calls itself.

```python
countdown(number - 1)
```

Each call should normally move the problem closer to the base case.

***

## 18. Factorial Using Recursion

Factorial of `5`:

```
5! = 5 × 4 × 3 × 2 × 1

5! = 120
```

Recursive definition:

```
factorial(5)
=
5 × factorial(4)

factorial(4)
=
4 × factorial(3)

factorial(3)
=
3 × factorial(2)

factorial(2)
=
2 × factorial(1)

factorial(1)
=
1
```

Code:

```python
def factorial(number):

    if number <= 1:
        return 1

    return number * factorial(number - 1)


print(factorial(5))
```

Output:

```
120
```

***

## 19. Understanding Recursive Return

Consider:

```python
factorial(4)
```

Calls:

```
factorial(4)
    ↓
4 × factorial(3)

factorial(3)
    ↓
3 × factorial(2)

factorial(2)
    ↓
2 × factorial(1)

factorial(1)
    ↓
1
```

Now the results return upward:

```
factorial(1) = 1

factorial(2)
= 2 × 1
= 2

factorial(3)
= 3 × 2
= 6

factorial(4)
= 4 × 6
= 24
```

***

## 20. Sum of Numbers Using Recursion

Calculate:

```
1 + 2 + 3 + 4 + 5
```

Code:

```python
def calculate_sum(number):

    if number == 0:
        return 0

    return number + calculate_sum(number - 1)


print(calculate_sum(5))
```

Output:

```
15
```

***

## 21. Recursive List Sum

```python
def list_sum(numbers):

    if len(numbers) == 0:
        return 0

    return numbers[0] + list_sum(numbers[1:])


numbers = [10, 20, 30, 40]

print(list_sum(numbers))
```

Output:

```
100
```

***

## 22. Data Engineering Example - Nested Dictionary

Consider nested JSON-like data:

```python
data = {
    "customer": {
        "id": 1001,
        "name": "Arun",
        "address": {
            "city": "Chennai",
            "state": "Tamil Nadu"
        }
    }
}
```

We can recursively print all values.

```python
def print_nested(data):

    for key, value in data.items():

        if isinstance(value, dict):
            print_nested(value)

        else:
            print(f"{key}: {value}")


print_nested(data)
```

Output:

```
id: 1001
name: Arun
city: Chennai
state: Tamil Nadu
```

This demonstrates why recursion can be useful when the depth of nested data is not fixed.

***

## 23. Recursion vs Loop

Loop:

```python
for number in range(5, 0, -1):
    print(number)
```

Recursion:

```python
def countdown(number):

    if number == 0:
        return

    print(number)

    countdown(number - 1)
```

For simple repetition, loops are generally preferable.

Recursion becomes useful when the data or problem itself is naturally recursive, such as:

```
Trees
Nested structures
Directories
Hierarchical relationships
```

***

## Part 3 - Variable-Length Arguments

## 24. What are Variable-Length Arguments?

Sometimes we do not know how many arguments will be passed to a function.

For example:

```python
calculate_total(10, 20)
```

or:

```python
calculate_total(10, 20, 30, 40, 50)
```

Python provides two major mechanisms:

```
*args
**kwargs
```

They allow functions to accept a variable number of arguments.

***

## 25. \*args

### Definition

`*args` allows a function to accept any number of positional arguments.

### Syntax

```python
def function_name(*args):
    ...
```

Example:

```python
def display_numbers(*numbers):

    print(numbers)


display_numbers(10, 20, 30, 40)
```

Output:

```
(10, 20, 30, 40)
```

Inside the function, the values are stored as a:

```
Tuple
```

***

## 26. \*args Example - Sum

```python
def calculate_total(*numbers):

    total = 0

    for number in numbers:
        total += number

    return total


print(calculate_total(10, 20))
print(calculate_total(10, 20, 30))
print(calculate_total(10, 20, 30, 40))
```

Output:

```
30
60
100
```

***

## 27. \*args with Normal Parameters

Normal parameters can be combined with `*args`.

```python
def calculate_salary(employee_name, *monthly_salary):

    print("Employee:", employee_name)
    print("Salary Values:", monthly_salary)


calculate_salary(
    "Arun",
    50000,
    52000,
    55000
)
```

Output:

```
Employee: Arun
Salary Values: (50000, 52000, 55000)
```

***

## 28. Unpacking a List with \*

Suppose we have:

```python
numbers = [10, 20, 30]
```

And:

```python
def add(a, b, c):

    return a + b + c
```

Instead of:

```python
add(
    numbers[0],
    numbers[1],
    numbers[2]
)
```

we can unpack the list:

```python
numbers = [10, 20, 30]

result = add(*numbers)

print(result)
```

Output:

```
60
```

The `*` operator unpacks:

```
[10, 20, 30]

into

10, 20, 30
```

***

## 29. \*\*kwargs

### Definition

`**kwargs` allows a function to accept any number of keyword arguments.

Example:

```python
def customer_details(**details):

    print(details)


customer_details(
    id=1001,
    name="Arun",
    city="Chennai"
)
```

Output:

```
{
    'id': 1001,
    'name': 'Arun',
    'city': 'Chennai'
}
```

Inside the function, `details` is a:

```
Dictionary
```

***

## 30. Traversing \*\*kwargs

```python
def customer_details(**details):

    for key, value in details.items():
        print(f"{key}: {value}")


customer_details(
    id=1001,
    name="Arun",
    city="Chennai",
    department="Data Engineering"
)
```

Output:

```
id: 1001
name: Arun
city: Chennai
department: Data Engineering
```

***

## 31. Unpacking Dictionary with \*\*

Suppose we have:

```python
customer = {
    "name": "Arun",
    "city": "Chennai"
}
```

And:

```python
def display_customer(name, city):

    print(name)
    print(city)
```

We can unpack the dictionary:

```python
customer = {
    "name": "Arun",
    "city": "Chennai"
}

display_customer(**customer)
```

Output:

```
Arun
Chennai
```

The dictionary keys must match the function parameter names.

***

## 32. \*args and \*\*kwargs Together

```python
def process_data(*args, **kwargs):

    print("Positional Arguments:")
    print(args)

    print("Keyword Arguments:")
    print(kwargs)


process_data(
    10,
    20,
    30,
    source="API",
    format="JSON"
)
```

Output:

```
Positional Arguments:
(10, 20, 30)

Keyword Arguments:
{'source': 'API', 'format': 'JSON'}
```

***

## 33. Important Difference

```
*args
↓
Variable number of positional arguments
↓
Stored as tuple


**kwargs
↓
Variable number of keyword arguments
↓
Stored as dictionary
```

Example:

```python
def example(*args, **kwargs):

    print(type(args))
    print(type(kwargs))


example(
    10,
    20,
    name="Arun"
)
```

Output:

```
<class 'tuple'>
<class 'dict'>
```

***

## 34. Important Naming Point

The names `args` and `kwargs` are conventions.

This works:

```python
def calculate(*numbers):

    print(numbers)
```

And this also works:

```python
def details(**customer):

    print(customer)
```

The important part is:

```
*
**
```

However, using:

```python
*args
**kwargs
```

is the standard Python convention.

***

## 35. Data Engineering Example - Flexible Configuration

```python
def pipeline_config(**config):

    print("Pipeline Configuration")

    for key, value in config.items():
        print(f"{key}: {value}")


pipeline_config(
    source="MySQL",
    destination="Data Warehouse",
    batch_size=10000,
    retries=3
)
```

Output:

```
Pipeline Configuration
source: MySQL
destination: Data Warehouse
batch_size: 10000
retries: 3
```

This type of flexible configuration pattern can be useful when building reusable functions.

***

## Part 4 - Introduction to Classes and Objects

Before understanding `__init__()`, we need a basic understanding of classes and objects.

## 36. What is a Class?

### Definition

A class is a blueprint used to create objects.

Example:

```
Class
↓
Blueprint

Object
↓
Actual instance created using the blueprint
```

### Real-World Analogy

Think of a house blueprint.

The blueprint defines:

```
Number of rooms
Number of doors
Number of windows
Design
```

From the same blueprint, we can construct multiple houses.

Similarly:

```
Class
    ↓
Object 1
Object 2
Object 3
```

***

## 37. Creating a Basic Class

```python
class Customer:

    pass
```

Create an object:

```python
customer1 = Customer()

print(customer1)
```

`customer1` is an object of the `Customer` class.

***

## Part 5 - `__init__()`

## 38. What is `__init__()`?

### Definition

`__init__()` is a special method that Python automatically calls after a new object has been created.

It is commonly used to initialize the object's attributes.

Example:

```python
class Customer:

    def __init__(self):

        print("Customer object initialized")


customer1 = Customer()
```

Output:

```
Customer object initialized
```

We did not manually call:

```python
customer1.__init__()
```

Python invoked `__init__()` as part of creating the object.

***

## 39. Understanding self

`self` refers to the current object.

Example:

```python
class Customer:

    def __init__(self):

        self.name = "Arun"
```

Create object:

```python
customer1 = Customer()

print(customer1.name)
```

Output:

```
Arun
```

Here:

```
self.name
```

means:

```
name belonging to this object
```

***

## 40. Why self is Required

Consider:

```python
class Customer:

    def __init__(self):

        self.name = "Arun"
        self.city = "Chennai"
```

When:

```python
customer1 = Customer()
```

Python effectively associates:

```
self → customer1
```

Therefore:

```python
self.name = "Arun"
```

creates an attribute on that object.

Conceptually:

```
customer1.name = "Arun"
```

***

## 41. Instance Attributes

Attributes created using `self` belong to the object.

```python
class Customer:

    def __init__(self):

        self.name = "Arun"
        self.city = "Chennai"


customer1 = Customer()

print(customer1.name)
print(customer1.city)
```

Output:

```
Arun
Chennai
```

***

## 42. Multiple Objects

```python
class Customer:

    def __init__(self):

        self.name = "Unknown"


customer1 = Customer()
customer2 = Customer()

customer1.name = "Arun"
customer2.name = "Rahul"

print(customer1.name)
print(customer2.name)
```

Output:

```
Arun
Rahul
```

Each object maintains its own instance data.

***

## Part 6 - Parameterized Constructor

## 43. What is a Parameterized Constructor?

In beginner Python terminology, a parameterized constructor usually refers to an `__init__()` method that accepts values used to initialize an object.

Example:

```python
class Customer:

    def __init__(self, name):

        self.name = name
```

Now the value must be supplied while creating the object.

```python
customer1 = Customer("Arun")

print(customer1.name)
```

Output:

```
Arun
```

Technically, Python creates the object through `__new__()` and then initializes it through `__init__()`. In most beginner courses, however, `__init__()` is commonly discussed as the constructor.

***

## 44. Parameterized Constructor with Multiple Parameters

```python
class Customer:

    def __init__(
        self,
        customer_id,
        name,
        city
    ):

        self.customer_id = customer_id
        self.name = name
        self.city = city
```

Create objects:

```python
customer1 = Customer(
    1001,
    "Arun",
    "Chennai"
)

customer2 = Customer(
    1002,
    "Rahul",
    "Mumbai"
)
```

Access attributes:

```python
print(customer1.customer_id)
print(customer1.name)
print(customer1.city)

print(customer2.customer_id)
print(customer2.name)
print(customer2.city)
```

Output:

```
1001
Arun
Chennai
1002
Rahul
Mumbai
```

***

## 45. Understanding the Flow

When we write:

```python
customer1 = Customer(
    1001,
    "Arun",
    "Chennai"
)
```

The values correspond to:

```
customer_id → 1001
name        → Arun
city        → Chennai
```

Inside:

```python
def __init__(
    self,
    customer_id,
    name,
    city
):
```

Python associates:

```
self        → customer1
customer_id → 1001
name        → Arun
city        → Chennai
```

Then:

```python
self.customer_id = customer_id
self.name = name
self.city = city
```

creates attributes on `customer1`.

***

## 46. Parameter vs Instance Attribute

Consider:

```python
class Customer:

    def __init__(self, name):

        self.name = name
```

There are two different things:

```
name
↓
Parameter

self.name
↓
Instance attribute
```

The assignment:

```python
self.name = name
```

means:

```
Take the value from the parameter "name"

and

Store it in the object's attribute "name"
```

***

## 47. Parameterized Constructor with Default Value

We can also provide default values.

```python
class Customer:

    def __init__(
        self,
        name,
        city="Chennai"
    ):

        self.name = name
        self.city = city
```

Create object:

```python
customer1 = Customer("Arun")

print(customer1.name)
print(customer1.city)
```

Output:

```
Arun
Chennai
```

Override the default:

```python
customer2 = Customer(
    "Rahul",
    "Mumbai"
)

print(customer2.city)
```

Output:

```
Mumbai
```

***

## 48. Adding Methods to a Class

Classes can contain methods in addition to `__init__()`.

```python
class Customer:

    def __init__(
        self,
        customer_id,
        name,
        city
    ):

        self.customer_id = customer_id
        self.name = name
        self.city = city


    def display(self):

        print("Customer ID:", self.customer_id)
        print("Name:", self.name)
        print("City:", self.city)
```

Create object:

```python
customer1 = Customer(
    1001,
    "Arun",
    "Chennai"
)

customer1.display()
```

Output:

```
Customer ID: 1001
Name: Arun
City: Chennai
```

***

## 49. Data Engineering Example - Pipeline Class

Suppose we want to represent a data pipeline.

```python
class DataPipeline:

    def __init__(
        self,
        pipeline_name,
        source,
        destination
    ):

        self.pipeline_name = pipeline_name
        self.source = source
        self.destination = destination


    def display_pipeline(self):

        print("Pipeline:", self.pipeline_name)
        print("Source:", self.source)
        print("Destination:", self.destination)
```

Create pipeline:

```python
pipeline1 = DataPipeline(
    "Customer ETL",
    "MySQL",
    "Data Warehouse"
)

pipeline1.display_pipeline()
```

Output:

```
Pipeline: Customer ETL
Source: MySQL
Destination: Data Warehouse
```

Create another pipeline:

```python
pipeline2 = DataPipeline(
    "Orders ETL",
    "API",
    "Data Lake"
)

pipeline2.display_pipeline()
```

Output:

```
Pipeline: Orders ETL
Source: API
Destination: Data Lake
```

The same class can therefore represent multiple pipelines with different configurations.

***

## 50. Data Engineering Example - File Processor

```python
class FileProcessor:

    def __init__(
        self,
        file_name,
        file_type
    ):

        self.file_name = file_name
        self.file_type = file_type


    def validate(self):

        if self.file_name.endswith(
            "." + self.file_type
        ):
            return True

        return False
```

Create object:

```python
file1 = FileProcessor(
    "customers.csv",
    "csv"
)

print(file1.validate())
```

Output:

```
True
```

Another object:

```python
file2 = FileProcessor(
    "orders.json",
    "csv"
)

print(file2.validate())
```

Output:

```
False
```

***

## 51. Lambda Practice Problems

### Problem 1 - Square Number

Create a lambda function that returns the square of a number.

Input:

```python
5
```

Expected:

```
25
```

***

## Problem 2 - Largest Number

Create a lambda function that accepts two numbers and returns the larger number.

Example:

```python
largest(10, 20)
```

Expected:

```
20
```

***

## Problem 3 - Filter Even Numbers

Given:

```python
numbers = [
    10,
    15,
    20,
    25,
    30,
    35
]
```

Use:

```
lambda
filter()
```

to create:

```
[10, 20, 30]
```

***

## Problem 4 - Clean Data

Given:

```python
names = [
    "  arun ",
    " RAHUL ",
    " priya "
]
```

Use:

```
lambda
map()
```

to produce:

```
['Arun', 'Rahul', 'Priya']
```

***

## Problem 5 - Sort Employees

Given:

```python
employees = [
    {"name": "Arun", "salary": 50000},
    {"name": "Rahul", "salary": 70000},
    {"name": "Priya", "salary": 60000}
]
```

Sort the employees based on salary from highest to lowest using:

```
sorted()
lambda
```

Expected order:

```
Rahul
Priya
Arun
```

***

## 52. Recursion Practice Problems

### Problem 6 - Countdown

Create a recursive function:

```python
countdown(number)
```

Input:

```python
countdown(5)
```

Expected:

```
5
4
3
2
1
Completed
```

***

## Problem 7 - Factorial

Create:

```python
factorial(number)
```

Input:

```python
factorial(5)
```

Expected:

```
120
```

***

## Problem 8 - Recursive Sum

Create:

```python
calculate_sum(number)
```

Input:

```python
calculate_sum(10)
```

The function should calculate:

```
10 + 9 + 8 + ... + 1
```

Expected:

```
55
```

***

## Problem 9 - Nested Dictionary

Given:

```python
data = {
    "customer": {
        "id": 1001,
        "details": {
            "name": "Arun",
            "address": {
                "city": "Chennai",
                "state": "Tamil Nadu"
            }
        }
    }
}
```

Create a recursive function to print all final key-value pairs.

Expected:

```
id: 1001
name: Arun
city: Chennai
state: Tamil Nadu
```

***

## 53. Variable-Length Arguments Practice Problems

### Problem 10 - Total Amount

Create:

```python
calculate_total(*amounts)
```

Example:

```python
calculate_total(
    1000,
    2500,
    3000,
    1500
)
```

Expected:

```
8000
```

***

## Problem 11 - Maximum Value

Create:

```python
find_maximum(*numbers)
```

The function should accept any number of values and return the maximum.

Example:

```python
find_maximum(
    10,
    50,
    20,
    80,
    40
)
```

Expected:

```
80
```

***

## Problem 12 - Pipeline Configuration

Create:

```python
pipeline_config(**config)
```

Call:

```python
pipeline_config(
    source="PostgreSQL",
    destination="Data Lake",
    format="Parquet",
    batch_size=5000
)
```

Print every configuration as:

```
source: PostgreSQL
destination: Data Lake
format: Parquet
batch_size: 5000
```

***

## 54. `__init__()` Practice Problems

### Problem 13 - Employee Class

Create:

```python
class Employee:
```

The parameterized `__init__()` should receive:

```
employee_id
name
department
salary
```

Create:

```python
employee1 = Employee(
    1001,
    "Arun",
    "Data Engineering",
    60000
)
```

Create a method:

```python
display()
```

Expected:

```
Employee ID: 1001
Name: Arun
Department: Data Engineering
Salary: 60000
```

***

## Problem 14 - Data Pipeline Class

Create:

```python
class DataPipeline:
```

The `__init__()` should receive:

```
pipeline_name
source
destination
status
```

Create two different pipeline objects.

Add a method:

```python
display_pipeline()
```

that displays all pipeline information.

***

## Problem 15 - File Class

Create:

```python
class DataFile:
```

Attributes:

```
file_name
file_type
file_size
```

Add:

```python
is_csv()
```

The method should return `True` if:

```
file_type == "csv"
```

Otherwise return:

```
False
```

***

## 55. Quick Comparison

| Concept                    | Purpose                                     |
| -------------------------- | ------------------------------------------- |
| Lambda                     | Small anonymous function                    |
| Recursion                  | Function calling itself                     |
| `*args`                    | Variable positional arguments               |
| `**kwargs`                 | Variable keyword arguments                  |
| `__init__()`               | Initializes a newly created object          |
| Parameterized `__init__()` | Initializes an object using supplied values |
| `self`                     | Refers to the current object                |

***

## 56. Key Takeaways

1. Lambda functions are small anonymous functions.
2. Lambda syntax is:

```python
lambda arguments: expression
```

3. Lambda functions are commonly used with:

```
sorted()
map()
filter()
```

4. For complicated logic, use a normal `def` function instead of lambda.
5. Recursion occurs when a function calls itself.
6. Every recursive function needs a proper stopping condition called the base case.
7. The recursive case should move toward the base case.
8. Recursion is useful for naturally hierarchical structures such as:

```
Nested JSON
Trees
Directories
Parent-child data
```

9. For simple repetition, loops are usually easier to understand and maintain.
10. `*args` accepts a variable number of positional arguments.
11. `*args` collects the arguments into a tuple.
12. `**kwargs` accepts a variable number of keyword arguments.
13. `**kwargs` collects the arguments into a dictionary.
14. `*` can also unpack lists and tuples when calling functions.
15. `**` can unpack dictionaries when calling functions.
16. A class acts as a blueprint for creating objects.
17. `__init__()` is automatically invoked during object initialization.
18. `self` refers to the current object.
19. Attributes created using `self.attribute` belong to the object.
20. A parameterized `__init__()` accepts values while creating an object.
21. Different objects created from the same class can contain different values.
22. Classes can contain both attributes and methods.
23. In Data Engineering, classes can be used to represent concepts such as pipelines, files, connections, jobs, configurations, and processors.
