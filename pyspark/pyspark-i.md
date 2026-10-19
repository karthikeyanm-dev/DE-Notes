# Pyspark - I

> Source notebook: [pyspark/Pyspark - I.ipynb](https://github.com/karthikeyanm-dev/DE-Notes/blob/main/pyspark/Pyspark%20-%20I.ipynb). Notebook outputs are omitted; open the source to run cells.

## PySpark Learning Notes — Checkpoint 1

### Scope

This checkpoint covers:

1. SparkSession vs SparkContext
2. SparkConf
3. SparkContext
4. `local[1]`, `local[2]`, `local[*]`
5. What happens when SparkContext starts
6. RDD fundamentals
7. Creating RDDs with `parallelize()`
8. Partitions
9. `map()`
10. `filter()`
11. `flatMap()`
12. RDD Actions

***

## 1. SparkSession vs SparkContext

### SparkSession

#### Definition

> SparkSession is the modern entry point for working with Spark DataFrames, Spark SQL, and general PySpark applications.

Example:

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("MyApp") \
    .master("local[*]") \
    .getOrCreate()
```

SparkSession is normally the starting point for modern PySpark applications.

***

### SparkContext

#### Definition

> SparkContext is the lower-level entry point to Spark Core and is mainly associated with RDD operations and the Spark execution environment.

Example:

```python
sc = spark.sparkContext
```

You can then create an RDD:

```python
rdd = sc.parallelize([1, 2, 3, 4])
```

***

### Relationship

```
SparkSession
     |
     └── SparkContext
             |
             └── RDD
```

Remember:

```
SparkSession → DataFrames / SQL / modern PySpark
SparkContext → Spark Core / RDD-level functionality
```

A SparkSession can give access to its underlying SparkContext:

```python
sc = spark.sparkContext
```

A SparkContext by itself is not the normal interface for creating DataFrames. DataFrames are created through SparkSession.

***

## 2. SparkConf

#### Definition

> SparkConf is a configuration object used to specify settings for a Spark application.

Example:

```python
from pyspark import SparkConf

conf = SparkConf()

conf.setAppName("CustomerAnalysis")
conf.setMaster("local[*]")
```

SparkConf can contain settings such as:

* Application name
* Master
* Executor settings
* Memory settings
* Serialization settings
* Other Spark configurations

Important:

> SparkConf stores configuration; it does not execute the Spark application.

Conceptually:

```
SparkConf
   |
   ├── appName
   ├── master
   ├── memory settings
   └── other configurations
```

***

## 3. SparkContext

#### Definition

> SparkContext represents the connection between a Spark application and the Spark execution environment and provides access to Spark Core functionality such as RDDs.

Low-level example:

```python
from pyspark import SparkConf, SparkContext

conf = SparkConf() \
    .setAppName("MyApp") \
    .setMaster("local[*]")

sc = SparkContext(conf=conf)
```

You can inspect:

```python
print(sc)
print(sc.appName)
print(sc.master)
```

Possible output:

```
MyApp
local[*]
```

RDD example:

```python
rdd = sc.parallelize([10, 20, 30, 40])
```

***

## 4. `local[1]`, `local[2]`, `local[*]`

The `master` setting determines where/how Spark runs.

### `local[1]`

```python
.master("local[1]")
```

Means Spark runs locally using one execution thread.

```
Your Computer
     |
   Spark
     |
  1 thread
```

***

### `local[2]`

```python
.master("local[2]")
```

Means Spark runs locally using two execution threads.

```
Your Computer
     |
   Spark
   /   \
  T1   T2
```

***

### `local[*]`

```python
.master("local[*]")
```

The `*` means Spark can use all available logical CPU cores for local execution.

For example, if the system exposes 8 logical processors:

```
local[*]
   |
   ├── Core 1
   ├── Core 2
   ├── Core 3
   ├── ...
   └── Core 8
```

For local learning, `local[*]` is commonly convenient.

***

## 5. What happens when SparkContext starts?

Consider:

```python
from pyspark import SparkConf, SparkContext

conf = SparkConf() \
    .setAppName("MyApp") \
    .setMaster("local[*]")

sc = SparkContext(conf=conf)
```

### Step 1 — Create configuration

```python
conf = SparkConf()
```

The configuration object contains settings such as:

```
appName = MyApp
master  = local[*]
```

### Step 2 — Create SparkContext

```python
sc = SparkContext(conf=conf)
```

Spark creates the execution environment according to the supplied configuration.

### Step 3 — Determine execution mode

Because:

```python
.setMaster("local[*]")
```

was specified, Spark knows that the application is running locally.

### Step 4 — `sc` becomes the handle to Spark Core

You can use:

```python
rdd = sc.parallelize([1, 2, 3, 4])
```

Conceptually:

```
Python Application
       |
       ↓
SparkContext
       |
       ↓
Spark Core
       |
       ↓
RDD
```

***

## 6. RDD

### Definition

> RDD (Resilient Distributed Dataset) is a distributed collection of data that Spark can process in parallel across partitions and recover through lineage when necessary.

The three words in RDD:

#### Resilient

Resilient means fault-tolerant.

If a partition is lost, Spark can recompute it using lineage.

#### Distributed

Data can be divided into partitions and processed in parallel.

#### Dataset

A collection of data.

***

## 7. Basic RDD Characteristics

An RDD is:

* Distributed
* Partitioned
* Immutable
* Fault-tolerant
* Lazy
* Parallelizable

### Distributed

An RDD can be divided into partitions:

```
RDD
|
├── Partition 1
├── Partition 2
└── Partition 3
```

### Partitioned

A partition is a logical division of an RDD.

Example:

```
RDD = [1,2,3,4,5,6,7,8]

Partition 1 → [1,2]
Partition 2 → [3,4]
Partition 3 → [5,6]
Partition 4 → [7,8]
```

### Immutable

An existing RDD is not directly modified.

Instead, an operation creates a new RDD:

```
rdd1
  |
  | operation
  ↓
rdd2
```

### Fault-tolerant

Spark maintains lineage information describing how an RDD was produced.

If a partition is lost, Spark can use that information to recompute it.

### Lazy

RDD transformations are not immediately executed.

Example:

```python
new_rdd = rdd.map(lambda x: x * 2)
```

Spark records the operation.

Execution happens when an action is called.

### Parallelizable

Partitions can be processed concurrently.

***

## 8. Creating an RDD with `parallelize()`

The simplest way to create an RDD from Python data is:

```python
data = [10, 20, 30, 40, 50]

rdd = sc.parallelize(data)
```

Conceptually:

```
Python Collection
[10,20,30,40,50]
       |
       | parallelize()
       ↓
      RDD
```

The syntax is:

```python
sc.parallelize(collection, numSlices)
```

Where:

* `collection` → local Python collection
* `numSlices` → requested number of partitions

Example:

```python
rdd = sc.parallelize([10, 20, 30, 40, 50], 2)
```

***

## 9. Inspecting an RDD

### Type

```python
print(type(rdd))
```

You should see something similar to:

```
<class 'pyspark.core.rdd.RDD'>
```

### Number of partitions

```python
print(rdd.getNumPartitions())
```

### Data

```python
print(rdd.collect())
```

Example output:

```
[10, 20, 30, 40, 50]
```

`collect()` is an action and brings all elements back to the driver.

***

## 10. Understanding Partitions with `glom()`

`glom()` is useful for learning how data is distributed between partitions.

Example:

```python
rdd = sc.parallelize([10, 20, 30, 40, 50], 2)

print(rdd.glom().collect())
```

Conceptually:

```
Partition 0 → [10,20,30]
Partition 1 → [40,50]
```

Possible output:

```
[[10, 20, 30], [40, 50]]
```

The exact distribution can depend on the partitioning behavior, so use `glom()` to inspect it rather than assuming a particular split.

***

### More examples

#### 3 partitions

```python
rdd = sc.parallelize([10, 20, 30, 40, 50, 60], 3)

print(rdd.glom().collect())
```

Conceptually:

```
Partition 0 → [10,20]
Partition 1 → [30,40]
Partition 2 → [50,60]
```

#### More partitions than elements

```python
rdd = sc.parallelize([10, 20, 30], 5)

print(rdd.glom().collect())
```

Some partitions can be empty.

Important:

> The number of partitions does not have to equal the number of elements.

***

## 11. Why Partitions Matter

Spark performs work at the partition level.

Example:

```
RDD
|
├── P0 → [1,2,3]
├── P1 → [4,5,6]
├── P2 → [7,8,9]
└── P3 → [10,11,12]
```

Conceptually, Spark can create work for each partition:

```
P0 → Task
P1 → Task
P2 → Task
P3 → Task
```

This is the foundation of Spark's parallel processing model.

Later this will connect:

```
Partition
    ↓
Task
    ↓
Executor
```

***

## 12. `collect()`

`collect()` returns all elements of an RDD to the driver.

Example:

```python
rdd = sc.parallelize([1, 2, 3, 4, 5])

result = rdd.collect()

print(result)
```

Output:

```
[1, 2, 3, 4, 5]
```

Conceptually:

```
Executors
    |
    | all data
    ↓
Driver
```

Important:

> Do not use `collect()` carelessly on very large datasets because all returned data must fit in driver memory.

For small learning datasets, it is fine.

***

## 13. Generic RDD Processing Model

Suppose:

```python
rdd = sc.parallelize([1, 2, 3, 4, 5, 6], 3)
```

Conceptually:

```
RDD
|
├── P0 → [1,2]
├── P1 → [3,4]
└── P2 → [5,6]
```

When Spark performs an element-level operation, it processes elements within these partitions.

```
P0 → operation → result
P1 → operation → result
P2 → operation → result
```

This partition-based processing is fundamental to Spark.

***

## 14. `map()`

### Definition

> `map()` is an RDD transformation that applies a function to every element and produces a new RDD containing the transformed elements.

Example:

```python
rdd = sc.parallelize([1, 2, 3, 4, 5])

new_rdd = rdd.map(lambda x: x * 2)

print(new_rdd.collect())
```

Output:

```
[2, 4, 6, 8, 10]
```

Conceptually:

```
1 → 2
2 → 4
3 → 6
4 → 8
5 → 10
```

### `map()` with partitions

```python
rdd = sc.parallelize([1, 2, 3, 4, 5, 6], 3)

new_rdd = rdd.map(lambda x: x * 10)
```

Conceptually:

```
Original:

P0 → [1,2]
P1 → [3,4]
P2 → [5,6]

After map:

P0 → [10,20]
P1 → [30,40]
P2 → [50,60]
```

`map()` normally preserves the number of partitions.

### Important properties

* `map()` is a transformation.
* It applies a function to every element.
* It creates a new RDD.
* It is lazy.
* It normally produces one output element for each input element.

***

## 15. `filter()`

### Definition

> `filter()` is an RDD transformation that keeps only the elements for which the supplied condition returns `True`.

Example:

```python
rdd = sc.parallelize([1, 2, 3, 4, 5, 6])

new_rdd = rdd.filter(lambda x: x % 2 == 0)

print(new_rdd.collect())
```

Output:

```
[2, 4, 6]
```

Conceptually:

```
1 → False → remove
2 → True  → keep
3 → False → remove
4 → True  → keep
5 → False → remove
6 → True  → keep
```

### Difference between `map()` and `filter()`

```
map()
→ transforms elements

filter()
→ selects elements
```

Example:

```python
rdd.map(lambda x: x * 10)

rdd.filter(lambda x: x > 10)
```

`filter()` is also lazy.

***

## 16. `flatMap()`

### Definition

> `flatMap()` applies a function to every element and then flattens the resulting collections into a single RDD.

The key difference:

```
map()     → one input → one output
flatMap() → one input → zero, one, or many outputs
```

Example:

```python
data = ["hello world", "spark is powerful"]

rdd = sc.parallelize(data)

words = rdd.flatMap(lambda x: x.split())

print(words.collect())
```

Output:

```
['hello', 'world', 'spark', 'is', 'powerful']
```

For:

```
"hello world"
```

`split()` produces:

```
["hello", "world"]
```

For:

```
"spark is powerful"
```

it produces:

```
["spark", "is", "powerful"]
```

`flatMap()` flattens the result:

```
["hello", "world", "spark", "is", "powerful"]
```

***

### `map()` vs `flatMap()`

With `map()`:

```python
rdd.map(lambda x: x.split())
```

Result is conceptually:

```
[
    ["hello", "world"],
    ["spark", "is", "powerful"]
]
```

With `flatMap()`:

```python
rdd.flatMap(lambda x: x.split())
```

Result is:

```
["hello", "world", "spark", "is", "powerful"]
```

Another example:

```python
rdd = sc.parallelize([
    [1, 2],
    [3, 4],
    [5, 6]
])

result = rdd.flatMap(lambda x: x)

print(result.collect())
```

Output:

```
[1, 2, 3, 4, 5, 6]
```

***

## 17. RDD Actions

### Definition

> An action is an RDD operation that triggers Spark to execute the required transformations and produces a result or performs an output operation.

Example:

```python
rdd2 = rdd.map(lambda x: x * 2)
```

At this point, the transformation is lazy.

Then:

```python
result = rdd2.collect()
```

The action triggers execution.

Conceptually:

```
RDD
 ↓
map()
 ↓
New RDD
 ↓
collect()
 ↓
Execution
```

***

## 18. Common RDD Actions

### `collect()`

Returns all elements.

```python
rdd.collect()
```

Example:

```python
[1, 2, 3, 4, 5]
```

***

### `count()`

Returns the number of elements.

```python
rdd.count()
```

Example:

```python
rdd = sc.parallelize([10, 20, 30, 40, 50])

print(rdd.count())
```

Output:

```
5
```

***

### `first()`

Returns the first element.

```python
print(rdd.first())
```

Output:

```
10
```

***

### `take(n)`

Returns the first `n` elements.

```python
print(rdd.take(3))
```

Output:

```
[10, 20, 30]
```

`take()` is useful when you only want to inspect a small amount of data.

***

### `reduce()`

Combines elements using a function.

Example:

```python
rdd = sc.parallelize([1, 2, 3, 4, 5])

result = rdd.reduce(lambda a, b: a + b)

print(result)
```

Output:

```
15
```

Conceptually:

```
1 + 2 = 3
3 + 3 = 6
6 + 4 = 10
10 + 5 = 15
```

In distributed Spark execution, reduction can occur within partitions and then combine partial results.

***

### `foreach()`

Executes a function for each element.

```python
rdd.foreach(lambda x: print(x))
```

Important:

> `foreach()` runs the function on the executors, so printed output is not necessarily displayed in the same way as normal driver-side Python `print()` output.

***

## 19. Transformations vs Actions

This distinction is fundamental.

| Transformation      | Action             |
| ------------------- | ------------------ |
| `map()`             | `collect()`        |
| `filter()`          | `count()`          |
| `flatMap()`         | `first()`          |
| Creates another RDD | Produces a result  |
| Lazy                | Triggers execution |

Example:

```python
rdd2 = rdd.map(lambda x: x * 2)

result = rdd2.collect()
```

The execution model is:

```
Transformation
      ↓
Build the processing plan
      ↓
Transformation
      ↓
Build the processing plan
      ↓
Action
      ↓
EXECUTE
```

This is the foundation of Spark's lazy evaluation.

***

## 20. Complete Checkpoint Example

```python
from pyspark.sql import SparkSession

# Create SparkSession
spark = SparkSession.builder \
    .appName("RDD Checkpoint") \
    .master("local[*]") \
    .getOrCreate()

# Get SparkContext
sc = spark.sparkContext

# Create RDD
data = [1, 2, 3, 4, 5, 6]

rdd = sc.parallelize(data, 3)

# Inspect partitions
print("Partitions:", rdd.getNumPartitions())
print("Partition data:", rdd.glom().collect())

# Transformation: map
mapped_rdd = rdd.map(lambda x: x * 10)

# Transformation: filter
filtered_rdd = mapped_rdd.filter(lambda x: x > 20)

# Action
result = filtered_rdd.collect()

print("Result:", result)
```

Conceptually:

```
Python List
     |
     | parallelize()
     ↓
    RDD
     |
     | 3 partitions
     ↓
 P0   P1   P2
  |    |    |
  └────┼────┘
       |
      map()
       |
       ↓
     RDD
       |
    filter()
       |
       ↓
    Action
       |
    collect()
       |
       ↓
    Driver
```

***

## Current Learning Position

You have now covered:

```
SparkSession
    ↓
SparkContext
    ↓
SparkConf
    ↓
local[*]
    ↓
RDD
    ↓
Partitions
    ↓
parallelize()
    ↓
map()
    ↓
filter()
    ↓
flatMap()
    ↓
Actions
    ↓
collect()
    ↓
count()
    ↓
first()
    ↓
take()
    ↓
reduce()
    ↓
foreach()
```

### Next checkpoint

The next major topic is:

```
Lazy Evaluation
      ↓
Lineage
      ↓
DAG
      ↓
Job
      ↓
Stage
      ↓
Task
```

This is where we move from **"how to use RDDs"** to **"how Spark actually executes RDD code."**
