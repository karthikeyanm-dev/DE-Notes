# Page

## Exception Handling

> Imported from [`python/Exception_Handling.ipynb`](https://github.com/karthikeyanm-dev/DE-Notes/blob/main/python/Exception_Handling.ipynb). Code outputs are omitted; run the source notebook for live results.

## Exception Handling in Python

Exception Handling is used to handle **unexpected situations that occur while a Python program is running**.

In Data Engineering, exception handling is extremely important because pipelines regularly deal with:

* Missing files
* Invalid data
* Wrong data types
* Database connection failures
* API failures
* Permission problems
* Invalid user input
* Network failures
* Duplicate records
* Unexpected data formats

Instead of allowing the entire program or pipeline to crash, we can **detect the problem, handle it, recover if possible, and continue execution**.

***

## 1. What is an Exception?

An **exception** is an unexpected event that occurs **during the execution of a program** and interrupts the normal flow of execution.

For example:

```python
number = 10
result = number / 0

print(result)
```

Output:

```
ZeroDivisionError: division by zero
```

Python encountered a problem while executing:

```python
number / 0
```

This problem is an **exception**.

***

## 2. What is an Error?

An **error** is a problem in a program that prevents the program from working correctly.

In Python, the word "error" is commonly used for different kinds of problems.

For example:

```python
print("Hello"
```

This produces:

```
SyntaxError
```

because the Python code itself is not written correctly.

Another example:

```python
10 / 0
```

produces:

```
ZeroDivisionError
```

because the code is syntactically correct, but an invalid operation occurs while the program runs.

***

## 3. Error vs Exception

This distinction is important.

#### Error

An error is a broad term for a problem in a program.

#### Exception

An exception is an event raised by Python during program execution that can often be **caught and handled**.

A simple way to remember:

```
Error
   ↓
Something is wrong

Exception
   ↓
Problem occurs during execution
   ↓
Python raises an exception
   ↓
We can catch and handle it
```

***

## 4. Example Without Exception Handling

Consider:

```python
print("Program started")

number = 10
result = number / 0

print("Program completed")
```

Output:

```
Program started

ZeroDivisionError: division by zero
```

Notice:

```python
print("Program completed")
```

was never executed.

The exception interrupted the normal flow.

***

## 5. Exception Handling

Python provides:

```
try
except
else
finally
```

for exception handling.

Basic structure:

```python
try:
    # Code that may cause an exception

except:
    # Code to handle the exception
```

***

## 6. Basic Example

```python
try:
    number = 10
    result = number / 0

except:
    print("Something went wrong")

print("Program completed")
```

Output:

```
Something went wrong
Program completed
```

The program did not terminate because the exception was handled.

***

## 7. How Exception Handling Works

The flow is:

```
try
 ↓
Execute risky code
 ↓
Exception occurs?
 ├── No → Continue
 │
 └── Yes
      ↓
   except
      ↓
   Handle problem
      ↓
   Continue program
```

***

## 8. `try`

The `try` block contains code that **may cause an exception**.

Example:

```python
try:
    number = int(input("Enter a number: "))
    result = 100 / number
```

Potential problems:

```
Invalid input
Division by zero
```

***

## 9. `except`

The `except` block handles the exception.

```python
try:
    number = int(input("Enter a number: "))
    result = 100 / number

except:
    print("Invalid operation")
```

***

## 10. Better Practice — Specify the Exception

Instead of:

```python
except:
```

we should normally specify the exception we expect.

Example:

```python
try:
    number = 10 / 0

except ZeroDivisionError:
    print("Cannot divide by zero")
```

Output:

```
Cannot divide by zero
```

This is better because we know exactly what problem we are handling.

***

## 11. `except` with Multiple Exception Types

We can handle different exceptions separately.

```python
try:
    number = int(input("Enter a number: "))
    result = 100 / number

except ValueError:
    print("Please enter a valid number")

except ZeroDivisionError:
    print("Number cannot be zero")
```

Possible scenarios:

```
Input: abc

Output:
Please enter a valid number
```

or:

```
Input: 0

Output:
Number cannot be zero
```

***

## 12. What Happens After an Exception is Handled?

Example:

```python
try:
    number = 10 / 0

except ZeroDivisionError:
    print("Cannot divide by zero")

print("Pipeline continues")
```

Output:

```
Cannot divide by zero
Pipeline continues
```

Once the exception is handled, Python continues after the `try-except` block.

***

## 13. Recovering from an Exception

Exception handling is not only about printing an error message.

We can **recover** from an exception.

For example:

```python
try:
    number = int(input("Enter a number: "))

except ValueError:
    print("Invalid input")
    number = 0

print("Number:", number)
```

If the user enters:

```
abc
```

Output:

```
Invalid input
Number: 0
```

We recovered by assigning a default value.

***

## 14. Recovery by Asking Again

A better approach for user input is to ask again.

```python
while True:

    try:
        number = int(input("Enter a number: "))
        break

    except ValueError:
        print("Invalid input. Try again.")

print("You entered:", number)
```

Example:

```
Enter a number: abc
Invalid input. Try again.

Enter a number: hello
Invalid input. Try again.

Enter a number: 25
You entered: 25
```

This is a real example of **recovery from an exception**.

***

## 15. `else`

The `else` block executes when **no exception occurs**.

Syntax:

```python
try:
    # risky code

except:
    # exception handling

else:
    # executes when no exception occurs
```

Example:

```python
try:
    number = int(input("Enter number: "))

except ValueError:
    print("Invalid number")

else:
    print("Valid number:", number)
```

If input is:

```
25
```

Output:

```
Valid number: 25
```

***

## 16. Why Use `else`?

It keeps successful code separate from exception-handling code.

Example:

```python
try:
    number = int(input("Enter number: "))

except ValueError:
    print("Invalid input")

else:
    result = number * 10
    print(result)
```

The `else` block executes only if the `try` block succeeds.

***

## 17. `finally`

The `finally` block **always executes**, whether an exception occurs or not.

Syntax:

```python
try:
    # code

except:
    # handle exception

finally:
    # always executes
```

Example:

```python
try:
    number = 10 / 0

except ZeroDivisionError:
    print("Cannot divide by zero")

finally:
    print("Execution completed")
```

Output:

```
Cannot divide by zero
Execution completed
```

***

## 18. `finally` When No Exception Occurs

```python
try:
    number = 10 / 2
    print(number)

except ZeroDivisionError:
    print("Cannot divide by zero")

finally:
    print("Execution completed")
```

Output:

```
5.0
Execution completed
```

So:

```
Exception → finally executes
No exception → finally executes
```

***

## 19. Why is `finally` Important in Data Engineering?

Suppose we open a file:

```python
file = open("data.txt", "r")
```

Something could go wrong while processing it.

We want to make sure the file is closed.

```python
file = None

try:
    file = open("data.txt", "r")
    data = file.read()

except FileNotFoundError:
    print("File not found")

finally:
    if file:
        file.close()
```

However, for files, the preferred approach is:

```python
with open("data.txt", "r") as file:
    data = file.read()
```

because the context manager automatically handles closing.

***

## 20. Complete `try-except-else-finally`

```python
try:
    number = int(input("Enter a number: "))
    result = 100 / number

except ValueError:
    print("Please enter a valid integer")

except ZeroDivisionError:
    print("Number cannot be zero")

else:
    print("Result:", result)

finally:
    print("Operation completed")
```

Input:

```
20
```

Output:

```
Result: 5.0
Operation completed
```

Input:

```
0
```

Output:

```
Number cannot be zero
Operation completed
```

Input:

```
abc
```

Output:

```
Please enter a valid integer
Operation completed
```

***

## 21. Common Python Exceptions

Python provides many built-in exception types.

Some important ones are:

| Exception             | Meaning                         |
| --------------------- | ------------------------------- |
| `SyntaxError`         | Invalid Python syntax           |
| `NameError`           | Variable/name does not exist    |
| `TypeError`           | Wrong type used in an operation |
| `ValueError`          | Correct type but invalid value  |
| `IndexError`          | Invalid list/string index       |
| `KeyError`            | Dictionary key doesn't exist    |
| `ZeroDivisionError`   | Division by zero                |
| `FileNotFoundError`   | File does not exist             |
| `FileExistsError`     | File already exists             |
| `PermissionError`     | Insufficient permission         |
| `AttributeError`      | Attribute/method doesn't exist  |
| `ImportError`         | Import problem                  |
| `ModuleNotFoundError` | Module cannot be found          |
| `OSError`             | Operating-system-related error  |
| `OverflowError`       | Numeric operation exceeds limit |
| `RecursionError`      | Recursion exceeds maximum depth |

***

## 22. `ValueError`

Occurs when a function receives the correct type but an inappropriate value.

Example:

```python
number = int("hello")
```

Output:

```
ValueError
```

Handle it:

```python
try:
    number = int("hello")

except ValueError:
    print("Cannot convert text to integer")
```

***

## 23. `TypeError`

Occurs when an operation is performed using incompatible types.

Example:

```python
result = "100" + 50
```

Output:

```
TypeError
```

Handle it:

```python
try:
    result = "100" + 50

except TypeError:
    print("Cannot add string and integer")
```

***

## 24. `IndexError`

Occurs when accessing an invalid index.

```python
numbers = [10, 20, 30]

print(numbers[5])
```

Output:

```
IndexError
```

Handle it:

```python
try:
    print(numbers[5])

except IndexError:
    print("Index does not exist")
```

***

## 25. `KeyError`

Occurs when accessing a dictionary key that does not exist.

```python
customer = {
    "name": "Karthi",
    "city": "Chennai"
}

print(customer["age"])
```

Output:

```
KeyError
```

Handle it:

```python
try:
    print(customer["age"])

except KeyError:
    print("Age key does not exist")
```

***

## 26. `NameError`

Occurs when a variable has not been defined.

```python
print(customer_name)
```

Output:

```
NameError
```

Example:

```python
try:
    print(customer_name)

except NameError:
    print("Variable is not defined")
```

***

## 27. `FileNotFoundError`

Very important for Data Engineering.

```python
with open("missing.csv", "r") as file:
    data = file.read()
```

Output:

```
FileNotFoundError
```

Handle it:

```python
try:
    with open("missing.csv", "r") as file:
        data = file.read()

except FileNotFoundError:
    print("Input file is missing")
```

***

## 28. `PermissionError`

Occurs when Python doesn't have permission to perform the requested operation.

```python
try:
    with open("restricted.txt", "r") as file:
        data = file.read()

except PermissionError:
    print("You do not have permission to access this file")
```

***

## 29. `AttributeError`

Occurs when an object does not have the requested attribute or method.

Example:

```python
name = "Karthi"

name.append("K")
```

Strings don't have an `append()` method.

So:

```
AttributeError
```

***

## 30. `ModuleNotFoundError`

Occurs when Python cannot find the requested module.

```python
import something_that_does_not_exist
```

Output:

```
ModuleNotFoundError
```

***

## 31. `ZeroDivisionError`

```python
try:
    result = 100 / 0

except ZeroDivisionError:
    print("Cannot divide by zero")
```

Output:

```
Cannot divide by zero
```

***

## 32. Catching Multiple Exceptions Together

If multiple exceptions should receive the same handling:

```python
try:
    number = int(input("Enter number: "))
    result = 100 / number

except (ValueError, ZeroDivisionError):
    print("Invalid input or division by zero")
```

***

## 33. Exception Object

We can store the exception in a variable.

```python
try:
    result = 10 / 0

except ZeroDivisionError as error:
    print(error)
```

Output:

```
division by zero
```

The variable:

```python
error
```

contains information about the exception.

***

## 34. Logging the Exception

Instead of only printing the error:

```python
except Exception as error:
    print("Error:", error)
```

In production applications, we commonly use Python's `logging` module.

Example:

```python
import logging

logging.basicConfig(level=logging.ERROR)

try:
    result = 10 / 0

except ZeroDivisionError as error:
    logging.error("Calculation failed: %s", error)
```

This is much more useful for production pipelines because logs can be reviewed later.

***

## 35. Catching `Exception`

We can write:

```python
try:
    # code

except Exception as error:
    print("Error:", error)
```

`Exception` is a broad base class for most normal runtime exceptions.

Example:

```python
try:
    result = 10 / 0

except Exception as error:
    print("Something went wrong:", error)
```

Output:

```
Something went wrong: division by zero
```

***

## 36. Should We Always Use `except Exception`?

Not necessarily.

Prefer specific exceptions when you know what can happen.

Better:

```python
try:
    number = int(value)

except ValueError:
    print("Invalid number")
```

Less specific:

```python
try:
    number = int(value)

except Exception:
    print("Something went wrong")
```

Why?

Because specific exceptions make the program easier to understand and debug.

***

## 37. Avoid Bare `except`

You may see:

```python
try:
    ...
except:
    ...
```

This catches almost everything and can hide important problems.

Prefer:

```python
except ValueError:
```

or:

```python
except FileNotFoundError:
```

or, when genuinely necessary:

```python
except Exception as error:
```

***

## 38. Raising an Exception Manually

Python allows us to raise an exception using:

```python
raise
```

Example:

```python
age = -5

if age < 0:
    raise ValueError("Age cannot be negative")
```

Output:

```
ValueError: Age cannot be negative
```

This is useful for data validation.

***

## 39. `raise` in Data Engineering

Suppose a pipeline requires an amount greater than zero.

```python
amount = -500

if amount < 0:
    raise ValueError("Amount cannot be negative")
```

This prevents invalid data from silently entering the pipeline.

***

## 40. `raise` with Exception Handling

We can raise an exception and then catch it.

```python
try:

    amount = -500

    if amount < 0:
        raise ValueError("Amount cannot be negative")

except ValueError as error:
    print("Validation failed:", error)
```

Output:

```
Validation failed: Amount cannot be negative
```

***

## 41. Custom Exceptions

We can create our own exception types.

Example:

```python
class InvalidOrderError(Exception):
    pass
```

Then:

```python
order_id = ""

try:

    if not order_id:
        raise InvalidOrderError("Order ID cannot be empty")

except InvalidOrderError as error:
    print(error)
```

Output:

```
Order ID cannot be empty
```

Custom exceptions are useful in larger applications and data pipelines.

***

## 42. Nested Exception Handling

We can have `try-except` inside another `try-except`.

```python
try:

    try:
        number = int("abc")

    except ValueError:
        print("Invalid number")

except Exception:
    print("Outer exception")
```

Output:

```
Invalid number
```

Usually, we should avoid unnecessary nesting because it can make code difficult to understand.

***

## 43. Exception Handling with File Processing

This is a very important Data Engineering example.

```python
try:

    with open("orders.csv", "r", encoding="utf-8") as file:

        for line in file:
            print(line.strip())

except FileNotFoundError:
    print("Orders file is missing")

except PermissionError:
    print("No permission to read orders file")
```

Possible outcomes:

```
File exists
     ↓
Read file
     ↓
Process records
```

or:

```
File missing
     ↓
FileNotFoundError
     ↓
Handle exception
```

***

## 44. Exception Handling in an ETL Pipeline

Consider:

```
Extract
   ↓
Transform
   ↓
Load
```

A failure can occur at any stage.

Example:

```python
def extract():
    print("Extracting data")


def transform():
    print("Transforming data")


def load():
    print("Loading data")


try:

    extract()
    transform()
    load()

except Exception as error:

    print("Pipeline failed:", error)
```

Output:

```
Extracting data
Transforming data
Loading data
```

***

## 45. ETL Example with Failure

```python
def extract():
    print("Extracting data")
    return [100, 200, 300]


def transform(data):
    print("Transforming data")

    result = []

    for value in data:
        result.append(value / 0)

    return result


def load(data):
    print("Loading data")


try:

    data = extract()

    transformed_data = transform(data)

    load(transformed_data)

except ZeroDivisionError:
    print("Transformation failed because of division by zero")
```

Output:

```
Extracting data
Transforming data
Transformation failed because of division by zero
```

Notice:

```
Extract
   ↓
Transform
   ↓
Exception
   ↓
Load is not executed
```

***

## 46. Recovering from an ETL Exception

Instead of completely stopping, we may decide to skip bad records.

```python
data = [100, 200, 0, 300]

results = []

for value in data:

    try:
        result = 1000 / value
        results.append(result)

    except ZeroDivisionError:
        print("Skipping invalid value:", value)

print(results)
```

Output:

```
Skipping invalid value: 0
[10.0, 5.0, 3.3333333333333335]
```

This is a very common data-processing pattern:

```
Good Record
    ↓
Process

Bad Record
    ↓
Catch exception
    ↓
Log / Skip / Quarantine
    ↓
Continue
```

***

## 47. Recovery Strategies

When an exception occurs, there are several possible strategies.

### Strategy 1 — Use a Default Value

```python
try:
    amount = float(value)

except ValueError:
    amount = 0
```

***

### Strategy 2 — Skip the Bad Record

```python
try:
    process(record)

except ValueError:
    continue
```

***

### Strategy 3 — Retry

Useful for:

```
API calls
Network operations
Database connections
Cloud services
```

Example concept:

```python
for attempt in range(3):

    try:
        connect_to_api()
        break

    except ConnectionError:
        print("Retrying...")
```

***

### Strategy 4 — Log the Problem

```python
except Exception as error:
    logging.error("Pipeline failed: %s", error)
```

***

### Strategy 5 — Stop the Pipeline

Some errors are too serious to ignore.

```python
except Exception as error:
    print("Critical error:", error)
    raise
```

This allows the exception to propagate after logging or performing cleanup.

***

## 48. Retry Example

```python
import time

for attempt in range(3):

    try:
        print("Attempt:", attempt + 1)

        # Simulating an operation
        raise ConnectionError("API unavailable")

    except ConnectionError as error:

        print("Connection failed:", error)

        time.sleep(1)

else:
    print("All attempts failed")
```

In a real application, the operation would be an API or database request.

***

## 49. Exception Handling in Data Validation

Suppose we receive:

```python
records = [
    {"id": "101", "amount": "500"},
    {"id": "102", "amount": "700"},
    {"id": "103", "amount": "INVALID"},
    {"id": "104", "amount": "900"}
]
```

We can process records safely:

```python
valid_records = []
invalid_records = []

for record in records:

    try:

        record["id"] = int(record["id"])
        record["amount"] = float(record["amount"])

        valid_records.append(record)

    except ValueError:

        invalid_records.append(record)

print("Valid:", valid_records)
print("Invalid:", invalid_records)
```

Output:

```
Valid: [
    {'id': 101, 'amount': 500.0},
    {'id': 102, 'amount': 700.0},
    {'id': 104, 'amount': 900.0}
]

Invalid: [
    {'id': '103', 'amount': 'INVALID'}
]
```

This is a very practical data-engineering use of exception handling.

***

## 50. Exception Handling vs Validation

These are related but not identical.

Validation:

```python
if amount < 0:
    print("Invalid amount")
```

Exception handling:

```python
try:
    amount = float(value)

except ValueError:
    print("Invalid numeric value")
```

Use normal conditions for expected business rules:

```
amount < 0
missing customer ID
status not in allowed values
```

Use exceptions for exceptional runtime situations:

```
file missing
invalid conversion
database failure
network failure
```

***

## 51. Exception Hierarchy

Python exceptions have a hierarchy.

Simplified:

```
BaseException
    |
    +-- Exception
          |
          +-- ArithmeticError
          |     |
          |     +-- ZeroDivisionError
          |
          +-- LookupError
          |     |
          |     +-- IndexError
          |     +-- KeyError
          |
          +-- OSError
          |     |
          |     +-- FileNotFoundError
          |     +-- PermissionError
          |
          +-- ValueError
          |
          +-- TypeError
```

This is why:

```python
except Exception:
```

can catch many different runtime exceptions.

***

## 52. Practical Example 1 — Safe Division

Write a program that:

1. Gets two numbers.
2. Divides them.
3. Handles invalid input.
4. Handles division by zero.

Solution:

```python
try:

    num1 = float(input("Enter first number: "))
    num2 = float(input("Enter second number: "))

    result = num1 / num2

except ValueError:
    print("Please enter valid numbers")

except ZeroDivisionError:
    print("Cannot divide by zero")

else:
    print("Result:", result)

finally:
    print("Calculation completed")
```

***

## 53. Practical Example 2 — Safe File Reading

```python
file_name = input("Enter file name: ")

try:

    with open(file_name, "r", encoding="utf-8") as file:
        data = file.read()

except FileNotFoundError:
    print("File does not exist")

except PermissionError:
    print("Permission denied")

else:
    print("File successfully read")
    print(data)

finally:
    print("File operation completed")
```

***

## 54. Practical Example 3 — Process Records

```python
records = ["100", "200", "ABC", "300", "XYZ"]

for record in records:

    try:

        number = int(record)

        print("Processed:", number)

    except ValueError:

        print("Invalid record:", record)
```

Output:

```
Processed: 100
Processed: 200
Invalid record: ABC
Processed: 300
Invalid record: XYZ
```

Notice that the bad record does not stop the entire process.

***

## 55. Practical Example 4 — Data Engineering Pipeline

```python
def extract():
    print("Extracting data...")
    return ["100", "200", "ABC", "300"]


def transform(data):

    transformed = []

    for value in data:

        try:
            number = int(value)
            transformed.append(number * 2)

        except ValueError:
            print("Invalid record:", value)

    return transformed


def load(data):
    print("Loading data:", data)


try:

    data = extract()

    transformed_data = transform(data)

    load(transformed_data)

except Exception as error:

    print("Pipeline failed:", error)
```

Output:

```
Extracting data...
Invalid record: ABC
Loading data: [200, 400, 600]
```

Here:

```
Extract
   ↓
Raw Data
   ↓
Transform
   ↓
Bad Record → Skip + Log
   ↓
Valid Records
   ↓
Load
```

***

## 56. Practical Example 5 — Create a Custom Validation Exception

```python
class InvalidAmountError(Exception):
    pass


def validate_amount(amount):

    if amount < 0:
        raise InvalidAmountError("Amount cannot be negative")

    return True


try:

    amount = -500

    validate_amount(amount)

except InvalidAmountError as error:

    print("Validation Error:", error)
```

Output:

```
Validation Error: Amount cannot be negative
```

***

## 57. Practical Example 6 — Reusable Safe Conversion Function

```python
def convert_to_integer(value):

    try:
        return int(value)

    except ValueError:
        return None
```

Use it:

```python
values = ["100", "200", "ABC", "300"]

for value in values:

    result = convert_to_integer(value)

    if result is None:
        print("Invalid:", value)

    else:
        print("Valid:", result)
```

Output:

```
Valid: 100
Valid: 200
Invalid: ABC
Valid: 300
```

This is a good way to isolate error-handling logic inside reusable functions.

***

## 58. Important Best Practices

### 1. Catch Specific Exceptions

Prefer:

```python
except ValueError:
```

instead of:

```python
except:
```

***

### 2. Don't Hide Errors

Avoid:

```python
except:
    pass
```

This silently ignores the problem.

Bad:

```python
try:
    process_data()

except:
    pass
```

You may never know that the pipeline failed.

***

### 3. Log Important Exceptions

```python
except Exception as error:
    logging.error("Processing failed: %s", error)
```

***

### 4. Don't Use Exceptions for Everything

Don't write:

```python
try:
    if age < 18:
        ...
except:
    ...
```

Use normal conditions for normal business rules.

***

### 5. Keep `try` Blocks Small

Instead of:

```python
try:
    # 50 lines of code
```

prefer:

```python
try:
    value = int(data)
except ValueError:
    ...
```

This makes it easier to identify what caused the exception.

***

## 59. Exception Handling Mental Model

Remember:

```
try
 ↓
Run risky code
 ↓
Exception?
 │
 ├── No
 │    ↓
 │   else
 │
 └── Yes
      ↓
    except
      ↓
    Recover / Log / Skip / Retry / Stop
      ↓
finally
      ↓
Always execute
```

***

## 60. Exception Handling Cheat Sheet

```python
# Basic

try:
    risky_code()

except ValueError:
    handle_error()
```

```python
# Multiple exceptions

try:
    risky_code()

except ValueError:
    handle_value_error()

except TypeError:
    handle_type_error()
```

```python
# Exception object

try:
    risky_code()

except Exception as error:
    print(error)
```

```python
# else

try:
    risky_code()

except ValueError:
    handle_error()

else:
    success()
```

```python
# finally

try:
    risky_code()

except ValueError:
    handle_error()

finally:
    cleanup()
```

```python
# Raise

raise ValueError("Invalid value")
```

```python
# Custom exception

class MyError(Exception):
    pass
```

***

## 61. Most Important Exceptions for Data Engineers

You should be particularly comfortable with:

```
ValueError
TypeError
KeyError
IndexError
FileNotFoundError
PermissionError
FileExistsError
OSError
ConnectionError
TimeoutError
```

These commonly appear while processing:

```
Files
CSV
JSON
APIs
Databases
Networks
Data validation
ETL pipelines
```

***

## 62. Exception Handling in a Real ETL Pipeline

A simplified production-style flow:

```
                 ┌──────────────┐
                 │    START     │
                 └──────┬───────┘
                        ↓
                 ┌──────────────┐
                 │   EXTRACT    │
                 └──────┬───────┘
                        ↓
                  Exception?
                   /       \
                 Yes        No
                 ↓           ↓
              Log/Retry   Transform
                             ↓
                       Exception?
                        /       \
                      Yes        No
                      ↓           ↓
                   Log/Skip     Load
                                 ↓
                           Exception?
                            /       \
                          Yes        No
                          ↓           ↓
                       Retry/Fail   Success
```

The key idea is:

> Exception handling allows a pipeline to respond intelligently when something goes wrong.

***

## 63. Final Key Takeaways

1. An **error** is a broad term for a problem in a program.
2. An **exception** is a runtime event that interrupts normal execution.
3. Python uses:

```python
try
except
else
finally
```

for exception handling.

4. `try` contains code that may fail.
5. `except` handles the exception.
6. `else` runs when no exception occurs.
7. `finally` runs whether an exception occurs or not.
8. We can recover from exceptions by:

```
Retry
Skip bad record
Use default value
Ask for input again
Log the error
Stop the pipeline
```

9. Important exceptions include:

```
ValueError
TypeError
KeyError
IndexError
ZeroDivisionError
FileNotFoundError
PermissionError
OSError
ConnectionError
```

10. Prefer specific exception handling:

```python
except ValueError:
```

instead of:

```python
except:
```

11. We can inspect an exception:

```python
except Exception as error:
    print(error)
```

12. We can deliberately raise an exception:

```python
raise ValueError("Invalid data")
```

13. We can create custom exceptions:

```python
class InvalidDataError(Exception):
    pass
```

14. In Data Engineering, exception handling is essential for building **reliable ETL pipelines**.

The practical mindset is:

```
Something goes wrong
        ↓
Detect
        ↓
Catch
        ↓
Understand
        ↓
Recover / Retry / Skip / Log / Stop
        ↓
Continue safely
```
