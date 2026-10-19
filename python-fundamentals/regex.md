# Page

## Regex

> Imported from [`python/Regex.ipynb`](https://github.com/karthikeyanm-dev/DE-Notes/blob/main/python/Regex.ipynb). Code outputs are omitted; run the source notebook for live results.

## Regular Expressions (Regex) in Python

### 1. What is Regex?

**Regular Expression (Regex)** is a pattern used to **search, find, validate, extract, or replace text**.

Instead of checking text character by character manually, we can define a pattern and let Python find the matching text.

For example, suppose we have:

```
Customer email: karthi@gmail.com
Order ID: ORD-2026-1001
Phone: 9876543210
```

We can use Regex to:

* Find an email address
* Find an order ID
* Find phone numbers
* Find numbers from text
* Check whether a string follows a particular format
* Replace unwanted characters
* Extract values from logs
* Validate data coming from files/APIs

***

## 2. Why Regex is Important in Data Engineering

Regex is very useful when working with **raw or semi-structured data**.

For example, a Data Engineer may receive:

```
"Customer: Karthi | Email: karthi@gmail.com | Order: ORD-2026-1001"
```

We can use Regex to extract:

```
Customer → Karthi
Email    → karthi@gmail.com
Order ID → ORD-2026-1001
```

Regex is commonly useful for:

* Data cleaning
* Data validation
* Log processing
* File-name processing
* Extracting IDs
* Extracting dates
* Extracting emails
* Extracting phone numbers
* Removing unwanted characters
* ETL pipelines
* Processing API responses
* Processing text files

***

## 3. Regex Before Python

Before learning Python Regex, understand the basic idea.

Suppose we have:

```
cat
car
can
dog
```

If we want words beginning with `ca`, the pattern can be:

```
ca
```

It matches:

```
cat
car
can
```

But not:

```
dog
```

Now suppose we want:

```
cat
car
can
```

where the third character can be anything.

We can use:

```
ca.
```

Here:

```
. → any single character
```

So:

```
ca.
```

matches:

```
cat
car
can
```

***

## 4. Python Regex Module

Python provides Regex functionality through the built-in `re` module.

```python
import re
```

No installation is required.

`re` stands for **Regular Expression**.

***

## 5. Basic Regex Example

```python
import re

text = "My order number is ORD12345"

pattern = r"ORD\d+"

result = re.search(pattern, text)

print(result)
```

Output:

```
<re.Match object; span=(20, 28), match='ORD12345'>
```

The pattern:

```
r"ORD\d+"
```

means:

```
ORD
+
one or more digits
```

So it finds:

```
ORD12345
```

***

## 6. Why Do We Use `r` Before the Pattern?

You will often see:

```python
r"\d+"
```

instead of:

```python
"\d+"
```

The `r` means **raw string**.

Regex uses backslashes heavily:

```
\d
\w
\s
\b
```

Using raw strings makes Regex patterns easier and safer to write.

Recommended:

```python
pattern = r"\d+"
```

Instead of:

```python
pattern = "\\d+"
```

Both can work, but raw strings are much easier to read.

***

## 7. Basic Regex Syntax

Some of the most important Regex symbols are:

| Pattern | Meaning               |
| ------- | --------------------- |
| `.`     | Any single character  |
| `\d`    | Digit                 |
| `\D`    | Not a digit           |
| `\w`    | Word character        |
| `\W`    | Not a word character  |
| `\s`    | Whitespace            |
| `\S`    | Not whitespace        |
| `^`     | Start of string       |
| `$`     | End of string         |
| `*`     | Zero or more          |
| `+`     | One or more           |
| `?`     | Zero or one           |
| `{n}`   | Exactly n times       |
| `{n,m}` | Between n and m times |
| `[]`    | Character class       |
| `()`    | Group                 |
| \`      | \`                    |

***

## 8. Character Classes

A **character class** allows us to specify which characters are allowed at a particular position.

The basic syntax is:

```
[characters]
```

***

### 8.1 `[abc]`

```
[abc]
```

Means:

```
a OR b OR c
```

Example:

```python
import re

text = "cat bat dog"

result = re.findall(r"[abc]", text)

print(result)
```

Output:

```
['c', 'a', 'b', 'a']
```

***

## 9. `[a-z]`

```
[a-z]
```

Means any lowercase letter from `a` to `z`.

Example:

```python
import re

text = "Hello 123 World"

result = re.findall(r"[a-z]", text)

print(result)
```

Output:

```
['e', 'l', 'l', 'o', 'o', 'r', 'l', 'd']
```

***

## 10. `[A-Z]`

```
[A-Z]
```

Matches uppercase letters.

```python
import re

text = "Hello WORLD"

result = re.findall(r"[A-Z]", text)

print(result)
```

Output:

```
['H', 'W', 'O', 'R', 'L', 'D']
```

***

## 11. `[0-9]`

```
[0-9]
```

Matches digits.

```python
import re

text = "Order12345"

result = re.findall(r"[0-9]", text)

print(result)
```

Output:

```
['1', '2', '3', '4', '5']
```

***

## 12. `[a-zA-Z]`

Matches uppercase and lowercase letters.

```python
import re

text = "Hello123"

result = re.findall(r"[a-zA-Z]", text)

print(result)
```

Output:

```
['H', 'e', 'l', 'l', 'o']
```

***

## 13. `[a-zA-Z0-9]`

Matches letters and numbers.

```python
import re

text = "ABC123-xyz"

result = re.findall(r"[a-zA-Z0-9]", text)

print(result)
```

Output:

```
['A', 'B', 'C', '1', '2', '3', 'x', 'y', 'z']
```

***

## 14. Negated Character Class `[^...]`

`^` inside a character class means **NOT**.

For example:

```
[^0-9]
```

means:

```
anything except a digit
```

Example:

```python
import re

text = "ABC123"

result = re.findall(r"[^0-9]", text)

print(result)
```

Output:

```
['A', 'B', 'C']
```

Another example:

```python
import re

text = "abc123xyz"

result = re.findall(r"[^a-z]", text)

print(result)
```

Output:

```
['1', '2', '3']
```

***

## 15. Important Character Classes

### `\d`

Matches digits.

```python
re.findall(r"\d", "Order123")
```

Output:

```
['1', '2', '3']
```

***

### `\D`

Matches non-digits.

```python
re.findall(r"\D", "Order123")
```

Output:

```
['O', 'r', 'd', 'e', 'r']
```

***

### `\w`

Matches word characters.

Generally:

```
letters
digits
underscore
```

Example:

```python
re.findall(r"\w", "ABC_123!")
```

Output:

```
['A', 'B', 'C', '_', '1', '2', '3']
```

***

### `\W`

Matches non-word characters.

```python
re.findall(r"\W", "ABC_123!")
```

Output:

```
['!']
```

***

### `\s`

Matches whitespace.

```python
text = "Hello World Python"

result = re.findall(r"\s", text)

print(result)
```

Output:

```
[' ', ' ']
```

***

### `\S`

Matches non-whitespace characters.

```python
text = "Hello World"

result = re.findall(r"\S+", text)

print(result)
```

Output:

```
['Hello', 'World']
```

***

## 16. The Dot `.`

The dot means:

```
any single character
```

Example:

```python
import re

text = "cat car cap"

result = re.findall(r"ca.", text)

print(result)
```

Output:

```
['cat', 'car', 'cap']
```

Because:

```
ca + any character
```

***

## 17. Quantifiers

Quantifiers tell Regex **how many times something should occur**.

Important quantifiers:

```
*
+
?
{n}
{n,}
{n,m}
```

***

## 18. `*` — Zero or More

```
*
```

Means:

```
zero or more occurrences
```

Example:

```python
import re

text = "ac abc abbc abbbc"

result = re.findall(r"ab*c", text)

print(result)
```

Output:

```
['ac', 'abc', 'abbc', 'abbbc']
```

Why?

```
ab*c
```

means:

```
a
zero or more b's
c
```

Therefore all of these match:

```
ac
abc
abbc
abbbc
```

***

## 19. `+` — One or More

```
+
```

Means:

```
one or more occurrences
```

Example:

```python
import re

text = "a ab abb abbb"

result = re.findall(r"ab+", text)

print(result)
```

Output:

```
['ab', 'abb', 'abbb']
```

`a` does not match because at least one `b` is required.

***

## 20. `?` — Zero or One

```
?
```

Means:

```
zero or one occurrence
```

Example:

```python
import re

text = "color colour"

result = re.findall(r"colou?r", text)

print(result)
```

Output:

```
['color', 'colour']
```

The `u` is optional.

***

## 21. `{n}` — Exactly n Times

```
{n}
```

Example:

```python
import re

text = "123 1234 12345"

result = re.findall(r"\d{4}", text)

print(result)
```

Output:

```
['1234', '1234']
```

Important:

`findall()` finds four-digit portions, so:

```
12345
```

can contain:

```
1234
```

as a matching portion.

If we want the entire string to contain exactly four digits, anchors should be used.

***

## 22. `{n,}` — At Least n Times

```
{n,}
```

Example:

```python
import re

text = "123 12345 1234567"

result = re.findall(r"\d{5,}", text)

print(result)
```

Output:

```
['12345', '1234567']
```

Means:

```
at least 5 digits
```

***

## 23. `{n,m}` — Between n and m

```
{n,m}
```

Example:

```python
import re

text = "12 123 1234 12345"

result = re.findall(r"\d{3,4}", text)

print(result)
```

Output:

```
['123', '1234', '1234']
```

It means:

```
minimum 3
maximum 4
```

***

## 24. Anchors

Anchors are used to specify the **position** of a match.

### `^` — Start

```
^
```

Means:

```
start of string
```

Example:

```python
import re

text = "Python is powerful"

result = re.search(r"^Python", text)

print(result.group())
```

Output:

```
Python
```

***

## 25. `$` — End

```
$
```

Means:

```
end of string
```

Example:

```python
import re

text = "file.csv"

result = re.search(r"\.csv$", text)

print(result.group())
```

Output:

```
.csv
```

This is useful for checking file extensions.

***

## 26. Groups `()`

Parentheses are used to create **groups**.

Example:

```
(\d{4})-(\d{2})-(\d{2})
```

This can represent:

```
2026-09-10
```

The groups are:

```
2026
09
10
```

Example:

```python
import re

text = "Date: 2026-09-10"

pattern = r"(\d{4})-(\d{2})-(\d{2})"

result = re.search(pattern, text)

print(result.group())
print(result.group(1))
print(result.group(2))
print(result.group(3))
```

Output:

```
2026-09-10
2026
09
10
```

***

## 27. Named Groups

We can give names to groups.

Syntax:

```
(?P<name>pattern)
```

Example:

```python
import re

text = "Order: ORD-2026-1001"

pattern = r"(?P<prefix>[A-Z]+)-(?P<year>\d{4})-(?P<id>\d+)"

result = re.search(pattern, text)

print(result.group("prefix"))
print(result.group("year"))
print(result.group("id"))
```

Output:

```
ORD
2026
1001
```

Named groups are especially useful when extracting structured data.

***

## 28. OR Operator `|`

`|` means:

```
OR
```

Example:

```python
import re

text = "cat dog bird"

result = re.findall(r"cat|dog", text)

print(result)
```

Output:

```
['cat', 'dog']
```

Another example:

```python
pattern = r"csv|json|parquet"
```

This can match any of:

```
csv
json
parquet
```

***

## 29. `re.search()`

`search()` searches the **entire string** and returns the **first matching occurrence**.

Syntax:

```python
re.search(pattern, string)
```

Example:

```python
import re

text = "Customer ID is CUST12345"

result = re.search(r"CUST\d+", text)

print(result.group())
```

Output:

```
CUST12345
```

If no match exists:

```python
import re

text = "Customer ID is ABC123"

result = re.search(r"CUST\d+", text)

print(result)
```

Output:

```
None
```

***

## 30. `search()` Important Point

`search()` finds the pattern **anywhere**.

```python
import re

text = "Hello Customer123"

result = re.search(r"Customer\d+", text)

print(result.group())
```

Output:

```
Customer123
```

It does not require the pattern to be at the beginning.

***

## 31. `re.match()`

`match()` checks only from the **beginning of the string**.

Example:

```python
import re

text = "Customer123"

result = re.match(r"Customer\d+", text)

print(result.group())
```

Output:

```
Customer123
```

But:

```python
import re

text = "Hello Customer123"

result = re.match(r"Customer\d+", text)

print(result)
```

Output:

```
None
```

Because `Customer123` is not at the beginning.

***

## 32. `search()` vs `match()`

| Function      | Where does it search?    |
| ------------- | ------------------------ |
| `re.search()` | Anywhere in string       |
| `re.match()`  | Beginning of string only |

Example:

```python
text = "Hello ABC123"
```

```python
re.search(r"ABC\d+", text)
```

Finds:

```
ABC123
```

But:

```python
re.match(r"ABC\d+", text)
```

Returns:

```
None
```

***

## 33. `re.findall()`

`findall()` returns **all matching occurrences**.

Example:

```python
import re

text = "Orders: ORD123 ORD456 ORD789"

result = re.findall(r"ORD\d+", text)

print(result)
```

Output:

```
['ORD123', 'ORD456', 'ORD789']
```

This is extremely useful in Data Engineering.

For example, extracting all IDs from a log:

```python
log = """
Processed ORD1001
Processed ORD1002
Failed ORD1003
Processed ORD1004
"""

orders = re.findall(r"ORD\d+", log)

print(orders)
```

Output:

```
['ORD1001', 'ORD1002', 'ORD1003', 'ORD1004']
```

***

## 34. `re.sub()`

`sub()` is used to **replace matching text**.

Syntax:

```python
re.sub(pattern, replacement, string)
```

Example:

```python
import re

text = "My phone number is 9876543210"

result = re.sub(r"\d", "*", text)

print(result)
```

Output:

```
My phone number is **********
```

***

## 35. Replacing Multiple Spaces

Suppose:

```python
text = "Hello     World     Python"
```

We can replace multiple spaces with a single space.

```python
import re

text = "Hello     World     Python"

result = re.sub(r"\s+", " ", text)

print(result)
```

Output:

```
Hello World Python
```

This is a common data-cleaning operation.

***

## 36. Removing Special Characters

```python
import re

text = "Karthi@123#Data$Engineer!"

result = re.sub(r"[^a-zA-Z0-9]", "", text)

print(result)
```

Output:

```
Karthi123DataEngineer
```

Pattern:

```
[^a-zA-Z0-9]
```

means:

```
anything that is NOT a letter or digit
```

***

## 37. Extracting Email Addresses

A basic email pattern:

```
[\w.-]+@[\w.-]+\.\w+
```

Example:

```python
import re

text = "Contact us at support@example.com"

result = re.findall(r"[\w.-]+@[\w.-]+\.\w+", text)

print(result)
```

Output:

```
['support@example.com']
```

This is a practical example of Regex extraction.

***

## 38. Extracting Multiple Emails

```python
import re

text = """
Contact:
admin@example.com
support@test.com
sales@company.org
"""

emails = re.findall(r"[\w.-]+@[\w.-]+\.\w+", text)

print(emails)
```

Output:

```
['admin@example.com', 'support@test.com', 'sales@company.org']
```

***

## 39. Extracting Phone Numbers

For a basic 10-digit phone number:

```python
import re

text = "Call me at 9876543210"

result = re.findall(r"\d{10}", text)

print(result)
```

Output:

```
['9876543210']
```

***

## 40. Extracting Dates

Suppose dates are stored as:

```
2026-09-10
2026-08-25
2025-12-31
```

Pattern:

```
\d{4}-\d{2}-\d{2}
```

Example:

```python
import re

text = "Orders were processed on 2026-09-10 and 2026-08-25"

dates = re.findall(r"\d{4}-\d{2}-\d{2}", text)

print(dates)
```

Output:

```
['2026-09-10', '2026-08-25']
```

***

## 41. Data Engineering Example — Log Processing

Consider a log:

```python
log = """
2026-09-10 INFO Order ORD1001 processed
2026-09-10 INFO Order ORD1002 processed
2026-09-10 ERROR Order ORD1003 failed
2026-09-10 INFO Order ORD1004 processed
"""
```

### Extract Order IDs

```python
import re

orders = re.findall(r"ORD\d+", log)

print(orders)
```

Output:

```
['ORD1001', 'ORD1002', 'ORD1003', 'ORD1004']
```

### Extract Dates

```python
dates = re.findall(r"\d{4}-\d{2}-\d{2}", log)

print(dates)
```

Output:

```
['2026-09-10', '2026-09-10', '2026-09-10', '2026-09-10']
```

### Extract ERROR

```python
errors = re.findall(r"ERROR", log)

print(errors)
```

Output:

```
['ERROR']
```

***

## 42. Data Cleaning Example

Suppose a source system gives:

```python
data = "  Karthi     Kumar   |   Data Engineer  "
```

We want:

```
Karthi Kumar | Data Engineer
```

Use:

```python
import re

data = "  Karthi     Kumar   |   Data Engineer  "

cleaned = re.sub(r"\s+", " ", data).strip()

print(cleaned)
```

Output:

```
Karthi Kumar | Data Engineer
```

***

## 43. Validation Example — Order ID

Suppose valid Order IDs must look like:

```
ORD-2026-1001
ORD-2026-1002
```

Pattern:

```
^ORD-\d{4}-\d+$
```

Explanation:

```
^        → start
ORD-     → exact text
\d{4}    → exactly 4 digits
-        → hyphen
\d+      → one or more digits
$        → end
```

Example:

```python
import re

order_id = "ORD-2026-1001"

if re.match(r"^ORD-\d{4}-\d+$", order_id):
    print("Valid Order ID")
else:
    print("Invalid Order ID")
```

Output:

```
Valid Order ID
```

***

## 44. Complete Basic Regex Example

```python
import re

text = """
Customer: Karthi
Email: karthi@example.com
Order: ORD-2026-1001
Phone: 9876543210
Date: 2026-09-10
"""

email = re.findall(r"[\w.-]+@[\w.-]+\.\w+", text)
order = re.findall(r"ORD-\d{4}-\d+", text)
phone = re.findall(r"\d{10}", text)
date = re.findall(r"\d{4}-\d{2}-\d{2}", text)

print("Email:", email)
print("Order:", order)
print("Phone:", phone)
print("Date:", date)
```

Output:

```
Email: ['karthi@example.com']
Order: ['ORD-2026-1001']
Phone: ['9876543210']
Date: ['2026-09-10']
```

***

## 45. Regex Cheat Sheet

### Character Classes

| Regex      | Meaning               | Example              |
| ---------- | --------------------- | -------------------- |
| `.`        | Any character         | `a.c` → `abc`, `axc` |
| `\d`       | Digit                 | `123`                |
| `\D`       | Non-digit             | `ABC`                |
| `\w`       | Word character        | `A`, `1`, `_`        |
| `\W`       | Non-word character    | `@`, `#`, `!`        |
| `\s`       | Whitespace            | space, tab           |
| `\S`       | Non-whitespace        | `Hello`              |
| `[abc]`    | a, b, or c            | `a`                  |
| `[a-z]`    | Lowercase letters     | `a-z`                |
| `[A-Z]`    | Uppercase letters     | `A-Z`                |
| `[0-9]`    | Digits                | `0-9`                |
| `[a-zA-Z]` | Any letter            | `A-Z`, `a-z`         |
| `[^0-9]`   | Anything except digit | `A`, `@`             |

***

## 46. Quantifier Cheat Sheet

| Regex   | Meaning      | Example   |
| ------- | ------------ | --------- |
| `*`     | Zero or more | `ab*c`    |
| `+`     | One or more  | `ab+c`    |
| `?`     | Zero or one  | `colou?r` |
| `{n}`   | Exactly n    | `\d{4}`   |
| `{n,}`  | At least n   | `\d{5,}`  |
| `{n,m}` | n to m       | `\d{3,5}` |

***

## 47. Anchor Cheat Sheet

| Regex | Meaning             |
| ----- | ------------------- |
| `^`   | Start of string     |
| `$`   | End of string       |
| `\b`  | Word boundary       |
| `\B`  | Not a word boundary |

Example:

```python
import re

text = "Python Pythonic"

result = re.findall(r"\bPython\b", text)

print(result)
```

Output:

```
['Python']
```

It matches the complete word `Python`, not the `Python` inside `Pythonic`.

***

## 48. Group Cheat Sheet

| Regex           | Meaning             |
| --------------- | ------------------- |
| `(abc)`         | Capturing group     |
| `(?:abc)`       | Non-capturing group |
| `(?P<name>abc)` | Named group         |
| \`a             | b\`                 |

Example:

```python
import re

text = "ORD-2026-1001"

pattern = r"(ORD)-(\d{4})-(\d+)"

result = re.search(pattern, text)

print(result.group(1))
print(result.group(2))
print(result.group(3))
```

Output:

```
ORD
2026
1001
```

***

## 49. Python Regex Function Cheat Sheet

| Function         | Purpose                   |
| ---------------- | ------------------------- |
| `re.search()`    | Find first match anywhere |
| `re.match()`     | Match from beginning      |
| `re.findall()`   | Find all matches          |
| `re.sub()`       | Replace matches           |
| `re.split()`     | Split using Regex         |
| `re.fullmatch()` | Match entire string       |
| `re.compile()`   | Compile a Regex pattern   |

***

## 50. `search()` vs `match()` vs `findall()`

Suppose:

```python
text = "ABC123 XYZ456 ABC789"
```

#### `search()`

```python
re.search(r"ABC\d+", text)
```

Returns first matching occurrence:

```
ABC123
```

#### `match()`

```python
re.match(r"ABC\d+", text)
```

Returns:

```
ABC123
```

because the pattern starts at the beginning.

#### `findall()`

```python
re.findall(r"ABC\d+", text)
```

Returns:

```
['ABC123', 'ABC789']
```

***

## 51. `sub()` Cheat Sheet

Basic replacement:

```python
re.sub(pattern, replacement, text)
```

Example:

```python
re.sub(r"\d", "*", "ABC123")
```

Output:

```
ABC***
```

Remove characters:

```python
re.sub(r"[^a-zA-Z0-9]", "", text)
```

Replace multiple spaces:

```python
re.sub(r"\s+", " ", text)
```

Mask email username:

```python
re.sub(r"(?<=\w)@\w+", "@***", "user@gmail.com")
```

***

## 52. Compiling a Regex

If the same Regex pattern is used many times, we can compile it.

```python
import re

pattern = re.compile(r"ORD\d+")

text1 = "ORD1001"
text2 = "ORD1002"

print(pattern.search(text1).group())
print(pattern.search(text2).group())
```

Output:

```
ORD1001
ORD1002
```

Instead of repeatedly writing:

```python
re.search(r"ORD\d+", text)
```

we can reuse:

```python
pattern.search(text)
```

***

## 53. Important Difference: `match()` Does Not Mean Exact Match

This is an important beginner mistake.

Consider:

```python
import re

text = "ORD12345ABC"

result = re.match(r"ORD\d+", text)

print(result.group())
```

Output:

```
ORD12345
```

It matches from the beginning, but the entire string is not validated.

If we want the **whole string** to match, use anchors:

```python
re.match(r"^ORD\d+$", text)
```

or:

```python
re.fullmatch(r"ORD\d+", text)
```

***

## 54. `re.fullmatch()`

`fullmatch()` requires the entire string to match.

```python
import re

text = "ORD12345"

result = re.fullmatch(r"ORD\d+", text)

print(result.group())
```

Output:

```
ORD12345
```

But:

```python
text = "ORD12345ABC"

result = re.fullmatch(r"ORD\d+", text)

print(result)
```

Output:

```
None
```

***

## 55. A Practical Data Engineering Regex Pipeline

Suppose a raw record contains:

```python
raw_record = """
Customer: Karthi Kumar
Email: karthi@example.com
Order: ORD-2026-1001
Phone: 9876543210
"""
```

We can extract structured fields.

```python
import re

raw_record = """
Customer: Karthi Kumar
Email: karthi@example.com
Order: ORD-2026-1001
Phone: 9876543210
"""

customer = re.search(r"Customer:\s*(.+)", raw_record).group(1)
email = re.search(r"Email:\s*(\S+)", raw_record).group(1)
order = re.search(r"Order:\s*(ORD-\d{4}-\d+)", raw_record).group(1)
phone = re.search(r"Phone:\s*(\d{10})", raw_record).group(1)

record = {
    "customer": customer,
    "email": email,
    "order": order,
    "phone": phone
}

print(record)
```

Output:

```
{
    'customer': 'Karthi Kumar',
    'email': 'karthi@example.com',
    'order': 'ORD-2026-1001',
    'phone': '9876543210'
}
```

This is a simple example of converting **semi-structured text into structured data**.

***

## 56. Common Regex Mistakes

### Mistake 1 — Forgetting raw strings

Prefer:

```python
r"\d+"
```

instead of:

```python
"\d+"
```

***

### Mistake 2 — Confusing `*` and `+`

```
* → zero or more
+ → one or more
```

***

### Mistake 3 — Confusing `search()` and `match()`

```
search() → anywhere
match()  → beginning
```

***

### Mistake 4 — Assuming `match()` validates the whole string

It does not.

Use:

```python
re.fullmatch()
```

or:

```
^pattern$
```

for complete validation.

***

### Mistake 5 — Forgetting that Regex is case-sensitive

```python
re.search(r"python", "Python")
```

does not normally match.

For case-insensitive matching:

```python
re.search(r"python", "Python", re.IGNORECASE)
```

***

## 57. Regex with Case-Insensitive Matching

```python
import re

text = "Python PYTHON python"

result = re.findall(r"python", text, re.IGNORECASE)

print(result)
```

Output:

```
['Python', 'PYTHON', 'python']
```

***

## 58. Regex Mental Model

When you see a Regex pattern, read it from left to right.

For example:

```
^ORD-\d{4}-\d+$
```

Read it as:

```
^
Start

ORD-
The text ORD-

\d{4}
Exactly 4 digits

-
A hyphen

\d+
One or more digits

$
End
```

So valid values include:

```
ORD-2026-1001
ORD-2025-45
ORD-1999-123456
```

Invalid examples:

```
ABC-2026-1001
ORD-26-1001
ORD-2026
ORD-2026-ABC
```

***

## 59. Quick Regex Cheat Sheet for Data Engineers

```
\d+                    → one or more digits

\d{10}                 → exactly 10 digits

\d{4}-\d{2}-\d{2}      → YYYY-MM-DD

[A-Z]+                 → one or more uppercase letters

[a-z]+                 → one or more lowercase letters

[a-zA-Z]+              → one or more letters

\w+                    → one or more word characters

\s+                    → one or more spaces/whitespace

[^a-zA-Z0-9]           → special character

[^a-zA-Z0-9]+          → one or more special characters

^ABC                   → starts with ABC

ABC$                   → ends with ABC

^ABC$                  → exactly ABC

ORD\d+                 → ORD followed by digits

ORD-\d{4}-\d+          → Order ID pattern

[\w.-]+@[\w.-]+\.\w+   → basic email pattern

.*                     → zero or more of any character

.+                     → one or more of any character
```

***

## 60. Most Important Patterns to Remember

#### Character Classes

```
\d       digit
\D       non-digit
\w       word character
\W       non-word character
\s       whitespace
\S       non-whitespace
```

#### Character Sets

```
[abc]       a, b or c
[a-z]       lowercase
[A-Z]       uppercase
[0-9]       digit
[^0-9]      not a digit
```

#### Quantifiers

```
*           0 or more
+           1 or more
?           0 or 1
{3}         exactly 3
{3,}        at least 3
{3,5}       3 to 5
```

#### Position

```
^           beginning
$           end
\b          word boundary
```

#### Groups

```
(...)       group
(?:...)     non-capturing group
(?P<name>...) named group
|           OR
```

#### Python Functions

```
re.search()
re.match()
re.findall()
re.sub()
re.split()
re.fullmatch()
re.compile()
```

***

## 61. Practice Problems

### Problem 1 — Extract Numbers

Given:

```python
text = "Orders 1001, 1002 and 1003 were processed"
```

Extract all numbers.

Expected:

```
['1001', '1002', '1003']
```

***

### Problem 2 — Extract Order IDs

Given:

```python
text = "ORD1001 ORD1002 ABC1003 ORD1004"
```

Extract only Order IDs beginning with `ORD`.

Expected:

```
['ORD1001', 'ORD1002', 'ORD1004']
```

***

### Problem 3 — Extract Emails

Given:

```python
text = """
admin@example.com
support@test.com
invalid-email
sales@company.org
"""
```

Extract valid-looking email addresses.

***

### Problem 4 — Remove Special Characters

Given:

```python
text = "Karthi@Data#Engineer$2026!"
```

Expected:

```
KarthiDataEngineer2026
```

***

### Problem 5 — Normalize Spaces

Given:

```python
text = "Python     is       useful     for     Data Engineering"
```

Convert multiple spaces into one.

Expected:

```
Python is useful for Data Engineering
```

***

### Problem 6 — Validate Order ID

Create a Regex that validates:

```
ORD-2026-1001
```

but rejects:

```
ABC-2026-1001
ORD-26-1001
ORD-2026-ABC
```

***

### Problem 7 — Extract Dates

Given:

```python
text = """
Order created: 2026-09-01
Order shipped: 2026-09-05
Order delivered: 2026-09-08
"""
```

Extract all dates.

Expected:

```
['2026-09-01', '2026-09-05', '2026-09-08']
```

***

### Problem 8 — Log Analysis

Given:

```python
log = """
INFO Order ORD1001 processed
INFO Order ORD1002 processed
ERROR Order ORD1003 failed
INFO Order ORD1004 processed
ERROR Order ORD1005 failed
"""
```

Extract:

1. All Order IDs
2. All `ERROR` occurrences
3. IDs of failed orders

Expected failed orders:

```
['ORD1003', 'ORD1005']
```

***

## 62. Final Key Takeaways

1. **Regex is a pattern-matching tool for text.**
2. Python provides Regex through the built-in `re` module.
3. Use raw strings such as:

```python
r"\d+"
```

4. Character classes define allowed characters:

```
[abc]
[a-z]
[A-Z]
[0-9]
[^0-9]
```

5. Special character classes:

```
\d
\w
\s
```

6. Quantifiers control repetition:

```
*
+
?
{n}
{n,m}
```

7. Anchors control position:

```
^
$
\b
```

8. Groups are created using:

```
(...)
```

9. `search()` finds the first match anywhere.
10. `match()` checks from the beginning.
11. `findall()` extracts all matches.
12. `sub()` replaces matching text.
13. Regex is especially useful in Data Engineering for:

```
Raw Text
   ↓
Regex Extraction
   ↓
Cleaning
   ↓
Validation
   ↓
Structured Data
   ↓
ETL / Pipeline
```

14. The most important Regex functions to master first are:

```python
re.search()
re.match()
re.findall()
re.sub()
```

15. The most important Regex concepts to master first are:

```
Character Classes
Quantifiers
Anchors
Groups
```

Once these are comfortable, more advanced Regex topics such as **lookahead, lookbehind, non-capturing groups, flags, backreferences, greedy vs lazy matching, and complex validation patterns** become much easier.
