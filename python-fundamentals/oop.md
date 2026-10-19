# Page

## OOP

> Imported from [`python/OOP.ipynb`](https://github.com/karthikeyanm-dev/DE-Notes/blob/main/python/OOP.ipynb). Code outputs are omitted; run the source notebook for live results.

## Object-Oriented Programming (OOP) in Python

## Before Starting OOP

Before learning OOP, it is important to understand **why OOP is needed**.

We can write Python programs without classes and objects by using:

```
Variables
Functions
Lists
Dictionaries
Modules
```

This approach works very well for small programs.

But as the application becomes larger, managing hundreds of functions and data separately can become difficult.

OOP provides a way to organize:

```
Data
+
Functions that operate on that data
```

into a single logical unit called an **object**.

***

## 1. Example - Before OOP

Suppose we are building a small Data Engineering application for processing files.

We need functions for:

```
Reading a file
Validating a file
Cleaning data
Transforming data
Loading data
Generating logs
```

Without OOP, we might write:

```python
def read_file(file_name):
    print(f"Reading {file_name}")


def validate_file(file_name):
    print(f"Validating {file_name}")


def clean_data(file_name):
    print(f"Cleaning {file_name}")


def transform_data(file_name):
    print(f"Transforming {file_name}")


def load_data(file_name):
    print(f"Loading {file_name}")


def generate_log(file_name, status):
    print(f"File: {file_name}, Status: {status}")


file_name = "customers.csv"

read_file(file_name)
validate_file(file_name)
clean_data(file_name)
transform_data(file_name)
load_data(file_name)
generate_log(file_name, "SUCCESS")
```

Output:

```
Reading customers.csv
Validating customers.csv
Cleaning customers.csv
Transforming customers.csv
Loading customers.csv
File: customers.csv, Status: SUCCESS
```

This works perfectly for a small program.

***

## 2. Problem as the Application Grows

Now imagine we have:

```
100 files
20 file types
Multiple data sources
Multiple destinations
Different validation rules
Different transformation rules
Logging
Error handling
Configuration
```

We might end up with many functions:

```
read_csv()
read_json()
read_parquet()

validate_csv()
validate_json()
validate_parquet()

clean_csv()
clean_json()
clean_parquet()

transform_csv()
transform_json()
transform_parquet()

load_to_mysql()
load_to_postgres()
load_to_s3()
load_to_warehouse()
```

And many variables:

```
csv_file_name
csv_file_size
csv_status

json_file_name
json_file_size
json_status

parquet_file_name
parquet_file_size
parquet_status
```

Now the program becomes harder to organize.

The main problem is that:

```
Data
```

and

```
Functions operating on that data
```

are separate.

***

## 3. Using Modules Before OOP

We can improve the organization by splitting functions into modules.

For example:

```
project/
│
├── file_reader.py
├── validator.py
├── transformer.py
├── loader.py
└── main.py
```

### file\_reader.py

```python
def read_file(file_name):

    print(f"Reading {file_name}")
```

### validator.py

```python
def validate_file(file_name):

    print(f"Validating {file_name}")
```

### transformer.py

```python
def transform_data(file_name):

    print(f"Transforming {file_name}")
```

### loader.py

```python
def load_data(file_name):

    print(f"Loading {file_name}")
```

### main.py

```python
from file_reader import read_file
from validator import validate_file
from transformer import transform_data
from loader import load_data


file_name = "customers.csv"

read_file(file_name)

validate_file(file_name)

transform_data(file_name)

load_data(file_name)
```

This is better because the code is divided into modules.

But there is still a problem.

The file's:

```
Name
Size
Type
Status
Configuration
```

are still separate from the functions that operate on the file.

***

## 4. After OOP

With OOP, we can group:

```
Data
+
Functions related to that data
```

into a class.

For example:

```python
class DataFile:

    def __init__(self, file_name, file_type):

        self.file_name = file_name
        self.file_type = file_type


    def read(self):

        print(f"Reading {self.file_name}")


    def validate(self):

        print(f"Validating {self.file_name}")


    def transform(self):

        print(f"Transforming {self.file_name}")


    def load(self):

        print(f"Loading {self.file_name}")
```

Create an object:

```python
file1 = DataFile(
    "customers.csv",
    "csv"
)
```

Now the data and its related operations are together:

```
file1
 │
 ├── file_name
 ├── file_type
 │
 ├── read()
 ├── validate()
 ├── transform()
 └── load()
```

Use the object:

```python
file1.read()
file1.validate()
file1.transform()
file1.load()
```

Output:

```
Reading customers.csv
Validating customers.csv
Transforming customers.csv
Loading customers.csv
```

***

## 5. Multiple Objects

The biggest advantage becomes clearer when we have multiple files.

```python
file1 = DataFile(
    "customers.csv",
    "csv"
)

file2 = DataFile(
    "orders.json",
    "json"
)

file3 = DataFile(
    "products.parquet",
    "parquet"
)
```

Each object contains its own data:

```
file1
→ customers.csv
→ csv

file2
→ orders.json
→ json

file3
→ products.parquet
→ parquet
```

And each object can use the same methods:

```python
file1.validate()
file2.validate()
file3.validate()
```

This is the fundamental idea behind OOP.

***

## 6. What OOP Provides

OOP helps us organize software around objects.

The major concepts are:

```
Class
Object
Attributes
Methods
self

Inheritance
Polymorphism
Encapsulation
Abstraction
```

The four major pillars of OOP are:

```
Encapsulation
Inheritance
Polymorphism
Abstraction
```

***

## Part 1 - Class

## 7. What is a Class?

### Definition

A class is a blueprint or template used to create objects.

A class defines:

```
What data an object should contain
+
What operations an object can perform
```

Example:

```python
class DataFile:

    pass
```

Here:

```
DataFile
```

is a class.

***

## 8. Real-World Analogy

Think about a building blueprint.

The blueprint defines:

```
Rooms
Doors
Windows
Structure
```

But the blueprint itself is not the actual building.

Similarly:

```
Class
↓
Blueprint

Object
↓
Actual instance
```

***

## Part 2 - Object

## 9. What is an Object?

### Definition

An object is an instance of a class.

Example:

```python
class DataFile:

    pass


file1 = DataFile()
```

Here:

```
DataFile
→ Class

file1
→ Object
```

We can create multiple objects:

```python
file1 = DataFile()
file2 = DataFile()
file3 = DataFile()
```

All three objects are created from the same class.

***

## 10. Class vs Object

| Class                   | Object               |
| ----------------------- | -------------------- |
| Blueprint               | Actual instance      |
| Defines structure       | Contains actual data |
| Concept/template        | Real entity          |
| Can create many objects | Created from a class |

Example:

```
DataFile
    ↓
    Class

    ↓
 ┌──────────────┐
 │ file1        │
 │ customers.csv│
 └──────────────┘

 ┌──────────────┐
 │ file2        │
 │ orders.json  │
 └──────────────┘
```

***

## Part 3 - Attributes

## 11. What are Attributes?

### Definition

Attributes are variables that belong to an object.

Example:

```python
class DataFile:

    def __init__(self, file_name, file_type):

        self.file_name = file_name
        self.file_type = file_type
```

Here:

```
self.file_name
self.file_type
```

are attributes.

Create object:

```python
file1 = DataFile(
    "customers.csv",
    "csv"
)
```

Access attributes:

```python
print(file1.file_name)
print(file1.file_type)
```

Output:

```
customers.csv
csv
```

***

## 12. Instance Attributes

Attributes created using `self` belong to individual objects.

```python
class Customer:

    def __init__(self, name, city):

        self.name = name
        self.city = city
```

Create two objects:

```python
customer1 = Customer(
    "Arun",
    "Chennai"
)

customer2 = Customer(
    "Rahul",
    "Mumbai"
)
```

Each object has its own data:

```
customer1
→ Arun
→ Chennai

customer2
→ Rahul
→ Mumbai
```

***

## Part 4 - Methods

## 13. What is a Method?

### Definition

A method is a function defined inside a class.

Example:

```python
class Customer:

    def __init__(self, name):

        self.name = name


    def display(self):

        print(self.name)
```

Create object:

```python
customer1 = Customer("Arun")

customer1.display()
```

Output:

```
Arun
```

The difference is:

```
Function
→ Defined independently

Method
→ Defined inside a class
```

***

## 14. Attributes + Methods

A class usually combines:

```
Attributes
+
Methods
```

Example:

```python
class DataFile:

    def __init__(self, file_name):

        self.file_name = file_name


    def read(self):

        print(f"Reading {self.file_name}")


    def validate(self):

        print(f"Validating {self.file_name}")
```

Here:

```
Attribute:
self.file_name

Methods:
read()
validate()
```

***

## Part 5 - self

## 15. What is self?

### Definition

`self` refers to the current object.

Example:

```python
class Customer:

    def __init__(self, name):

        self.name = name
```

When:

```python
customer1 = Customer("Arun")
```

conceptually:

```
self
↓
customer1
```

Therefore:

```python
self.name = name
```

means that the value `"Arun"` is stored in the current object's `name` attribute.

***

## 16. Why self is Important

Consider:

```python
class Customer:

    def __init__(self, name):

        self.name = name
```

Suppose:

```python
customer1 = Customer("Arun")
customer2 = Customer("Rahul")
```

Conceptually:

```
For customer1:

self → customer1
self.name → Arun


For customer2:

self → customer2
self.name → Rahul
```

Therefore each object maintains separate data.

***

## 17. self with Methods

```python
class Customer:

    def __init__(self, name, city):

        self.name = name
        self.city = city


    def display(self):

        print(self.name)
        print(self.city)
```

Create object:

```python
customer1 = Customer(
    "Arun",
    "Chennai"
)

customer1.display()
```

Output:

```
Arun
Chennai
```

Inside `display()`:

```python
self.name
self.city
```

refer to the attributes of the current object.

***

## Part 6 - `__init__()`

## 18. What is `__init__()`?

`__init__()` is a special method used to initialize an object's attributes.

It is automatically called when an object is created.

Example:

```python
class Customer:

    def __init__(self, name):

        self.name = name


customer1 = Customer("Arun")
```

Python automatically calls the initialization method as part of object creation.

***

## 19. Parameterized `__init__()`

A parameterized `__init__()` accepts values during object creation.

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

Create object:

```python
customer1 = Customer(
    1001,
    "Arun",
    "Chennai"
)
```

Now:

```
customer1.customer_id → 1001
customer1.name        → Arun
customer1.city        → Chennai
```

***

## Part 7 - Encapsulation

## 20. What is Encapsulation?

### Definition

Encapsulation means bundling data and the methods that operate on that data together inside a class, while controlling how that data is accessed or modified.

In simple terms:

```
Data
+
Related Methods
+
Controlled Access
```

Encapsulation helps protect an object's internal state from inappropriate direct modification.

***

## 21. Real-World Analogy

Think about a bank account.

A customer should not directly change:

```
balance = ₹1,00,000
```

to:

```
balance = ₹10,00,000
```

Instead, the bank provides controlled operations:

```
deposit()
withdraw()
check_balance()
```

The internal balance should be managed through appropriate rules.

***

## 22. Encapsulation Example

```python
class BankAccount:

    def __init__(self, balance):

        self.__balance = balance


    def deposit(self, amount):

        if amount > 0:
            self.__balance += amount


    def get_balance(self):

        return self.__balance
```

Use:

```python
account = BankAccount(10000)

account.deposit(5000)

print(account.get_balance())
```

Output:

```
15000
```

The balance is intended to be managed through the class's methods.

***

## 23. Private Variables

Python does not have Java-style strict private fields, but it supports **name mangling** for attributes beginning with double underscores.

Example:

```python
class BankAccount:

    def __init__(self, balance):

        self.__balance = balance
```

Here:

```python
__balance
```

is treated as a private-by-convention/internal attribute and is name-mangled by Python.

Direct access:

```python
account = BankAccount(10000)

print(account.__balance)
```

raises an `AttributeError`.

The intended interface is through methods such as:

```python
account.get_balance()
```

***

## 24. Why Private Variables?

Private-style attributes help us:

```
Protect internal state
Control modifications
Apply validation
Hide implementation details
Maintain consistency
```

For example, an account balance should not normally become negative simply because someone directly assigned an invalid value.

***

## Part 8 - Getter and Setter

## 25. What is a Getter?

A getter is a method used to retrieve an internal value.

Example:

```python
class Employee:

    def __init__(self, salary):

        self.__salary = salary


    def get_salary(self):

        return self.__salary
```

Usage:

```python
employee = Employee(60000)

print(employee.get_salary())
```

Output:

```
60000
```

Here:

```python
get_salary()
```

is the getter.

***

## 26. What is a Setter?

A setter is a method used to modify an internal value in a controlled way.

```python
class Employee:

    def __init__(self, salary):

        self.__salary = salary


    def get_salary(self):

        return self.__salary


    def set_salary(self, salary):

        if salary > 0:
            self.__salary = salary
```

Usage:

```python
employee = Employee(60000)

employee.set_salary(70000)

print(employee.get_salary())
```

Output:

```
70000
```

***

## 27. Getter and Setter with Validation

```python
class Employee:

    def __init__(self, salary):

        self.__salary = salary


    def get_salary(self):

        return self.__salary


    def set_salary(self, salary):

        if salary <= 0:
            print("Invalid salary")
            return

        self.__salary = salary
```

Valid:

```python
employee = Employee(60000)

employee.set_salary(70000)

print(employee.get_salary())
```

Output:

```
70000
```

Invalid:

```python
employee.set_salary(-5000)
```

Output:

```
Invalid salary
```

The setter provides a controlled way to modify the data.

***

## 28. Property - Pythonic Getter and Setter

Python provides `@property` for creating getter/setter-style interfaces.

```python
class Employee:

    def __init__(self, salary):

        self.__salary = salary


    @property
    def salary(self):

        return self.__salary


    @salary.setter
    def salary(self, value):

        if value <= 0:
            raise ValueError("Salary must be positive")

        self.__salary = value
```

Usage:

```python
employee = Employee(60000)

print(employee.salary)

employee.salary = 70000

print(employee.salary)
```

Output:

```
60000
70000
```

This allows us to use:

```python
employee.salary
```

instead of:

```python
employee.get_salary()
```

and:

```python
employee.salary = 70000
```

instead of:

```python
employee.set_salary(70000)
```

***

## Part 9 - Inheritance

## 29. What is Inheritance?

### Definition

Inheritance allows one class to acquire or reuse attributes and methods from another class.

The existing class is commonly called:

```
Parent Class
Base Class
Super Class
```

The new class is commonly called:

```
Child Class
Derived Class
Sub Class
```

***

## 30. Real-World Data Engineering Analogy

Imagine different types of data files:

```
DataFile
   │
   ├── CSVFile
   ├── JSONFile
   └── ParquetFile
```

All files may have common operations:

```
file_name
read()
validate()
```

But each format may have specialized behavior.

Instead of rewriting common functionality in every class, we can create a base class.

***

## 31. Basic Inheritance

```python
class DataFile:

    def read(self):

        print("Reading data file")


class CSVFile(DataFile):

    pass
```

Create object:

```python
file = CSVFile()

file.read()
```

Output:

```
Reading data file
```

`CSVFile` inherited `read()` from `DataFile`.

***

## 32. Inheritance with `__init__()`

```python
class DataFile:

    def __init__(self, file_name):

        self.file_name = file_name


    def display(self):

        print(self.file_name)


class CSVFile(DataFile):

    pass
```

Create:

```python
file = CSVFile("customers.csv")

file.display()
```

Output:

```
customers.csv
```

The child class inherited the initialization behavior and `display()` method.

***

## 33. Method Overriding

A child class can provide its own implementation of a method inherited from the parent.

```python
class DataFile:

    def read(self):

        print("Reading generic file")


class CSVFile(DataFile):

    def read(self):

        print("Reading CSV file")
```

Create:

```python
file = CSVFile()

file.read()
```

Output:

```
Reading CSV file
```

The child implementation overrides the parent's implementation.

***

## 34. `super()`

`super()` is used to access functionality from the parent class.

```python
class DataFile:

    def __init__(self, file_name):

        self.file_name = file_name


class CSVFile(DataFile):

    def __init__(self, file_name, delimiter):

        super().__init__(file_name)

        self.delimiter = delimiter
```

Create:

```python
file = CSVFile(
    "customers.csv",
    ","
)

print(file.file_name)
print(file.delimiter)
```

Output:

```
customers.csv
,
```

***

## Part 10 - Polymorphism

## 35. What is Polymorphism?

### Definition

Polymorphism means **one interface, multiple forms**.

In OOP, different objects can respond to the same method call in different ways.

Example:

```
read()
```

can mean:

```
CSV → Read CSV
JSON → Read JSON
Parquet → Read Parquet
```

The caller can use the same method name:

```python
file.read()
```

without needing to know the internal implementation.

***

## 36. Polymorphism Example

```python
class CSVFile:

    def read(self):

        print("Reading CSV file")


class JSONFile:

    def read(self):

        print("Reading JSON file")


class ParquetFile:

    def read(self):

        print("Reading Parquet file")
```

Create objects:

```python
files = [
    CSVFile(),
    JSONFile(),
    ParquetFile()
]
```

Use the same method:

```python
for file in files:

    file.read()
```

Output:

```
Reading CSV file
Reading JSON file
Reading Parquet file
```

The same:

```python
file.read()
```

behaves differently depending on the object.

***

## 37. Polymorphism Through Inheritance

```python
class DataFile:

    def read(self):

        print("Reading generic file")


class CSVFile(DataFile):

    def read(self):

        print("Reading CSV")


class JSONFile(DataFile):

    def read(self):

        print("Reading JSON")


files = [
    CSVFile(),
    JSONFile()
]

for file in files:

    file.read()
```

Output:

```
Reading CSV
Reading JSON
```

This is a very important OOP concept for designing extensible systems.

***

## Part 11 - Abstraction

## 38. What is Abstraction?

### Definition

Abstraction means hiding unnecessary implementation details and exposing only the essential interface.

In simple terms:

```
User needs to know:
WHAT to do

User does not need to know:
HOW everything is implemented internally
```

***

## 39. Real-World Analogy

Think about using an ATM.

You interact with:

```
Insert Card
Enter PIN
Withdraw Money
Check Balance
```

You do not need to know:

```
How the bank database works
How transactions are internally processed
How the bank server communicates
How the balance is stored
```

The ATM provides an interface and hides the internal implementation.

***

## 40. Abstraction in Data Engineering

Suppose we have different data sources:

```
MySQL
PostgreSQL
API
CSV
```

We might define a common interface:

```
connect()
extract()
```

The user of the class only needs to know:

```python
source.connect()
source.extract()
```

The internal connection logic can be different for every source.

***

## Part 12 - Abstract Class

## 41. What is an Abstract Class?

An abstract class is a class intended to define a common interface or structure for its subclasses.

It can contain:

```
Concrete methods
Abstract methods
```

An abstract method defines a method that subclasses are expected to implement.

Python provides this through the `abc` module.

***

## 42. Creating an Abstract Class

```python
from abc import ABC, abstractmethod


class DataSource(ABC):

    @abstractmethod
    def extract(self):

        pass
```

`DataSource` is an abstract class.

`extract()` is an abstract method.

***

## 43. Implementing the Abstract Class

```python
from abc import ABC, abstractmethod


class DataSource(ABC):

    @abstractmethod
    def extract(self):

        pass


class MySQLSource(DataSource):

    def extract(self):

        print("Extracting data from MySQL")
```

Create object:

```python
source = MySQLSource()

source.extract()
```

Output:

```
Extracting data from MySQL
```

***

## 44. Why Abstract Classes?

Abstract classes are useful when we want multiple classes to follow the same contract.

For example:

```
DataSource
    │
    ├── MySQLSource
    ├── PostgreSQLSource
    ├── APISource
    └── CSVSource
```

Every source should implement:

```
extract()
```

But each source can implement it differently.

***

## 45. Abstract Class + Polymorphism

```python
from abc import ABC, abstractmethod


class DataSource(ABC):

    @abstractmethod
    def extract(self):

        pass


class MySQLSource(DataSource):

    def extract(self):

        print("Extracting from MySQL")


class APISource(DataSource):

    def extract(self):

        print("Extracting from API")


class CSVSource(DataSource):

    def extract(self):

        print("Extracting from CSV")
```

Create objects:

```python
sources = [
    MySQLSource(),
    APISource(),
    CSVSource()
]
```

Same interface:

```python
for source in sources:

    source.extract()
```

Output:

```
Extracting from MySQL
Extracting from API
Extracting from CSV
```

This combines:

```
Abstraction
+
Inheritance
+
Polymorphism
```

***

## Part 13 - Four Pillars of OOP

## 46. Encapsulation

### Definition

Encapsulation bundles data and related methods together and controls access to internal state.

Example:

```
BankAccount
    │
    ├── balance
    ├── deposit()
    └── withdraw()
```

The balance should be managed through appropriate operations.

***

## 47. Inheritance

### Definition

Inheritance allows a child class to reuse or extend functionality from a parent class.

Example:

```
DataFile
   │
   ├── CSVFile
   ├── JSONFile
   └── ParquetFile
```

***

## 48. Polymorphism

### Definition

Polymorphism allows different objects to respond to the same interface in different ways.

Example:

```python
source.extract()
```

could mean:

```
MySQL → Extract from MySQL
API   → Extract from API
CSV   → Extract from CSV
```

***

## 49. Abstraction

### Definition

Abstraction hides implementation details and exposes the essential interface.

Example:

```python
source.extract()
```

The user knows what operation is required without needing to know all the internal extraction logic.

***

## 50. Four Pillars Summary

| Pillar        | Main Idea                          | Example                   |
| ------------- | ---------------------------------- | ------------------------- |
| Encapsulation | Protect/control data               | Private balance + methods |
| Inheritance   | Reuse/extend code                  | CSVFile inherits DataFile |
| Polymorphism  | Same interface, different behavior | `source.extract()`        |
| Abstraction   | Hide implementation details        | Abstract `DataSource`     |

***

## Part 14 - Complete OOP Data Engineering Example

Consider a Data Engineering system supporting multiple data sources.

```python
from abc import ABC, abstractmethod


class DataSource(ABC):

    def __init__(self, source_name):

        self.source_name = source_name


    @abstractmethod
    def extract(self):

        pass


    def display_source(self):

        print(
            f"Source: {self.source_name}"
        )


class MySQLSource(DataSource):

    def extract(self):

        print(
            "Extracting data from MySQL"
        )


class APISource(DataSource):

    def extract(self):

        print(
            "Extracting data from API"
        )


class CSVSource(DataSource):

    def extract(self):

        print(
            "Extracting data from CSV"
        )


sources = [
    MySQLSource("Customer Database"),
    APISource("Customer API"),
    CSVSource("Customer File")
]


for source in sources:

    source.display_source()
    source.extract()
    print()
```

Output:

```
Source: Customer Database
Extracting data from MySQL

Source: Customer API
Extracting data from API

Source: Customer File
Extracting data from CSV
```

This single example demonstrates:

```
Class
Object
Attributes
Methods
self
__init__()
Inheritance
Polymorphism
Abstraction
Abstract Class
```

***

## 51. Where Encapsulation Fits

We can add a private-style attribute to the base class.

```python
from abc import ABC, abstractmethod


class DataSource(ABC):

    def __init__(self, source_name):

        self.__source_name = source_name


    def get_source_name(self):

        return self.__source_name


    @abstractmethod
    def extract(self):

        pass
```

Now the internal source name is accessed through:

```python
get_source_name()
```

instead of direct access to:

```python
__source_name
```

This demonstrates controlled access.

***

## 52. Complete Four-Pillar Structure

A well-designed OOP Data Engineering system can look conceptually like:

```
                    DataSource
                       │
              ┌────────┴────────┐
              │                 │
          MySQLSource        APISource
              │                 │
              └────────┬────────┘
                       │
                 common interface
                       │
                    extract()
```

#### Abstraction

```
DataSource defines extract()
```

#### Inheritance

```
MySQLSource inherits DataSource
APISource inherits DataSource
```

#### Polymorphism

```
source.extract()
```

behaves differently for each source.

#### Encapsulation

Internal data and implementation details are controlled inside the class.

***

## 53. Before OOP vs After OOP

### Before OOP

```
Functions
│
├── read_file()
├── validate_file()
├── clean_data()
├── transform_data()
└── load_data()

Variables
│
├── file_name
├── file_type
├── file_status
└── file_size
```

Data and operations are separate.

***

### Modules

```
project/
│
├── reader.py
├── validator.py
├── transformer.py
├── loader.py
└── main.py
```

Better organization, but data and behavior may still be separated.

***

### After OOP

```
DataFile
│
├── Attributes
│   ├── file_name
│   ├── file_type
│   └── file_size
│
└── Methods
    ├── read()
    ├── validate()
    ├── transform()
    └── load()
```

Now:

```
Related Data
      +
Related Behavior
      ↓
   Object
```

***

## 54. Important Mental Model

When learning OOP, think in this order:

```
Class
  ↓
Blueprint

Object
  ↓
Actual instance

Attributes
  ↓
Object's data

Methods
  ↓
Object's behavior

self
  ↓
Current object

__init__()
  ↓
Initialize object

Inheritance
  ↓
Reuse / extend another class

Polymorphism
  ↓
Same interface, different behavior

Encapsulation
  ↓
Bundle and control data

Abstraction
  ↓
Expose essential interface
```

***

## 55. Important Interview Definitions

### Class

A class is a blueprint or template for creating objects.

### Object

An object is an instance of a class.

### Attribute

An attribute is data associated with an object.

### Method

A method is a function defined inside a class that represents behavior.

### self

`self` refers to the current object instance.

### `__init__()`

`__init__()` is a special method used to initialize an object's state when the object is created.

### Inheritance

Inheritance allows a class to reuse and extend functionality from another class.

### Polymorphism

Polymorphism allows different objects to respond to the same interface in different ways.

### Encapsulation

Encapsulation combines data and related behavior while controlling access to an object's internal state.

### Abstraction

Abstraction hides unnecessary implementation details and exposes only the essential interface.

### Abstract Class

An abstract class defines a common interface or structure for subclasses and may contain abstract methods that subclasses must implement.

### Private Variable

In Python, a name beginning with `__` inside a class is name-mangled and is commonly used to indicate internal/private implementation details.

### Getter

A getter provides controlled access to an internal value.

### Setter

A setter provides controlled modification of an internal value, often allowing validation before changing it.

## 56. Key Takeaways

1. OOP is a way of organizing programs around objects.
2. OOP becomes especially useful as applications become larger and more complex.
3. Before OOP, programs can be organized using functions and modules.
4. Modules help separate code into files.
5. OOP goes one step further by grouping related data and behavior together.
6. A class is a blueprint.
7. An object is an instance of a class.
8. Attributes represent object data.
9. Methods represent object behavior.
10. `self` refers to the current object.
11. `__init__()` initializes an object's state.
12. A parameterized `__init__()` accepts values while creating an object.
13. Encapsulation bundles data and behavior and provides controlled access.
14. Private-style attributes using `__name` are name-mangled by Python.
15. Getters retrieve internal values.
16. Setters modify internal values with optional validation.
17. `@property` provides a Pythonic way to expose getter/setter behavior.
18. Inheritance allows reuse and extension of existing classes.
19. Method overriding allows a child class to provide specialized behavior.
20. `super()` allows access to parent-class functionality.
21. Polymorphism means the same interface can produce different behavior depending on the object.
22. Abstraction hides implementation details and exposes essential functionality.
23. Abstract classes can define contracts that subclasses must implement.
24. The `abc` module provides `ABC` and `@abstractmethod`.
25. The four pillars of OOP are:

```
Encapsulation
Inheritance
Polymorphism
Abstraction
```

26. A common Data Engineering OOP structure can be:

```
DataSource
    ↓
MySQLSource
APISource
CSVSource
    ↓
extract()
```

27. OOP is particularly useful for building reusable and maintainable Data Engineering components such as:

```
Data Sources
File Processors
Database Connections
ETL Pipelines
API Clients
Data Validators
Transformers
Loaders
Pipeline Configurations
```
