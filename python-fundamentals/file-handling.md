# Page

## File Handeling

> Imported from [`python/File_Handeling.ipynb`](https://github.com/karthikeyanm-dev/DE-Notes/blob/main/python/File_Handeling.ipynb). Code outputs are omitted; run the source notebook for live results.

## File Handling in Python

File handling is one of the most important topics in Python, especially for **Data Engineering**.

As a Data Engineer, you will frequently work with:

* CSV files
* JSON files
* Text files
* Log files
* Configuration files
* XML files
* Parquet files
* Excel files
* Binary files
* Temporary files

A typical Data Engineering workflow may look like:

```
Source System
     ↓
File
     ↓
Read
     ↓
Validate
     ↓
Clean
     ↓
Transform
     ↓
Write
     ↓
Output File
```

***

## 1. What is a File?

A **file** is a collection of data stored permanently on a storage device with a specific name and format.

For example:

```
customers.csv
orders.csv
employees.txt
config.json
sales.xlsx
data.parquet
```

A file allows data to remain available even after a Python program stops running.

***

## 2. Why Do We Need Files?

Consider this Python program:

```python
name = "Karthi"
age = 30
```

The values are stored in memory while the program is running.

When the program ends, those variables are gone.

A file provides **persistent storage**.

```
Python Program
      ↓
     Data
      ↓
    File
      ↓
Stored permanently
```

For example:

```
customers.csv
```

can contain:

```
id,name,city
101,Karthi,Chennai
102,Ravi,Bangalore
103,Arun,Coimbatore
```

The data remains in the file even after Python exits.

***

## 3. File vs Variable

| Variable                           | File                       |
| ---------------------------------- | -------------------------- |
| Stored in memory                   | Stored on storage          |
| Temporary during program execution | Persistent                 |
| Lost when program ends             | Remains after program ends |
| Fast to access                     | Usually slower than memory |
| Suitable for temporary processing  | Suitable for storing data  |

***

## 4. File Types

Files can contain different types of data.

### Text Files

```
.txt
.csv
.log
.json
.xml
```

Example:

```
employees.txt
```

***

### Binary Files

Binary files store data in binary format.

Examples:

```
.jpg
.png
.pdf
.mp3
.mp4
.exe
```

Python can also work with binary files.

***

### Data Engineering File Formats

Common formats include:

```
CSV
JSON
Parquet
Avro
ORC
XML
Excel
```

We will first learn **Python's built-in file handling**, and later move toward CSV, JSON, and other formats.

***

## 5. File Handling in Python

Python provides the built-in `open()` function for working with files.

Basic syntax:

```python
open(filename, mode)
```

Example:

```python
file = open("data.txt", "r")
```

Here:

```
data.txt → file name
r        → mode
```

***

## 6. File Handling Basic Flow

The basic process is:

```
Open
  ↓
Read / Write
  ↓
Close
```

Example:

```python
file = open("data.txt", "r")

data = file.read()

print(data)

file.close()
```

***

## 7. Why Should We Close a File?

When Python opens a file, the operating system allocates resources for that file.

After finishing the work, we should close it.

```python
file.close()
```

Otherwise, we may encounter:

* Resource usage
* File locking issues
* Data not being flushed properly
* Too many open files in large applications

***

## 8. File Modes

The `mode` tells Python **what we want to do with the file**.

The most important modes are:

```
r
w
a
x
```

And their binary versions:

```
rb
wb
ab
xb
```

There is also:

```
r+
w+
a+
```

***

## 9. File Mode Cheat Sheet

| Mode | Meaning            |
| ---- | ------------------ |
| `r`  | Read               |
| `w`  | Write              |
| `a`  | Append             |
| `x`  | Create new file    |
| `r+` | Read + Write       |
| `w+` | Write + Read       |
| `a+` | Append + Read      |
| `rb` | Read binary        |
| `wb` | Write binary       |
| `ab` | Append binary      |
| `xb` | Create binary file |

***

## 10. `r` — Read Mode

`r` means **read**.

```python
file = open("data.txt", "r")
```

It is used when we want to read existing data.

Example:

```python
file = open("data.txt", "r")

data = file.read()

print(data)

file.close()
```

Important:

The file should already exist.

If the file does not exist:

```
FileNotFoundError
```

will occur.

***

## 11. `r` is the Default Mode

If no mode is provided:

```python
open("data.txt")
```

Python assumes:

```python
open("data.txt", "r")
```

So these are equivalent:

```python
open("data.txt")
```

and:

```python
open("data.txt", "r")
```

***

## 12. `w` — Write Mode

`w` means **write**.

```python
file = open("data.txt", "w")
```

It allows us to write data into a file.

Example:

```python
file = open("data.txt", "w")

file.write("Hello Python")

file.close()
```

After execution:

```
data.txt
```

contains:

```
Hello Python
```

***

## 13. Important Warning About `w`

The biggest thing to remember:

> `w` can overwrite existing content.

Suppose:

```
data.txt
```

contains:

```
Hello
Python
Data Engineering
```

Now:

```python
file = open("data.txt", "w")

file.write("New Data")

file.close()
```

The old content is replaced.

The file now contains:

```
New Data
```

So:

```
w → overwrite
```

***

## 14. `w` If the File Does Not Exist

If the file doesn't exist:

```python
file = open("new_file.txt", "w")
```

Python creates it.

So:

```
w
```

means:

```
Existing file → overwrite

Non-existing file → create
```

***

## 15. `a` — Append Mode

`a` means **append**.

It adds new data to the end of the existing file.

Example:

```python
file = open("log.txt", "a")

file.write("New log entry\n")

file.close()
```

If the file contains:

```
Log 1
Log 2
```

after execution:

```
Log 1
Log 2
New log entry
```

***

## 16. `a` If the File Does Not Exist

If the file does not exist:

```python
file = open("log.txt", "a")
```

Python creates it.

So:

```
a
```

means:

```
Existing file → add to end

Non-existing file → create
```

***

## 17. `x` — Exclusive Creation Mode

`x` means:

> Create a new file.

Example:

```python
file = open("new_data.txt", "x")

file.write("Hello")

file.close()
```

If the file does not exist, it is created.

But if it already exists:

```
FileExistsError
```

occurs.

This is useful when we specifically want to make sure we **do not accidentally overwrite an existing file**.

***

## 18. `r` vs `w` vs `a` vs `x`

| Mode | Existing file | Missing file | Main purpose |
| ---- | ------------- | ------------ | ------------ |
| `r`  | Read          | Error        | Read         |
| `w`  | Overwrite     | Create       | Write        |
| `a`  | Append        | Create       | Add data     |
| `x`  | Error         | Create       | Create only  |

This table is extremely important.

***

## 19. `+` Modes

The `+` symbol means:

```
Read + Write
```

Examples:

```
r+
w+
a+
```

***

## 20. `r+` — Read and Write

```python
file = open("data.txt", "r+")
```

Allows:

```
Read
Write
```

The file must already exist.

Example:

```python
file = open("data.txt", "r+")

data = file.read()

print(data)

file.write("\nNew Data")

file.close()
```

Important:

`r+` does **not** automatically erase the existing content.

***

## 21. `w+` — Write and Read

```python
file = open("data.txt", "w+")
```

Allows:

```
Write
Read
```

But remember:

```
w+ → existing content is overwritten
```

Example:

```python
file = open("data.txt", "w+")

file.write("Hello Python")

file.seek(0)

print(file.read())

file.close()
```

Output:

```
Hello Python
```

***

## 22. Why Did We Use `seek(0)`?

After:

```python
file.write("Hello Python")
```

the file cursor is at the end.

So:

```python
file.read()
```

would have nothing left to read.

We move the cursor back to the beginning:

```python
file.seek(0)
```

Then:

```python
file.read()
```

can read the content.

***

## 23. `a+` — Append and Read

```python
file = open("data.txt", "a+")
```

Allows:

```
Append
Read
```

Example:

```python
file = open("data.txt", "a+")

file.write("\nNew Record")

file.seek(0)

print(file.read())

file.close()
```

The new data is added to the end.

***

## 24. `r+` vs `w+` vs `a+`

| Mode | Read | Write | Existing content   |
| ---- | ---- | ----- | ------------------ |
| `r+` | Yes  | Yes   | Preserved          |
| `w+` | Yes  | Yes   | Overwritten        |
| `a+` | Yes  | Yes   | Preserved + append |

***

## 25. Binary Modes

Sometimes files don't contain normal text.

Examples:

```
image
PDF
audio
video
binary data
```

For these, we use:

```
b
```

Example:

```python
open("image.jpg", "rb")
```

Here:

```
r → read
b → binary
```

***

## 26. `rb` — Read Binary

```python
file = open("image.jpg", "rb")

data = file.read()

print(data)

file.close()
```

Output may look like:

```
b'\xff\xd8\xff\xe0...'
```

The `b` indicates binary data.

***

## 27. `wb` — Write Binary

```python
file = open("output.bin", "wb")

file.write(b"Hello")

file.close()
```

Notice:

```python
b"Hello"
```

This is a bytes object.

***

## 28. `ab` — Append Binary

```python
file = open("data.bin", "ab")

file.write(b"New Data")

file.close()
```

This appends binary data.

***

## 29. Text vs Binary

| Text                    | Binary                    |
| ----------------------- | ------------------------- |
| `r`                     | `rb`                      |
| `w`                     | `wb`                      |
| `a`                     | `ab`                      |
| Works with strings      | Works with bytes          |
| `.txt`, `.csv`, `.json` | Images, PDFs, audio, etc. |

***

## 30. Reading a File

Python provides several methods for reading.

The most important are:

```
read()
readline()
readlines()
```

***

## 31. `read()`

`read()` reads the entire file.

Suppose:

```
data.txt
```

contains:

```
Python
SQL
Spark
```

Code:

```python
file = open("data.txt", "r")

data = file.read()

print(data)

file.close()
```

Output:

```
Python
SQL
Spark
```

***

## 32. `read(n)`

We can specify how many characters to read.

```python
file = open("data.txt", "r")

data = file.read(6)

print(data)

file.close()
```

If the file begins with:

```
Python
SQL
Spark
```

Output:

```
Python
```

***

## 33. `readline()`

`readline()` reads one line at a time.

```python
file = open("data.txt", "r")

print(file.readline())
print(file.readline())

file.close()
```

If the file contains:

```
Python
SQL
Spark
```

Output:

```
Python

SQL
```

***

## 34. `readlines()`

`readlines()` reads all lines and returns them as a list.

```python
file = open("data.txt", "r")

lines = file.readlines()

print(lines)

file.close()
```

Output:

```
['Python\n', 'SQL\n', 'Spark\n']
```

Notice the:

```
\n
```

representing the newline character.

***

## 35. `read()` vs `readline()` vs `readlines()`

| Method        | Result                   |
| ------------- | ------------------------ |
| `read()`      | Entire content as string |
| `read(n)`     | n characters             |
| `readline()`  | One line                 |
| `readlines()` | List of all lines        |

***

## 36. Reading Large Files

Suppose a log file has:

```
10 GB
```

Using:

```python
file.read()
```

may attempt to load the entire file into memory.

That is not ideal.

Instead, we can process the file line by line.

```python
file = open("large_log.txt", "r")

for line in file:
    print(line.strip())

file.close()
```

This is much more suitable for large files.

***

## 37. Writing to a File

The most common method is:

```python
write()
```

Example:

```python
file = open("data.txt", "w")

file.write("Python")

file.close()
```

***

## 38. `write()` Returns Number of Characters

```python
file = open("data.txt", "w")

result = file.write("Python")

print(result)

file.close()
```

Output:

```
6
```

Because:

```
Python
```

contains 6 characters.

***

## 39. Writing Multiple Lines

```python
file = open("data.txt", "w")

file.write("Python\n")
file.write("SQL\n")
file.write("Spark\n")

file.close()
```

File content:

```
Python
SQL
Spark
```

***

## 40. `writelines()`

`writelines()` writes multiple strings.

```python
lines = [
    "Python\n",
    "SQL\n",
    "Spark\n"
]

file = open("data.txt", "w")

file.writelines(lines)

file.close()
```

File:

```
Python
SQL
Spark
```

Important:

`writelines()` does **not automatically add `\n`**.

This:

```python
lines = ["Python", "SQL", "Spark"]
```

would produce:

```
PythonSQLSpark
```

So we usually include newline characters ourselves.

***

## 41. File Cursor

When Python works with a file, it maintains a **file cursor**.

Think of it like a reading pointer.

```
Python SQL Spark
^
cursor
```

After reading `Python`:

```
Python SQL Spark
      ^
    cursor
```

The cursor moves as we read or write.

***

## 42. `tell()`

`tell()` tells us the current cursor position.

```python
file = open("data.txt", "r")

print(file.tell())

file.read(6)

print(file.tell())

file.close()
```

Possible output:

```
0
6
```

The cursor initially starts at:

```
0
```

***

## 43. `seek()`

`seek()` moves the cursor.

Syntax:

```python
file.seek(position)
```

Example:

```python
file = open("data.txt", "r")

print(file.read(6))

file.seek(0)

print(file.read(6))

file.close()
```

Output:

```
Python
Python
```

***

## 44. `seek()` in Data Processing

Sometimes we need to:

```
Read
↓
Move cursor
↓
Read again
```

Example:

```python
file.seek(0)
```

moves the cursor to the beginning.

***

## 45. `with open()` — Recommended Way

Instead of:

```python
file = open("data.txt", "r")

data = file.read()

file.close()
```

Python provides a better approach:

```python
with open("data.txt", "r") as file:
    data = file.read()

print(data)
```

This is called a **context manager**.

***

## 46. Why Use `with open()`?

The biggest advantage:

> Python automatically closes the file.

Example:

```python
with open("data.txt", "r") as file:
    data = file.read()
```

After the `with` block finishes:

```
file → automatically closed
```

This is the preferred approach in modern Python.

***

## 47. Checking Whether a File is Closed

```python
with open("data.txt", "r") as file:
    print(file.closed)

print(file.closed)
```

Output:

```
False
True
```

Inside the block:

```
False
```

After leaving the block:

```
True
```

***

## 48. Reading with `with`

```python
with open("data.txt", "r") as file:
    data = file.read()

print(data)
```

***

## 49. Writing with `with`

```python
with open("data.txt", "w") as file:
    file.write("Hello Python")
```

***

## 50. Appending with `with`

```python
with open("log.txt", "a") as file:
    file.write("New log entry\n")
```

***

## 51. Encoding

Text files use character encoding.

A common encoding is:

```
UTF-8
```

It is good practice to specify it when appropriate.

```python
with open("data.txt", "r", encoding="utf-8") as file:
    data = file.read()
```

Writing:

```python
with open("data.txt", "w", encoding="utf-8") as file:
    file.write("Hello Python")
```

***

## 52. Why Encoding Matters

Suppose a file contains:

```
தமிழ்
Python
Data Engineering
```

Using the correct encoding ensures the characters are read correctly.

For modern data-processing workflows, UTF-8 is commonly used.

***

## 53. Newline Character

The newline character is:

```
\n
```

Example:

```python
text = "Python\nSQL\nSpark"

print(text)
```

Output:

```
Python
SQL
Spark
```

***

## 54. `strip()` While Reading Files

When reading line by line, lines often contain:

```
\n
```

Example:

```python
with open("data.txt", "r") as file:
    for line in file:
        print(line.strip())
```

`strip()` removes leading/trailing whitespace, including the newline.

***

## 55. Processing a File Line by Line

Suppose:

```
orders.txt
```

contains:

```
ORD1001
ORD1002
ORD1003
```

We can process:

```python
with open("orders.txt", "r") as file:
    for line in file:
        order_id = line.strip()
        print("Processing:", order_id)
```

Output:

```
Processing: ORD1001
Processing: ORD1002
Processing: ORD1003
```

This is a simple Data Engineering-style file processing operation.

***

## 56. File Paths

A file can be located in:

```
Current directory
Subdirectory
Another directory
Absolute path
```

Example:

```python
open("data/orders.csv")
```

Here:

```
data/
    orders.csv
```

***

## 57. Relative Path

A relative path is based on the current working directory.

```python
open("data/orders.csv")
```

means:

```
current_directory/data/orders.csv
```

***

## 58. Absolute Path

An absolute path specifies the complete location.

Windows example:

```python
open(r"C:\Users\User\data\orders.csv")
```

Linux/macOS example:

```python
open("/home/user/data/orders.csv")
```

The `r` before the Windows path helps prevent backslash escape issues.

***

## 59. `os` Module

Python's `os` module can work with files and directories.

```python
import os
```

Check whether a file exists:

```python
print(os.path.exists("data.txt"))
```

Output:

```
True
```

or:

```
False
```

***

## 60. Check Whether It Is a File

```python
import os

print(os.path.isfile("data.txt"))
```

***

## 61. Check Whether It Is a Directory

```python
import os

print(os.path.isdir("data"))
```

***

## 62. Creating a Directory

```python
import os

os.mkdir("data")
```

This creates:

```
data/
```

***

## 63. `os.makedirs()`

For nested directories:

```python
import os

os.makedirs("data/raw/orders", exist_ok=True)
```

This can create:

```
data/
    raw/
        orders/
```

`exist_ok=True` prevents an error if the directory already exists.

***

## 64. Listing Files

```python
import os

files = os.listdir("data")

print(files)
```

Possible output:

```
['orders.csv', 'customers.csv', 'employees.csv']
```

***

## 65. `pathlib` — Modern Path Handling

Python also provides:

```python
from pathlib import Path
```

Example:

```python
path = Path("data/orders.csv")

print(path.exists())
```

Output:

```
True
```

***

## 66. `pathlib` Examples

```python
from pathlib import Path

path = Path("data/orders.csv")

print(path.name)
print(path.parent)
print(path.suffix)
```

Output:

```
orders.csv
data
.csv
```

***

## 67. Finding Files with `pathlib`

```python
from pathlib import Path

data_path = Path("data")

for file in data_path.glob("*.csv"):
    print(file)
```

Possible output:

```
data/orders.csv
data/customers.csv
data/products.csv
```

This is very useful in Data Engineering pipelines.

***

## 68. File Handling + Regex

File handling and Regex can work together.

Suppose a log file contains:

```
Order ORD1001 processed
Order ORD1002 processed
Order ORD1003 failed
```

We can read the file and extract Order IDs.

```python
import re

with open("orders.log", "r") as file:
    data = file.read()

orders = re.findall(r"ORD\d+", data)

print(orders)
```

Output:

```
['ORD1001', 'ORD1002', 'ORD1003']
```

***

## 69. File Handling + Data Engineering

Suppose we receive a raw file:

```
raw_orders.txt
```

with:

```
ORD1001, Karthi, 500
ORD1002, Ravi, 700
ORD1003, Arun, 900
```

We can read:

```python
with open("raw_orders.txt", "r") as file:
    for line in file:
        print(line.strip())
```

Then process each record:

```python
with open("raw_orders.txt", "r") as file:
    for line in file:
        order_id, customer, amount = line.strip().split(",")

        print(order_id)
        print(customer)
        print(amount)
```

Output:

```
ORD1001
 Karthi
 500
ORD1002
 Ravi
 700
ORD1003
 Arun
 900
```

We can then clean the values:

```python
with open("raw_orders.txt", "r") as file:
    for line in file:
        order_id, customer, amount = line.strip().split(",")

        order_id = order_id.strip()
        customer = customer.strip()
        amount = float(amount.strip())

        print(order_id, customer, amount)
```

Output:

```
ORD1001 Karthi 500.0
ORD1002 Ravi 700.0
ORD1003 Arun 900.0
```

This is the beginning of a simple ETL pipeline.

***

## 70. Handling File Errors

File operations can fail.

For example:

```python
with open("missing.txt", "r") as file:
    data = file.read()
```

If the file doesn't exist:

```
FileNotFoundError
```

We can handle it using `try-except`.

```python
try:
    with open("missing.txt", "r") as file:
        data = file.read()

    print(data)

except FileNotFoundError:
    print("File does not exist")
```

Output:

```
File does not exist
```

***

## 71. Handling Permission Errors

```python
try:
    with open("data.txt", "r") as file:
        data = file.read()

except FileNotFoundError:
    print("File not found")

except PermissionError:
    print("Permission denied")
```

***

## 72. Generic File Error Handling

```python
try:
    with open("data.txt", "r") as file:
        data = file.read()

except OSError as error:
    print("File operation failed:", error)
```

`OSError` covers many operating-system-level file errors.

***

## 73. Complete File Processing Example

```python
import os
import re

file_path = "orders.log"

if os.path.exists(file_path):

    with open(file_path, "r", encoding="utf-8") as file:

        for line in file:

            line = line.strip()

            order_ids = re.findall(r"ORD\d+", line)

            if order_ids:
                print("Found:", order_ids)

else:
    print("File does not exist")
```

This combines:

```
File existence check
        ↓
File opening
        ↓
Encoding
        ↓
Line-by-line processing
        ↓
String cleaning
        ↓
Regex extraction
```

***

## 74. Complete File Handling Mental Model

Think about file handling in this order:

```
1. What file?
       ↓
2. Where is the file?
       ↓
3. What do I want to do?
       ↓
4. Choose the mode
       ↓
5. Open the file
       ↓
6. Read / Write / Append
       ↓
7. Process the data
       ↓
8. Close the file
```

In modern Python:

```
with open()
```

handles the closing automatically.

***

## 75. File Mode Decision Tree

```
Do I want to read?
       │
       ├── Yes → r
       │
       └── Read + Write?
                 │
                 ├── Preserve content → r+
                 └── Replace content  → w+

Do I want to write?
       │
       ├── Replace existing content → w
       │
       └── Add at end → a

Do I want to create a NEW file only?
       │
       └── x

Do I need binary data?
       │
       └── Add b
```

***

## 76. File Handling Cheat Sheet

### Opening

```python
open("file.txt")
open("file.txt", "r")
open("file.txt", "w")
open("file.txt", "a")
open("file.txt", "x")
```

### Reading

```python
file.read()
file.read(10)
file.readline()
file.readlines()
```

### Writing

```python
file.write("Hello")
file.writelines(["A\n", "B\n"])
```

### Cursor

```python
file.tell()
file.seek(0)
```

### Closing

```python
file.close()
```

### Recommended

```python
with open("file.txt", "r") as file:
    data = file.read()
```

***

## 77. Complete Mode Cheat Sheet

| Mode | Read | Write | Create | Overwrite | Append |
| ---- | ---: | ----: | -----: | --------: | -----: |
| `r`  |    ✓ |     ✗ |      ✗ |         ✗ |      ✗ |
| `w`  |    ✗ |     ✓ |      ✓ |         ✓ |      ✗ |
| `a`  |    ✗ |     ✓ |      ✓ |         ✗ |      ✓ |
| `x`  |    ✗ |     ✓ |      ✓ |         ✗ |      ✗ |
| `r+` |    ✓ |     ✓ |      ✗ |         ✗ |      ✗ |
| `w+` |    ✓ |     ✓ |      ✓ |         ✓ |      ✗ |
| `a+` |    ✓ |     ✓ |      ✓ |         ✗ |      ✓ |

Binary:

```
rb
wb
ab
xb
```

***

## 78. Important Differences to Remember

```
r
→ Read existing file

w
→ Write
→ Existing content gets erased

a
→ Add data to end

x
→ Create new file
→ Error if already exists

+
→ Read + Write

b
→ Binary mode
```

Therefore:

```
r  = read
w  = write/overwrite
a  = append
x  = create
+  = read + write
b  = binary
```

***

## 79. Data Engineering File Handling Pattern

A simple real-world pattern is:

```python
from pathlib import Path

input_file = Path("data/raw/orders.txt")
output_file = Path("data/processed/orders_clean.txt")

with open(input_file, "r", encoding="utf-8") as source:

    with open(output_file, "w", encoding="utf-8") as target:

        for line in source:

            line = line.strip()

            if line:
                target.write(line + "\n")
```

Conceptually:

```
Raw File
   ↓
Read
   ↓
Clean
   ↓
Transform
   ↓
Write
   ↓
Processed File
```

This pattern is the foundation for more advanced ETL file processing.

***

## 80. What to Learn Next

After mastering basic file handling, the next progression should be:

```
File Basics
     ↓
File Modes
     ↓
read()
readline()
readlines()
     ↓
write()
writelines()
     ↓
with open()
     ↓
File Cursor
tell()
seek()
     ↓
Encoding
     ↓
File Paths
os
pathlib
     ↓
Exception Handling
     ↓
CSV Files
     ↓
JSON Files
     ↓
Excel Files
     ↓
Large File Processing
     ↓
Generators
     ↓
Compressed Files
     ↓
Binary Files
     ↓
Temporary Files
     ↓
ETL File Processing
     ↓
Cloud Files
S3 / Azure Blob / GCS
```

For a **Data Engineering Python course**, the most important transition after this topic is:

```
Basic File Handling
        ↓
CSV
        ↓
JSON
        ↓
Large Files
        ↓
ETL File Processing
        ↓
Parquet
```

***

## Key Takeaways

* A file is persistent data stored on a storage device.
* Python uses `open()` to work with files.
* Always understand the file mode before opening a file.
* `r` → read.
* `w` → write and overwrite.
* `a` → append.
* `x` → create only.
* `+` → read and write.
* `b` → binary.
* `read()` reads the entire content.
* `readline()` reads one line.
* `readlines()` returns all lines as a list.
* `write()` writes a string.
* `writelines()` writes multiple strings.
* `tell()` returns the current cursor position.
* `seek()` moves the cursor.
* Prefer:

```python
with open(...) as file:
```

because the file is automatically closed.

* For large files, avoid blindly using `read()`; process the file line by line.
* Use `encoding="utf-8"` when appropriate for text files.
* `pathlib` is a modern and convenient way to work with paths.
* File handling is a fundamental building block of Data Engineering ETL pipelines.
