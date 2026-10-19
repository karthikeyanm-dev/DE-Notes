# Page

## Strings

> Imported from [`python/Strings.ipynb`](https://github.com/karthikeyanm-dev/DE-Notes/blob/main/python/Strings.ipynb). Code outputs are omitted; run the source notebook for live results.

## Strings - Indexing, Slicing, Traversing, and String Immutability

### Introduction

A string is a sequence of characters.

For example:

```python
text = "Python"
```

The string can be visualized as:

```
Character:  P   y   t   h   o   n
Index:      0   1   2   3   4   5
Negative:  -6  -5  -4  -3  -2  -1
```

Because strings are sequences, Python allows us to:

* Access individual characters using indexing
* Extract portions using slicing
* Process characters one by one using traversal
* Understand how string immutability works

## 1. Indexing

### Definition

Indexing is the process of accessing an individual character from a string using its position.

Python uses **zero-based indexing**.

This means the first character has index `0`.

### Real-World Analogy

Think of a row of seats in a classroom.

```
Seat:       A    B    C    D    E
Position:   0    1    2    3    4
```

If you want the student sitting in the third position, you access position `2` because Python starts counting from `0`.

The same concept applies to characters in a string.

### Example

```python
text = "Python"

print(text[0])
print(text[1])
print(text[2])
```

Output:

```
P
y
t
```

### String Index Positions

For:

```python
text = "Python"
```

The positions are:

```
Character:  P   y   t   h   o   n
Index:      0   1   2   3   4   5
```

Therefore:

```python
print(text[0])
```

Output:

```
P
```

And:

```python
print(text[5])
```

Output:

```
n
```

## 2. Negative Indexing

### Definition

Python also supports negative indexing.

Negative indexing starts from the end of the string.

For:

```python
text = "Python"
```

The positions are:

```
Character:  P   y   t   h   o   n
Index:      0   1   2   3   4   5
Negative:  -6  -5  -4  -3  -2  -1
```

The last character always has index `-1`.

### Example

```python
text = "Python"

print(text[-1])
print(text[-2])
print(text[-3])
```

Output:

```
n
o
h
```

### Data Engineering Example

Suppose we receive file names:

```python
file_name = "customers.csv"

print(file_name[-3:])
```

Output:

```
csv
```

This can help extract information from strings, although methods such as `.endswith()` are often better for checking file extensions.

## 3. Indexing a Data Engineering Value

Suppose we have a customer ID:

```python
customer_id = "CUS10025"

print(customer_id[0])
print(customer_id[1])
print(customer_id[-1])
```

Output:

```
C
U
5
```

This can be useful when working with structured identifiers where certain characters have specific meanings.

## 4. IndexError

If we try to access an index that does not exist, Python raises an `IndexError`.

```python
text = "Python"

print(text[10])
```

This produces an error similar to:

```
IndexError: string index out of range
```

The valid indexes are:

```
0 to 5
```

for the string `"Python"`.

## 5. Slicing

### Definition

Slicing is used to extract a portion of a string.

Instead of accessing only one character, slicing allows us to retrieve multiple characters.

### Syntax

```python
string[start:stop]
```

Important:

* `start` is included.
* `stop` is excluded.

### Example

```python
text = "Python"

print(text[0:3])
```

Output:

```
Pyt
```

The indexes are:

```
P → 0
y → 1
t → 2
h → 3
```

Since the stop index is `3`, Python stops before index `3`.

Therefore:

```
0, 1, 2
```

are included.

## 6. Slicing with Start and Stop

```python
text = "DataEngineering"

print(text[0:4])
```

Output:

```
Data
```

Another example:

```python
text = "DataEngineering"

print(text[4:16])
```

Output:

```
Engineering
```

## 7. Omitting Start

If the start index is omitted, Python starts from the beginning.

```python
text = "Python"

print(text[:3])
```

Output:

```
Pyt
```

This is equivalent to:

```python
text[0:3]
```

## 8. Omitting Stop

If the stop index is omitted, Python continues until the end.

```python
text = "Python"

print(text[3:])
```

Output:

```
hon
```

This means:

```
Start from index 3
Continue until the end
```

## 9. Copying a String Using Slicing

```python
text = "Python"

copy_text = text[:]

print(copy_text)
```

Output:

```
Python
```

## 10. Slicing with Negative Indexes

Negative indexes can also be used while slicing.

```python
text = "Python"

print(text[-3:])
```

Output:

```
hon
```

Another example:

```python
text = "Python"

print(text[:-2])
```

Output:

```
Pyth
```

Here:

```
-2
```

represents the second-last character.

## 11. Slicing with Step

### Syntax

```python
string[start:stop:step]
```

The `step` determines how many positions Python moves at a time.

### Example

```python
text = "Python"

print(text[0:6:2])
```

Output:

```
Pto
```

Python accesses:

```
Index 0 → P
Index 2 → t
Index 4 → o
```

## 12. Using Step to Skip Characters

```python
text = "0123456789"

print(text[::2])
```

Output:

```
02468
```

Every second character is selected.

Another example:

```python
text = "0123456789"

print(text[::3])
```

Output:

```
0369
```

## 13. Reversing a String

A negative step can be used to reverse a string.

```python
text = "Python"

print(text[::-1])
```

Output:

```
nohtyP
```

The syntax:

```python
[::-1]
```

means:

```
Start → beginning
Stop → end
Step → move backward by 1
```

## 14. Traversing a String

### Definition

Traversing means visiting each character of a string one by one.

A `for` loop is commonly used for string traversal.

### Example

```python
text = "Python"

for character in text:
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

## 15. Traversing with Index

We can also traverse a string using indexes.

```python
text = "Python"

for i in range(len(text)):
    print(i, text[i])
```

Output:

```
0 P
1 y
2 t
3 h
4 o
5 n
```

Here:

```python
len(text)
```

returns:

```
6
```

and:

```python
range(6)
```

produces:

```
0, 1, 2, 3, 4, 5
```

## 16. Traversing with enumerate()

`enumerate()` allows us to get both the index and the character.

```python
text = "Python"

for index, character in enumerate(text):
    print(index, character)
```

Output:

```
0 P
1 y
2 t
3 h
4 o
5 n
```

This is generally cleaner than manually using `range(len(text))`.

## 17. Data Engineering Example - Traversing File Names

Suppose we have a list of file names.

```python
files = [
    "customers.csv",
    "orders.csv",
    "products.csv"
]

for file in files:

    print(f"Processing file: {file}")

    for character in file:
        print(character)
```

The outer loop processes each file, while the inner loop traverses each file name character by character.

## 18. String Immutability

### Definition

Strings in Python are **immutable**.

Immutable means that once a string object is created, its individual characters cannot be changed.

### Real-World Analogy

Think of a printed document.

Once the document is printed, you cannot directly change one letter on the paper.

You need to create a new version of the document.

Python strings work similarly.

### Example

```python
text = "Python"

text[0] = "J"
```

This produces:

```
TypeError: 'str' object does not support item assignment
```

We cannot directly replace an individual character.

## 19. Creating a New String

Although we cannot modify an existing string, we can create a new string.

```python
text = "Python"

new_text = "J" + text[1:]

print(text)
print(new_text)
```

Output:

```
Python
Jython
```

The original string remains unchanged.

```
Original:
Python

New:
Jython
```

## 20. String Reassignment

We can assign a completely new string to the same variable.

```python
text = "Python"

text = "Java"

print(text)
```

Output:

```
Java
```

This does not modify the original `"Python"` string.

Instead, the variable `text` is now referencing another string object.

Think of it as:

```
Before:

text → "Python"

After:

text → "Java"
```

## 21. Immutability with replace()

String methods such as `replace()` do not modify the original string.

They create and return a new string.

```python
text = "Python"

new_text = text.replace("P", "J")

print(text)
print(new_text)
```

Output:

```
Python
Jython
```

The original string remains:

```
Python
```

The new string is:

```
Jython
```

## 22. Immutability with upper()

The same concept applies to `upper()`.

```python
text = "python"

new_text = text.upper()

print(text)
print(new_text)
```

Output:

```
python
PYTHON
```

The original string was not changed.

## 23. Why String Immutability Is Important

String immutability provides several benefits.

#### 1. Predictability

Once a string is created, its contents cannot unexpectedly change.

#### 2. Safe Sharing

Multiple variables can reference the same string without worrying that one operation will modify it.

#### 3. Hashing

Immutable strings can be used as dictionary keys and set elements.

Example:

```python
customer_city = {
    "Chennai": 100,
    "Mumbai": 200
}

print(customer_city["Chennai"])
```

#### 4. Data Processing

In Data Engineering, strings are frequently used for:

* Customer IDs
* File names
* Column names
* URLs
* Email addresses
* JSON fields
* Product names
* Log messages

Understanding immutability helps us understand how string transformations work.

## 24. Indexing vs Slicing

| Indexing                                  | Slicing                                         |
| ----------------------------------------- | ----------------------------------------------- |
| Retrieves a single character              | Retrieves a portion of a string                 |
| Uses one index                            | Uses start and stop positions                   |
| Example: `text[2]`                        | Example: `text[1:4]`                            |
| Returns a string containing one character | Returns a string containing multiple characters |

Example:

```python
text = "Python"

print(text[2])
print(text[1:4])
```

Output:

```
t
yth
```

## 25. Traversing vs Indexing

Indexing:

```python
text = "Python"

print(text[2])
```

Output:

```
t
```

Only one character is accessed.

Traversing:

```python
text = "Python"

for character in text:
    print(character)
```

Every character is accessed one by one.

## 26. Data Engineering Example - Extracting File Information

Suppose we receive a file name:

```python
file_name = "customer_orders_2026.csv"
```

We can use slicing to inspect parts of the string.

```python
print(file_name[-3:])
```

Output:

```
csv
```

We can also check the beginning of the file name:

```python
print(file_name[:8])
```

Output:

```
customer
```

A better approach for extension validation would be:

```python
if file_name.endswith(".csv"):
    print("CSV file")
```

Slicing is useful when we need to extract a specific portion of a string.

## 27. Data Engineering Example - Customer ID

Suppose customer IDs follow this format:

```
CUS10025
```

We can extract the prefix and numeric portion.

```python
customer_id = "CUS10025"

prefix = customer_id[:3]
number = customer_id[3:]

print(prefix)
print(number)
```

Output:

```
CUS
10025
```

This type of string manipulation can be useful when processing structured identifiers.

## 28. Data Engineering Example - Data Cleaning

Suppose a dataset contains customer names with unwanted spaces.

```python
customer_name = "  Arun Kumar  "

clean_name = customer_name.strip()

print(clean_name)
```

Output:

```
Arun Kumar
```

The original string remains unchanged:

```python
print(customer_name)
```

Output:

```
  Arun Kumar  
```

The `strip()` method returns a new string.

## 29. Complete Example

The following example combines indexing, slicing, traversal, and string immutability.

```python
customer_id = "CUS10025"

# Indexing

print("First character:", customer_id[0])
print("Last character:", customer_id[-1])


# Slicing

print("Prefix:", customer_id[:3])
print("Numeric portion:", customer_id[3:])


# Traversing

print("\nCharacters:")

for index, character in enumerate(customer_id):
    print(index, character)


# String immutability

new_customer_id = "ORD" + customer_id[3:]

print("\nOriginal ID:", customer_id)
print("New ID:", new_customer_id)
```

Output:

```
First character: C
Last character: 5
Prefix: CUS
Numeric portion: 10025

Characters:
0 C
1 U
2 S
3 1
4 0
5 0
6 2
7 5

Original ID: CUS10025
New ID: ORD10025
```

## 30. Key Takeaways

1. A string is a sequence of characters.
2. Python uses zero-based indexing.
3. The first character has index `0`.
4. The last character can be accessed using `-1`.
5. Negative indexing starts from the end.
6. Slicing extracts a portion of a string.
7. Slicing uses the syntax:

```python
string[start:stop:step]
```

8. The start index is included.
9. The stop index is excluded.
10. `[::-1]` can be used to reverse a string.
11. Traversing means processing characters one by one.
12. A `for` loop can be used to traverse strings.
13. `enumerate()` provides both index and value while traversing.
14. Strings are immutable in Python.
15. Individual characters of a string cannot be changed directly.
16. String methods such as `replace()` and `upper()` return new strings instead of modifying the original string.
17. Reassigning a variable creates a new reference; it does not modify the original string object.
18. Indexing, slicing, and traversal are fundamental operations when cleaning and transforming textual data in Data Engineering.

## Python String Methods - split(), join(), replace(), find(), count(), strip(), upper(), lower(), startswith(), endswith()

### Introduction

Python provides many built-in string methods for processing and transforming text.

These methods are extremely important in Data Engineering because data frequently arrives as text from:

* CSV files
* JSON files
* APIs
* Logs
* Databases
* Flat files
* Message queues

Common string operations include:

```
split()
join()
replace()
find()
count()
strip()
upper()
lower()
startswith()
endswith()
```

These methods help us clean, validate, search, transform, and prepare textual data before loading it into a database or data warehouse.

## 1. split()

### Definition

The `split()` method divides a string into multiple parts and returns the result as a list.

### Syntax

```python
string.split(separator)
```

If no separator is provided, Python splits the string based on whitespace.

### Basic Example

```python
text = "Python Data Engineering"

words = text.split()

print(words)
```

Output:

```
['Python', 'Data', 'Engineering']
```

The original string:

```
Python Data Engineering
```

is divided into:

```
Python
Data
Engineering
```

### Real-World Analogy

Imagine receiving a line from a CSV file:

```
1001,Arun,Chennai,25000
```

You can split the line using the comma:

```python
record = "1001,Arun,Chennai,25000"

data = record.split(",")

print(data)
```

Output:

```
['1001', 'Arun', 'Chennai', '25000']
```

This is a simple example of breaking a raw record into individual fields.

### split() with a Separator

```python
data = "apple,banana,orange"

fruits = data.split(",")

print(fruits)
```

Output:

```
['apple', 'banana', 'orange']
```

### split() with Different Separators

```python
data = "Chennai-Bangalore-Mumbai"

cities = data.split("-")

print(cities)
```

Output:

```
['Chennai', 'Bangalore', 'Mumbai']
```

### split() with maxsplit

We can control how many times Python splits the string.

```python
text = "Python Data Engineering Course"

result = text.split(" ", 2)

print(result)
```

Output:

```
['Python', 'Data', 'Engineering Course']
```

The `2` means Python performs at most two splits.

## 2. join()

### Definition

The `join()` method combines multiple strings into a single string using a specified separator.

It is essentially the opposite of `split()`.

### Syntax

```python
separator.join(iterable)
```

### Basic Example

```python
words = ["Python", "Data", "Engineering"]

result = " ".join(words)

print(result)
```

Output:

```
Python Data Engineering
```

### Real-World Analogy

`split()` is like separating items from a package.

`join()` is like putting those items back together using a specific separator.

```
split()

"Python Data Engineering"
          ↓
["Python", "Data", "Engineering"]


join()

["Python", "Data", "Engineering"]
          ↓
"Python Data Engineering"
```

### Joining with Comma

```python
columns = ["customer_id", "name", "city", "amount"]

result = ",".join(columns)

print(result)
```

Output:

```
customer_id,name,city,amount
```

This can be useful when constructing CSV-style data or column lists.

### Joining with Different Separators

```python
cities = ["Chennai", "Bangalore", "Mumbai"]

print("-".join(cities))
print(", ".join(cities))
print(" | ".join(cities))
```

Output:

```
Chennai-Bangalore-Mumbai
Chennai, Bangalore, Mumbai
Chennai | Bangalore | Mumbai
```

### Important

`join()` expects strings.

This will cause an error:

```python
numbers = [10, 20, 30]

result = ",".join(numbers)
```

Convert the values to strings first:

```python
numbers = [10, 20, 30]

result = ",".join(map(str, numbers))

print(result)
```

Output:

```
10,20,30
```

## 3. replace()

### Definition

The `replace()` method replaces one portion of a string with another string.

### Syntax

```python
string.replace(old, new)
```

### Basic Example

```python
text = "I like Java"

result = text.replace("Java", "Python")

print(result)
```

Output:

```
I like Python
```

### Important

Strings are immutable.

Therefore, `replace()` does not modify the original string.

```python
text = "I like Java"

result = text.replace("Java", "Python")

print(text)
print(result)
```

Output:

```
I like Java
I like Python
```

### Real-World Data Engineering Example

Suppose a dataset contains inconsistent city names.

```python
city = "Madras"

city = city.replace("Madras", "Chennai")

print(city)
```

Output:

```
Chennai
```

This can be part of a data-cleaning process.

### Replacing Multiple Occurrences

```python
text = "Python is easy. Python is powerful."

result = text.replace("Python", "Data Engineering")

print(result)
```

Output:

```
Data Engineering is easy. Data Engineering is powerful.
```

### Limiting Replacement

The third argument specifies the maximum number of replacements.

```python
text = "Python Python Python"

result = text.replace("Python", "Java", 1)

print(result)
```

Output:

```
Java Python Python
```

## 4. find()

### Definition

The `find()` method searches for a substring and returns the index of its first occurrence.

If the substring is not found, it returns:

```
-1
```

### Syntax

```python
string.find(substring)
```

### Basic Example

```python
text = "Data Engineering"

position = text.find("Engineering")

print(position)
```

Output:

```
5
```

Why?

```
D a t a   E n g i n e e r i n g
0 1 2 3 4 5 ...
```

`Engineering` starts at index `5`.

### Searching for a Character

```python
text = "Python"

print(text.find("P"))
print(text.find("t"))
```

Output:

```
0
2
```

### When Value Is Not Found

```python
text = "Python"

print(text.find("Java"))
```

Output:

```
-1
```

### Data Engineering Example

Suppose we want to check whether a file name contains a specific keyword.

```python
file_name = "customer_orders_2026.csv"

position = file_name.find("orders")

print(position)
```

Output:

```
9
```

We can use the result for further processing.

```python
if file_name.find("orders") != -1:
    print("Order file detected")
```

Output:

```
Order file detected
```

## 5. count()

### Definition

The `count()` method returns the number of times a substring occurs in a string.

### Syntax

```python
string.count(substring)
```

### Basic Example

```python
text = "Python Python Python"

count = text.count("Python")

print(count)
```

Output:

```
3
```

### Character Count

```python
text = "banana"

print(text.count("a"))
```

Output:

```
3
```

### Data Quality Example

Suppose we receive a comma-separated record.

```python
record = "1001,Arun,Chennai,25000"

print(record.count(","))
```

Output:

```
3
```

There are three commas, which means the record contains four fields.

This can be used as a simple validation technique.

```python
record = "1001,Arun,Chennai,25000"

if record.count(",") == 3:
    print("Record format looks valid")
else:
    print("Record format may be invalid")
```

Output:

```
Record format looks valid
```

## 6. strip()

### Definition

The `strip()` method removes leading and trailing whitespace from a string.

Whitespace can include spaces, tabs, and newline characters.

### Syntax

```python
string.strip()
```

### Basic Example

```python
name = "   Arun   "

clean_name = name.strip()

print(clean_name)
```

Output:

```
Arun
```

### Important

`strip()` removes whitespace from the beginning and end.

It does not remove spaces between words.

```python
name = "   Arun Kumar   "

print(name.strip())
```

Output:

```
Arun Kumar
```

### Real-World Data Engineering Example

Raw data often contains unwanted spaces.

```python
customer_name = "   Arun Kumar   "

customer_name = customer_name.strip()

print(customer_name)
```

This is a simple data-cleaning operation.

### lstrip()

`lstrip()` removes whitespace from the left side.

```python
text = "   Python"

print(text.lstrip())
```

Output:

```
Python
```

### rstrip()

`rstrip()` removes whitespace from the right side.

```python
text = "Python   "

print(text.rstrip())
```

Output:

```
Python
```

### Removing Specific Characters

`strip()` can also remove specified characters from both ends.

```python
text = "###Python###"

print(text.strip("#"))
```

Output:

```
Python
```

## 7. upper()

### Definition

The `upper()` method converts all alphabetic characters in a string to uppercase.

### Syntax

```python
string.upper()
```

### Example

```python
text = "python"

result = text.upper()

print(result)
```

Output:

```
PYTHON
```

### Data Engineering Example

Suppose department values have inconsistent capitalization.

```python
department = "it"

department = department.upper()

print(department)
```

Output:

```
IT
```

This can help standardize categorical data.

### Important

`upper()` returns a new string.

```python
text = "python"

result = text.upper()

print(text)
print(result)
```

Output:

```
python
PYTHON
```

## 8. lower()

### Definition

The `lower()` method converts all alphabetic characters in a string to lowercase.

### Syntax

```python
string.lower()
```

### Example

```python
text = "PYTHON"

result = text.lower()

print(result)
```

Output:

```
python
```

### Data Engineering Example

Suppose email addresses arrive with inconsistent capitalization.

```python
email = "USER@GMAIL.COM"

email = email.lower()

print(email)
```

Output:

```
user@gmail.com
```

Lowercasing can be useful when normalizing text before comparison or validation.

## 9. startswith()

### Definition

The `startswith()` method checks whether a string begins with a specified substring.

It returns:

```
True
```

or:

```
False
```

### Syntax

```python
string.startswith(value)
```

### Basic Example

```python
file_name = "customers.csv"

print(file_name.startswith("customer"))
```

Output:

```
True
```

Another example:

```python
file_name = "orders.csv"

print(file_name.startswith("customer"))
```

Output:

```
False
```

### Data Engineering Example

Suppose files received by a pipeline follow naming conventions.

```python
file_name = "orders_2026_08_25.csv"

if file_name.startswith("orders"):
    print("Order file detected")
```

Output:

```
Order file detected
```

### Multiple Possible Prefixes

`startswith()` can accept a tuple.

```python
file_name = "orders_2026.csv"

if file_name.startswith(("orders", "customers")):
    print("Valid input file")
```

Output:

```
Valid input file
```

## 10. endswith()

### Definition

The `endswith()` method checks whether a string ends with a specified substring.

It returns:

```
True
```

or:

```
False
```

### Syntax

```python
string.endswith(value)
```

### Basic Example

```python
file_name = "customers.csv"

print(file_name.endswith(".csv"))
```

Output:

```
True
```

Another example:

```python
file_name = "customers.json"

print(file_name.endswith(".csv"))
```

Output:

```
False
```

### Data Engineering Example

File-format validation is a common use case.

```python
file_name = "orders.parquet"

if file_name.endswith(".parquet"):
    print("Parquet file detected")
```

Output:

```
Parquet file detected
```

### Checking Multiple Extensions

```python
file_name = "customers.csv"

if file_name.endswith((".csv", ".json", ".parquet")):
    print("Supported file format")
else:
    print("Unsupported file format")
```

Output:

```
Supported file format
```

## 11. Combining String Methods

In real-world Data Engineering, multiple string methods are often used together.

Suppose we receive:

```python
raw_data = "  ARUN,CHENNAI,DATA ENGINEER  "
```

We can clean and process it:

```python
raw_data = "  ARUN,CHENNAI,DATA ENGINEER  "

clean_data = raw_data.strip()

fields = clean_data.split(",")

for field in fields:
    print(field.lower())
```

Output:

```
arun
chennai
data engineer
```

Here we used:

```
strip()
split()
lower()
```

as part of a simple data-cleaning process.

## 12. Complete Data Cleaning Example

Suppose a raw customer record arrives like this:

```
"  1001 , ARUN KUMAR , CHENNAI , arun@gmail.com  "
```

We want to clean the data.

```python
raw_record = "  1001 , ARUN KUMAR , CHENNAI , arun@gmail.com  "

fields = raw_record.split(",")

customer_id = fields[0].strip()
customer_name = fields[1].strip().title()
city = fields[2].strip().title()
email = fields[3].strip().lower()

print(f"Customer ID   : {customer_id}")
print(f"Customer Name : {customer_name}")
print(f"City          : {city}")
print(f"Email         : {email}")
```

Output:

```
Customer ID   : 1001
Customer Name : Arun Kumar
City          : Chennai
Email         : arun@gmail.com
```

This demonstrates a simple transformation step that resembles what happens during data cleaning.

## 13. Combining find() and count()

Suppose we want to check whether a record contains a particular value and how many times it occurs.

```python
record = "Python,Python,Java,Python"

if record.find("Python") != -1:
    print("Python found")

print("Python count:", record.count("Python"))
```

Output:

```
Python found
Python count: 3
```

## 14. Combining startswith() and endswith()

These methods are especially useful for validating file names.

```python
file_name = "orders_2026.csv"

if file_name.startswith("orders") and file_name.endswith(".csv"):
    print("Valid order CSV file")
else:
    print("Invalid file")
```

Output:

```
Valid order CSV file
```

This can represent a simple file-validation rule in a data pipeline.

## 15. split() and join() Together

A very common pattern is:

```
Split → Transform → Join
```

Example:

```python
text = "python,data,engineering"

words = text.split(",")

words = [word.upper() for word in words]

result = " | ".join(words)

print(result)
```

Output:

```
PYTHON | DATA | ENGINEERING
```

This pattern is extremely useful when transforming textual data.

## 16. String Method Summary

| Method         | Purpose                             | Example                             |
| -------------- | ----------------------------------- | ----------------------------------- |
| `split()`      | Breaks string into a list           | `"a,b".split(",")`                  |
| `join()`       | Combines strings                    | `",".join(["a","b"])`               |
| `replace()`    | Replaces text                       | `"Java".replace("Java","Python")`   |
| `find()`       | Finds first occurrence              | `"Python".find("t")`                |
| `count()`      | Counts occurrences                  | `"banana".count("a")`               |
| `strip()`      | Removes leading/trailing whitespace | `" hi ".strip()`                    |
| `upper()`      | Converts to uppercase               | `"python".upper()`                  |
| `lower()`      | Converts to lowercase               | `"PYTHON".lower()`                  |
| `startswith()` | Checks beginning                    | `"orders.csv".startswith("orders")` |
| `endswith()`   | Checks ending                       | `"orders.csv".endswith(".csv")`     |

## 17. Important Difference - find() vs count()

`find()` tells us the position of the first occurrence.

```python
text = "Python Python"

print(text.find("Python"))
```

Output:

```
0
```

`count()` tells us how many occurrences exist.

```python
text = "Python Python"

print(text.count("Python"))
```

Output:

```
2
```

## 18. Important Difference - split() vs join()

`split()` converts:

```
String → List
```

Example:

```python
text = "A,B,C"

result = text.split(",")

print(result)
```

Output:

```
['A', 'B', 'C']
```

`join()` converts:

```
List → String
```

Example:

```python
values = ["A", "B", "C"]

result = ",".join(values)

print(result)
```

Output:

```
A,B,C
```

## 19. Complete Example

The following example demonstrates all the methods in one Data Engineering-style workflow.

```python
# Raw file name

file_name = "  ORDERS_2026_08_25.CSV  "

# Clean whitespace

file_name = file_name.strip()

# Normalize case

file_name = file_name.lower()

# Validate file

if file_name.startswith("orders") and file_name.endswith(".csv"):

    print("Valid order file")

    # Extract file name components

    parts = file_name.replace(".csv", "").split("_")

    print("\nFile Components:")

    for part in parts:
        print(part)

    # Join components

    formatted_name = " | ".join(parts)

    print("\nFormatted Name:")
    print(formatted_name)

else:

    print("Invalid file")
```

Output:

```
Valid order file

File Components:
orders
2026
08
25

Formatted Name:
orders | 2026 | 08 | 25
```

## 20. Key Takeaways

1. `split()` breaks a string into a list of values.
2. `join()` combines multiple strings into a single string.
3. `replace()` replaces one piece of text with another.
4. `find()` returns the index of the first occurrence of a substring.
5. `find()` returns `-1` when the substring is not found.
6. `count()` returns the number of occurrences of a substring.
7. `strip()` removes whitespace from the beginning and end of a string.
8. `upper()` converts text to uppercase.
9. `lower()` converts text to lowercase.
10. `startswith()` checks whether a string begins with a specific value.
11. `endswith()` checks whether a string ends with a specific value.
12. String methods return new strings because Python strings are immutable.
13. `split()` and `join()` are commonly used together in data transformation.
14. `strip()`, `upper()`, `lower()`, and `replace()` are commonly used during data cleaning.
15. `startswith()` and `endswith()` are useful for file-name and file-format validation.
16. `find()` and `count()` are useful for searching and validating textual data.
17. These methods form an important foundation for text processing in Data Engineering.
