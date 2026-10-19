# RDD Strings

> Source notebook: [pyspark/RDD\_Strings.ipynb](https://github.com/karthikeyanm-dev/DE-Notes/blob/main/pyspark/RDD_Strings.ipynb). Notebook outputs are omitted; open the source to run cells.

## PySpark RDD --- Strings In-Depth

This guide focuses specifically on **working with strings inside PySpark RDDs**.

It covers:

* Creating String RDDs
* String transformations
* String filtering
* Splitting and flattening
* Cleaning text
* Searching and matching
* Word processing
* Word count
* Character processing
* Case conversion
* Whitespace handling
* Regular expressions
* Pair RDDs created from strings
* Practical text-processing pipelines
* Detailed parameter explanations
* Complete code examples

> **Important:** RDD itself does not have a special "String RDD" class. An RDD can contain Python strings. Therefore, normal RDD transformations such as `map()`, `filter()`, and `flatMap()` are combined with Python string methods such as `split()`, `strip()`, `lower()`, `replace()`, and `startswith()`.

***

## PART 1 --- STRING RDD BASICS

### 1. What Is a String RDD?

An RDD containing Python string objects is commonly called a String RDD.

Example:

```python
rdd = sc.parallelize([
    "Python",
    "Spark",
    "RDD",
    "Data Engineering"
])
```

Conceptually:

```
RDD
 |
 +-- "Python"
 +-- "Spark"
 +-- "RDD"
 +-- "Data Engineering"
```

Each element is a Python `str`.

You can therefore use normal Python string operations inside RDD transformations.

***

## 2. Creating a String RDD

### `parallelize()`

#### Definition

Creates an RDD from a local Python collection.

#### Signature

```python
sc.parallelize(c, numSlices=None)
```

#### Parameters

#### `c`

The local collection that will become the RDD.

Example:

```python
[
    "Python",
    "Java",
    "Spark"
]
```

#### `numSlices`

Optional number of partitions.

Example:

```python
rdd = sc.parallelize(
    ["Python", "Java", "Spark"],
    2
)
```

The data is divided across approximately two partitions.

#### Example

```python
rdd = sc.parallelize([
    "Python",
    "Java",
    "Spark"
])

print(rdd.collect())
```

Output:

```
['Python', 'Java', 'Spark']
```

***

## PART 2 --- BASIC STRING TRANSFORMATIONS

## 3. `map()` With Strings

### Definition

`map()` applies a function to every string.

#### Example: Convert to uppercase

```python
rdd = sc.parallelize([
    "python",
    "spark",
    "hadoop"
])

result = rdd.map(
    lambda text: text.upper()
)

print(result.collect())
```

Output:

```
['PYTHON', 'SPARK', 'HADOOP']
```

#### Deep Breakdown

The expression:

```python
lambda text: text.upper()
```

means:

```python
def convert_upper(text):
    return text.upper()
```

Spark conceptually performs:

```
"python" -> "PYTHON"
"spark"  -> "SPARK"
"hadoop" -> "HADOOP"
```

#### Why `map()`?

Because each input string produces exactly one transformed string.

```
1 input
   |
   v
1 output
```

***

## 4. `map()` With `lower()`

#### Example

```python
rdd = sc.parallelize([
    "Python",
    "SPARK",
    "HaDoOp"
])

result = rdd.map(
    lambda text: text.lower()
)

print(result.collect())
```

Output:

```
['python', 'spark', 'hadoop']
```

#### Practical Use

Lowercase normalization is commonly performed before:

* Word counting
* Searching
* Duplicate detection
* Text classification
* Keyword matching

For example:

```
"Python"
"python"
"PYTHON"
```

can all become:

```
"python"
```

***

## 5. `map()` With `strip()`

### Definition

`strip()` removes leading and trailing whitespace.

#### Example

```python
rdd = sc.parallelize([
    "  Python  ",
    " Spark ",
    "  Hadoop"
])

result = rdd.map(
    lambda text: text.strip()
)

print(result.collect())
```

Output:

```
['Python', 'Spark', 'Hadoop']
```

#### Important

`strip()` does not remove spaces between words.

```python
"Python   Spark".strip()
```

becomes:

```
"Python   Spark"
```

The internal spaces remain.

***

## 6. `lstrip()` and `rstrip()`

### `lstrip()`

Removes whitespace from the left side.

```python
rdd = sc.parallelize([
    "   Python",
    "   Spark"
])

result = rdd.map(
    lambda text: text.lstrip()
)

print(result.collect())
```

Output:

```
['Python', 'Spark']
```

### `rstrip()`

Removes whitespace from the right side.

```python
rdd = sc.parallelize([
    "Python   ",
    "Spark   "
])

result = rdd.map(
    lambda text: text.rstrip()
)

print(result.collect())
```

Output:

```
['Python', 'Spark']
```

***

## 7. `replace()` With String RDDs

### Definition

Replaces one substring with another.

#### Python syntax

```python
text.replace(old, new, count)
```

#### Parameters

`old`

Substring to find.

`new`

Replacement string.

`count`

Optional maximum number of replacements.

#### Example

```python
rdd = sc.parallelize([
    "Python is easy",
    "Python is powerful",
    "Python is popular"
])

result = rdd.map(
    lambda text: text.replace(
        "Python",
        "PySpark"
    )
)

print(result.collect())
```

Output:

```
[
    'PySpark is easy',
    'PySpark is powerful',
    'PySpark is popular'
]
```

***

## 8. `replace()` With `count`

```python
rdd = sc.parallelize([
    "Python Python Python"
])

result = rdd.map(
    lambda text: text.replace(
        "Python",
        "PySpark",
        2
    )
)

print(result.collect())
```

Output:

```
['PySpark PySpark Python']
```

The third parameter:

```python
2
```

means replace at most two occurrences.

***

## PART 3 --- FILTERING STRING RDDs

## 9. `filter()` With `startswith()`

### Definition

Keeps strings beginning with a specified prefix.

#### Example

```python
rdd = sc.parallelize([
    "Python",
    "Java",
    "PySpark",
    "C++",
    "Pandas"
])

result = rdd.filter(
    lambda text: text.startswith("Py")
)

print(result.collect())
```

Output:

```
['Python', 'PySpark']
```

#### Breakdown

```python
text.startswith("Py")
```

returns:

```
True
```

for:

```
Python
PySpark
```

and:

```
False
```

for:

```
Java
C++
Pandas
```

***

## 10. `filter()` With `endswith()`

```python
rdd = sc.parallelize([
    "python.py",
    "data.csv",
    "spark.py",
    "notes.txt"
])

result = rdd.filter(
    lambda text: text.endswith(".py")
)

print(result.collect())
```

Output:

```
['python.py', 'spark.py']
```

***

## 11. `filter()` With `in`

### Definition

Checks whether a substring exists inside a string.

#### Example

```python
rdd = sc.parallelize([
    "Python is easy",
    "Java is powerful",
    "Python is popular",
    "Spark is fast"
])

result = rdd.filter(
    lambda text: "Python" in text
)

print(result.collect())
```

Output:

```
[
    'Python is easy',
    'Python is popular'
]
```

***

## 12. Case-Insensitive Search

Suppose:

```
"Python is easy"
"PYTHON is powerful"
"python is popular"
```

A case-sensitive search:

```python
lambda text: "python" in text
```

will not match all of them.

Normalize first:

```python
rdd = sc.parallelize([
    "Python is easy",
    "PYTHON is powerful",
    "python is popular"
])

result = rdd.filter(
    lambda text: "python" in text.lower()
)

print(result.collect())
```

Output:

```
[
    'Python is easy',
    'PYTHON is powerful',
    'python is popular'
]
```

This is an important real-world text-processing pattern.

***

## PART 4 --- SPLITTING STRINGS

## 13. `split()`

### Definition

Splits a string into a Python list.

#### Syntax

```python
text.split(sep=None, maxsplit=-1)
```

#### Parameters

#### `sep`

Separator.

Example:

```python
"Python,Java,Spark".split(",")
```

produces:

```
['Python', 'Java', 'Spark']
```

If `sep` is omitted, Python splits on whitespace.

#### `maxsplit`

Maximum number of splits.

Example:

```python
"Python Java Spark".split(" ", 1)
```

produces:

```
['Python', 'Java Spark']
```

***

## 14. `map()` + `split()`

```python
rdd = sc.parallelize([
    "Python Spark",
    "Java Spring",
    "Hadoop Hive"
])

result = rdd.map(
    lambda text: text.split()
)

print(result.collect())
```

Output:

```
[
    ['Python', 'Spark'],
    ['Java', 'Spring'],
    ['Hadoop', 'Hive']
]
```

Notice that the output is a nested structure.

***

## 15. `flatMap()` + `split()`

For word processing, `flatMap()` is usually what we need.

```python
rdd = sc.parallelize([
    "Python Spark",
    "Java Spring",
    "Hadoop Hive"
])

result = rdd.flatMap(
    lambda text: text.split()
)

print(result.collect())
```

Output:

```
[
    'Python',
    'Spark',
    'Java',
    'Spring',
    'Hadoop',
    'Hive'
]
```

#### Why `flatMap()`?

Each input line can contain multiple words.

```
One line
   |
   +----> word 1
   +----> word 2
   +----> word 3
```

Therefore:

```
one input -> many outputs
```

`flatMap()` is ideal.

***

## PART 5 --- WORD PROCESSING

## 16. Word Count

Word count is one of the most important RDD string-processing examples.

#### Input

```python
rdd = sc.parallelize([
    "python is easy",
    "python is powerful",
    "spark is fast"
])
```

#### Step 1 --- Split lines into words

```python
words = rdd.flatMap(
    lambda line: line.split()
)
```

Result:

```
[
    'python',
    'is',
    'easy',
    'python',
    'is',
    'powerful',
    'spark',
    'is',
    'fast'
]
```

#### Step 2 --- Convert each word into a pair

```python
pairs = words.map(
    lambda word: (word, 1)
)
```

Result:

```
[
    ('python', 1),
    ('is', 1),
    ('easy', 1),
    ('python', 1),
    ...
]
```

#### Step 3 --- Aggregate

```python
counts = pairs.reduceByKey(
    lambda a, b: a + b
)
```

#### Complete code

```python
rdd = sc.parallelize([
    "python is easy",
    "python is powerful",
    "spark is fast"
])

counts = (
    rdd
    .flatMap(lambda line: line.split())
    .map(lambda word: (word, 1))
    .reduceByKey(lambda a, b: a + b)
)

print(counts.collect())
```

Possible output:

```
[
    ('python', 2),
    ('is', 3),
    ('easy', 1),
    ('powerful', 1),
    ('spark', 1),
    ('fast', 1)
]
```

***

## 17. Case-Insensitive Word Count

Without normalization:

```
Python
python
PYTHON
```

would be three separate keys.

Use:

```python
rdd = sc.parallelize([
    "Python is easy",
    "python is powerful",
    "PYTHON is popular"
])

counts = (
    rdd
    .flatMap(lambda line: line.split())
    .map(lambda word: word.lower())
    .map(lambda word: (word, 1))
    .reduceByKey(lambda a, b: a + b)
)

print(counts.collect())
```

Possible output:

```
[
    ('python', 3),
    ('is', 3),
    ('easy', 1),
    ('powerful', 1),
    ('popular', 1)
]
```

***

## 18. Removing Punctuation

Consider:

```
"Python is easy!"
"Python is powerful."
```

Without cleaning, words can become:

```
easy!
powerful.
```

A simple approach:

```python
import string

rdd = sc.parallelize([
    "Python is easy!",
    "Python is powerful.",
    "Spark is fast!"
])

cleaned = rdd.map(
    lambda line: line.translate(
        str.maketrans("", "", string.punctuation)
    )
)

print(cleaned.collect())
```

Output:

```
[
    'Python is easy',
    'Python is powerful',
    'Spark is fast'
]
```

Then:

```python
words = cleaned.flatMap(
    lambda line: line.split()
)
```

***

## PART 6 --- STRING LENGTH

## 19. Find Length of Each String

```python
rdd = sc.parallelize([
    "Python",
    "Spark",
    "Hadoop"
])

result = rdd.map(
    lambda text: len(text)
)

print(result.collect())
```

Output:

```
[6, 5, 6]
```

***

## 20. Keep String With Its Length

```python
rdd = sc.parallelize([
    "Python",
    "Spark",
    "Hadoop"
])

result = rdd.map(
    lambda text: (text, len(text))
)

print(result.collect())
```

Output:

```
[
    ('Python', 6),
    ('Spark', 5),
    ('Hadoop', 6)
]
```

***

## 21. Find Strings Longer Than N Characters

```python
rdd = sc.parallelize([
    "Python",
    "SQL",
    "Spark",
    "Hadoop",
    "AI"
])

result = rdd.filter(
    lambda text: len(text) > 4
)

print(result.collect())
```

Output:

```
['Python', 'Spark', 'Hadoop']
```

***

## PART 7 --- CHARACTER-LEVEL RDD PROCESSING

## 22. Convert Each String Into Characters

Using `flatMap()`:

```python
rdd = sc.parallelize([
    "ABC",
    "DE"
])

result = rdd.flatMap(
    lambda text: list(text)
)

print(result.collect())
```

Output:

```
['A', 'B', 'C', 'D', 'E']
```

Each string produces multiple character outputs.

***

## 23. Character Frequency

```python
rdd = sc.parallelize([
    "hello",
    "world"
])

result = (
    rdd
    .flatMap(lambda text: list(text))
    .map(lambda char: (char, 1))
    .reduceByKey(lambda a, b: a + b)
)

print(result.collect())
```

Possible output:

```
[
    ('h', 1),
    ('e', 1),
    ('l', 3),
    ('o', 2),
    ('w', 1),
    ('r', 1),
    ('d', 1)
]
```

***

## PART 8 --- STRING SEARCH

## 24. `find()` Is Not an RDD Method

Important distinction:

RDD does not provide:

```python
rdd.find(...)
```

Instead, use string operations inside transformations.

Example:

```python
rdd = sc.parallelize([
    "Python is easy",
    "Java is powerful",
    "Spark is fast"
])

result = rdd.filter(
    lambda text: text.find("Python") != -1
)

print(result.collect())
```

Output:

```
['Python is easy']
```

A simpler form is:

```python
result = rdd.filter(
    lambda text: "Python" in text
)
```

***

## 25. `startswith()` With Multiple Prefixes

Python's `startswith()` accepts a tuple.

```python
rdd = sc.parallelize([
    "Python",
    "PySpark",
    "Java",
    "JavaScript",
    "Scala"
])

result = rdd.filter(
    lambda text: text.startswith(
        ("Py", "Java")
    )
)

print(result.collect())
```

Output:

```
[
    'Python',
    'PySpark',
    'Java',
    'JavaScript'
]
```

***

## 26. `endswith()` With Multiple Extensions

```python
rdd = sc.parallelize([
    "data.csv",
    "image.png",
    "program.py",
    "script.py",
    "notes.txt"
])

result = rdd.filter(
    lambda text: text.endswith(
        (".py", ".csv")
    )
)

print(result.collect())
```

Output:

```
[
    'data.csv',
    'program.py',
    'script.py'
]
```

***

## PART 9 --- STRING PARSING

## 27. Parsing CSV-Like Data

Suppose each RDD element is:

```
name,age,department
```

Example:

```python
rdd = sc.parallelize([
    "Arun,25,IT",
    "Priya,28,HR",
    "Karthik,30,IT"
])
```

Split each record:

```python
result = rdd.map(
    lambda line: line.split(",")
)

print(result.collect())
```

Output:

```
[
    ['Arun', '25', 'IT'],
    ['Priya', '28', 'HR'],
    ['Karthik', '30', 'IT']
]
```

***

## 28. Convert Parsed Fields to Proper Types

The values returned by `split()` are strings.

For example:

```
"25"
```

is a string, not an integer.

Use:

```python
result = rdd.map(
    lambda line: (
        line.split(",")[0],
        int(line.split(",")[1]),
        line.split(",")[2]
    )
)

print(result.collect())
```

Output:

```
[
    ('Arun', 25, 'IT'),
    ('Priya', 28, 'HR'),
    ('Karthik', 30, 'IT')
]
```

A cleaner approach is:

```python
def parse_employee(line):
    name, age, department = line.split(",")

    return (
        name,
        int(age),
        department
    )

result = rdd.map(parse_employee)

print(result.collect())
```

***

## 29. Filter Parsed Records

Find employees from IT:

```python
result = (
    rdd
    .map(lambda line: line.split(","))
    .filter(lambda fields: fields[2] == "IT")
)

print(result.collect())
```

Output:

```
[
    ['Arun', '25', 'IT'],
    ['Karthik', '30', 'IT']
]
```

***

## PART 10 --- REGULAR EXPRESSIONS WITH STRING RDDs

## 30. Why Regex?

Normal string methods are sufficient for simple patterns.

Regex is useful when you need patterns such as:

```
Email addresses
Phone numbers
URLs
IDs
Log formats
Multiple delimiters
```

Python's `re` module can be used inside RDD transformations.

***

## 31. Regex Search

```python
import re

rdd = sc.parallelize([
    "Contact: admin@example.com",
    "No email here",
    "Send to user@gmail.com"
])

pattern = re.compile(
    r'[\w\.-]+@[\w\.-]+\.\w+'
)

result = rdd.filter(
    lambda text: pattern.search(text) is not None
)

print(result.collect())
```

Output:

```
[
    'Contact: admin@example.com',
    'Send to user@gmail.com'
]
```

***

## 32. Extract Emails

Instead of filtering, extract the actual email addresses.

```python
import re

rdd = sc.parallelize([
    "Contact admin@example.com",
    "Send user@gmail.com",
    "No email"
])

pattern = re.compile(
    r'[\w\.-]+@[\w\.-]+\.\w+'
)

result = rdd.flatMap(
    lambda text: pattern.findall(text)
)

print(result.collect())
```

Output:

```
[
    'admin@example.com',
    'user@gmail.com'
]
```

#### Why `flatMap()`?

One line can contain:

```
0 emails
1 email
2 emails
...
```

Therefore the transformation can produce multiple outputs per input line.

***

## PART 11 --- DUPLICATE STRING PROCESSING

## 33. `distinct()` With Strings

```python
rdd = sc.parallelize([
    "Python",
    "Java",
    "Python",
    "Spark",
    "Java"
])

result = rdd.distinct()

print(result.collect())
```

Possible output:

```
[
    'Python',
    'Java',
    'Spark'
]
```

The ordering should not be relied upon.

***

## 34. Case-Insensitive Distinct

If:

```
Python
python
PYTHON
```

should count as one value:

```python
rdd = sc.parallelize([
    "Python",
    "python",
    "PYTHON",
    "Java"
])

result = (
    rdd
    .map(lambda text: text.lower())
    .distinct()
)

print(result.collect())
```

Output:

```
[
    'python',
    'java'
]
```

***

## PART 12 --- STRING SORTING

## 35. `sortBy()`

```python
rdd = sc.parallelize([
    "Python",
    "Java",
    "Spark",
    "Hadoop"
])

result = rdd.sortBy(
    lambda text: text
)

print(result.collect())
```

Output:

```
[
    'Hadoop',
    'Java',
    'Python',
    'Spark'
]
```

***

## 36. Sort by String Length

```python
rdd = sc.parallelize([
    "Python",
    "AI",
    "Spark",
    "Hadoop"
])

result = rdd.sortBy(
    lambda text: len(text)
)

print(result.collect())
```

Possible output:

```
[
    'AI',
    'Spark',
    'Python',
    'Hadoop'
]
```

***

## 37. Sort by Length Descending

```python
result = rdd.sortBy(
    lambda text: len(text),
    ascending=False
)

print(result.collect())
```

Possible output:

```
[
    'Hadoop',
    'Python',
    'Spark',
    'AI'
]
```

***

## PART 13 --- STRING PAIR RDDs

## 38. Convert Strings Into Key-Value Pairs

Suppose:

```
Python
PySpark
Java
Spark
```

We want the first character as key.

```python
rdd = sc.parallelize([
    "Python",
    "PySpark",
    "Java",
    "Spark"
])

result = rdd.map(
    lambda text: (text[0], text)
)

print(result.collect())
```

Output:

```
[
    ('P', 'Python'),
    ('P', 'PySpark'),
    ('J', 'Java'),
    ('S', 'Spark')
]
```

***

## 39. Group Strings by First Character

```python
rdd = sc.parallelize([
    "Python",
    "PySpark",
    "Java",
    "JavaScript",
    "Spark"
])

result = (
    rdd
    .map(lambda text: (text[0], text))
    .groupByKey()
    .mapValues(list)
)

print(result.collect())
```

Possible output:

```
[
    ('P', ['Python', 'PySpark']),
    ('J', ['Java', 'JavaScript']),
    ('S', ['Spark'])
]
```

***

## 40. Count Strings by First Character

```python
rdd = sc.parallelize([
    "Python",
    "PySpark",
    "Pandas",
    "Java",
    "JavaScript",
    "Spark"
])

result = (
    rdd
    .map(lambda text: (text[0], 1))
    .reduceByKey(lambda a, b: a + b)
)

print(result.collect())
```

Output:

```
[
    ('P', 3),
    ('J', 2),
    ('S', 1)
]
```

***

## PART 14 --- PRACTICAL STRING RDD PROJECTS

## 41. Find Python-Related Records

```python
rdd = sc.parallelize([
    "Python is easy to learn",
    "Java is widely used",
    "PySpark uses Python",
    "C++ is powerful",
    "Python is popular in data science"
])

result = rdd.filter(
    lambda line: "python" in line.lower()
)

print(result.collect())
```

Output:

```
[
    'Python is easy to learn',
    'PySpark uses Python',
    'Python is popular in data science'
]
```

***

## 42. Find Lines Containing a Number

```python
import re

rdd = sc.parallelize([
    "User 101 logged in",
    "System started",
    "User 202 logged out",
    "No activity"
])

result = rdd.filter(
    lambda line: re.search(r'\d+', line) is not None
)

print(result.collect())
```

Output:

```
[
    'User 101 logged in',
    'User 202 logged out'
]
```

***

## 43. Extract Numbers From Strings

```python
import re

rdd = sc.parallelize([
    "User 101 logged in",
    "Order 500 created",
    "No number here"
])

result = rdd.flatMap(
    lambda line: re.findall(
        r'\d+',
        line
    )
)

print(result.collect())
```

Output:

```
['101', '500']
```

If integers are required:

```python
result = rdd.flatMap(
    lambda line: [
        int(x)
        for x in re.findall(r'\d+', line)
    ]
)

print(result.collect())
```

Output:

```
[101, 500]
```

***

## 44. Log Processing Example

Suppose logs are:

```python
logs = sc.parallelize([
    "INFO User logged in",
    "ERROR Database connection failed",
    "INFO Request completed",
    "ERROR Timeout occurred",
    "WARNING CPU usage high"
])
```

### Find errors

```python
errors = logs.filter(
    lambda line: line.startswith("ERROR")
)

print(errors.collect())
```

Output:

```
[
    'ERROR Database connection failed',
    'ERROR Timeout occurred'
]
```

### Count log levels

```python
counts = (
    logs
    .map(lambda line: line.split()[0])
    .map(lambda level: (level, 1))
    .reduceByKey(lambda a, b: a + b)
)

print(counts.collect())
```

Possible output:

```
[
    ('INFO', 2),
    ('ERROR', 2),
    ('WARNING', 1)
]
```

***

## 45. Find the Longest String

```python
rdd = sc.parallelize([
    "Python",
    "PySpark",
    "Data Engineering",
    "AI"
])

result = rdd.max(
    key=lambda text: len(text)
)

print(result)
```

Output:

```
Data Engineering
```

The `key` function tells Spark how to compare the strings.

***

## 46. Find the Shortest String

```python
rdd = sc.parallelize([
    "Python",
    "PySpark",
    "Data Engineering",
    "AI"
])

result = rdd.min(
    key=lambda text: len(text)
)

print(result)
```

Output:

```
AI
```

***

## 47. Total Number of Characters

```python
rdd = sc.parallelize([
    "Python",
    "Spark",
    "RDD"
])

result = rdd.map(
    lambda text: len(text)
).sum()

print(result)
```

Output:

```
14
```

Calculation:

```
Python = 6
Spark  = 5
RDD    = 3

6 + 5 + 3 = 14
```

***

## 48. Average String Length

```python
rdd = sc.parallelize([
    "Python",
    "Spark",
    "RDD"
])

lengths = rdd.map(
    lambda text: len(text)
)

average = lengths.mean()

print(average)
```

Output:

```
4.666666666666667
```

***

## 49. Complete Text Cleaning Pipeline

A realistic text-processing pipeline might be:

```python
import string

rdd = sc.parallelize([
    "Python, is powerful!",
    "Python is EASY.",
    "Spark is FAST!",
    "Python and Spark are popular."
])

result = (
    rdd
    .map(lambda line: line.lower())
    .map(
        lambda line: line.translate(
            str.maketrans("", "", string.punctuation)
        )
    )
    .flatMap(lambda line: line.split())
    .filter(lambda word: len(word) > 2)
    .map(lambda word: (word, 1))
    .reduceByKey(lambda a, b: a + b)
)

print(result.collect())
```

Pipeline:

```
Raw Text
   |
   v
lower()
   |
   v
Remove punctuation
   |
   v
split()
   |
   v
flatMap()
   |
   v
Remove short words
   |
   v
(word, 1)
   |
   v
reduceByKey()
   |
   v
Word frequencies
```

***

## PART 15 --- IMPORTANT STRING METHODS TO KNOW

When processing strings inside RDDs, these Python methods are particularly useful.

Method Purpose

***

`lower()` Convert to lowercase `upper()` Convert to uppercase `capitalize()` Capitalize first character `title()` Capitalize words `strip()` Remove leading/trailing whitespace `lstrip()` Remove left whitespace `rstrip()` Remove right whitespace `split()` Split into list `rsplit()` Split from right `splitlines()` Split lines `replace()` Replace substring `startswith()` Test prefix `endswith()` Test suffix `find()` Find substring position `count()` Count substring occurrences `join()` Combine strings `isdigit()` Check digits `isalpha()` Check alphabetic characters `isalnum()` Check letters/numbers `isspace()` Check whitespace `islower()` Check lowercase `isupper()` Check uppercase

***

## PART 16 --- IMPORTANT PATTERNS

### Pattern 1 --- Clean every string

```python
result = rdd.map(
    lambda x: x.strip().lower()
)
```

***

### Pattern 2 --- Filter strings

```python
result = rdd.filter(
    lambda x: "python" in x.lower()
)
```

***

### Pattern 3 --- Split every line

```python
result = rdd.flatMap(
    lambda x: x.split()
)
```

***

### Pattern 4 --- Convert words to Pair RDD

```python
result = rdd.map(
    lambda word: (word, 1)
)
```

***

### Pattern 5 --- Count words

```python
result = (
    rdd
    .flatMap(lambda line: line.split())
    .map(lambda word: (word, 1))
    .reduceByKey(lambda a, b: a + b)
)
```

***

### Pattern 6 --- Case-insensitive word count

```python
result = (
    rdd
    .flatMap(lambda line: line.lower().split())
    .map(lambda word: (word, 1))
    .reduceByKey(lambda a, b: a + b)
)
```

***

### Pattern 7 --- Extract using regex

```python
import re

result = rdd.flatMap(
    lambda line: re.findall(
        r'\d+',
        line
    )
)
```

***

## PART 17 --- COMMON MISTAKES

### Mistake 1 --- Using `map()` when `flatMap()` is required

Wrong for word extraction:

```python
result = rdd.map(
    lambda line: line.split()
)
```

Output:

```
[
    ['Python', 'Spark'],
    ['RDD', 'is', 'fast']
]
```

If you need individual words:

```python
result = rdd.flatMap(
    lambda line: line.split()
)
```

***

### Mistake 2 --- Forgetting Case Normalization

This:

```python
rdd.filter(
    lambda x: "python" in x
)
```

will not match:

```
Python
PYTHON
```

Better:

```python
rdd.filter(
    lambda x: "python" in x.lower()
)
```

***

### Mistake 3 --- Forgetting That `split()` Returns Strings

```python
line = "Arun,25,IT"

fields = line.split(",")

print(type(fields[1]))
```

Output:

```
<class 'str'>
```

So:

```python
int(fields[1])
```

is required when numerical operations are needed.

***

### Mistake 4 --- Collecting Huge Text RDDs

Avoid:

```python
print(huge_rdd.collect())
```

Prefer:

```python
print(huge_rdd.take(10))
```

for inspection.

***

### Mistake 5 --- Using `foreach(print)` for Driver-Side Debugging

```python
rdd.foreach(
    lambda x: print(x)
)
```

runs on worker processes.

For debugging:

```python
print(rdd.take(10))
```

is generally clearer.

***

## PART 18 --- COMPLETE STRING RDD EXAMPLE

The following example demonstrates a complete text-processing workflow.

```python
from pyspark import SparkContext
import string

sc = SparkContext(
    master="local[*]",
    appName="StringRDDExample"
)

lines = sc.parallelize([
    "Python is powerful!",
    "Python is easy to learn.",
    "Spark is fast!",
    "Spark is powerful.",
    "Python and Spark are popular."
])

word_counts = (
    lines

    # Convert everything to lowercase
    .map(lambda line: line.lower())

    # Remove punctuation
    .map(
        lambda line: line.translate(
            str.maketrans(
                "",
                "",
                string.punctuation
            )
        )
    )

    # Convert each line into individual words
    .flatMap(
        lambda line: line.split()
    )

    # Remove very short words
    .filter(
        lambda word: len(word) > 2
    )

    # Create Pair RDD
    .map(
        lambda word: (word, 1)
    )

    # Count each word
    .reduceByKey(
        lambda a, b: a + b
    )
)

print(word_counts.collect())

sc.stop()
```

Possible output:

```
[
    ('python', 3),
    ('powerful', 2),
    ('spark', 3),
    ('easy', 1),
    ('learn', 1),
    ('fast', 1),
    ('and', 1),
    ('are', 1),
    ('popular', 1)
]
```

***

## PART 19 --- STRING RDD MENTAL MODEL

The most important thing is to understand the layers.

```
                RDD
                 |
                 v
        Python String
                 |
        +--------+--------+
        |        |        |
        v        v        v
      map()   filter() flatMap()
        |        |        |
        +--------+--------+
                 |
                 v
         String Processing
                 |
       +---------+---------+
       |         |         |
       v         v         v
     split()   lower()   replace()
       |
       v
     Words
       |
       v
  (word, 1)
       |
       v
 reduceByKey()
       |
       v
 Word Counts
       |
       v
   collect()
```

***

## PART 20 --- FINAL CHEAT SHEET

### String → String

```python
rdd.map(lambda x: x.lower())
```

```python
rdd.map(lambda x: x.upper())
```

```python
rdd.map(lambda x: x.strip())
```

```python
rdd.map(lambda x: x.replace("old", "new"))
```

***

### Filter Strings

```python
rdd.filter(lambda x: x.startswith("Py"))
```

```python
rdd.filter(lambda x: x.endswith(".py"))
```

```python
rdd.filter(lambda x: "python" in x.lower())
```

```python
rdd.filter(lambda x: len(x) > 10)
```

***

### String → Multiple Strings

```python
rdd.flatMap(lambda x: x.split())
```

```python
rdd.flatMap(lambda x: list(x))
```

```python
rdd.flatMap(lambda x: x.split(","))
```

***

### String → Pair RDD

```python
rdd.map(lambda x: (x, 1))
```

```python
rdd.map(lambda x: (x[0], x))
```

***

### Pair RDD → Aggregation

```python
rdd.reduceByKey(lambda a, b: a + b)
```

***

### String Statistics

```python
rdd.map(lambda x: len(x)).sum()
```

```python
rdd.map(lambda x: len(x)).mean()
```

```python
rdd.max(key=lambda x: len(x))
```

```python
rdd.min(key=lambda x: len(x))
```

***

## FINAL TAKEAWAY

A String RDD is simply an RDD whose elements are Python strings.

The most important pattern is:

```
Raw strings
     |
     v
Clean
     |
     v
Normalize
     |
     v
Split
     |
     v
flatMap
     |
     v
Filter
     |
     v
Transform into (key, value)
     |
     v
reduceByKey / groupByKey
     |
     v
Action
```

For practical PySpark text processing, master these first:

```python
map()
filter()
flatMap()
split()
strip()
lower()
upper()
replace()
startswith()
endswith()
```

Then combine them with:

```python
map(lambda x: (x, 1))
reduceByKey(lambda a, b: a + b)
```

This combination forms the foundation of many basic RDD text-processing applications.
