# RDD Transformations Actions

> Source notebook: [pyspark/RDD\_Transformations\_Actions.ipynb](https://github.com/karthikeyanm-dev/DE-Notes/blob/main/pyspark/RDD_Transformations_Actions.ipynb). Notebook outputs are omitted; open the source to run cells.

## PySpark RDD --- Transformations and Actions In-Depth

This document focuses on exactly two areas:

1. **RDD Transformations**
2. **RDD Actions**

For every important method, the explanation covers:

* Definition
* Method signature
* Parameters
* What each parameter means
* Input and output structure
* Detailed code breakdown
* Execution behavior
* Practical use case
* Important notes and common mistakes

***

## PART 1 --- RDD TRANSFORMATIONS

### 1. What Is an RDD Transformation?

A transformation is an operation that creates a **new RDD from an existing RDD**.

Example:

```python
rdd2 = rdd1.map(lambda x: x * 2)
```

Here:

```
rdd1
  |
  | map()
  v
rdd2
```

The original RDD is not modified.

RDDs are immutable.

#### Important characteristic: Lazy Evaluation

This code:

```python
rdd2 = rdd1.map(lambda x: x * 2)
```

does not normally execute the computation immediately.

Spark records the transformation.

Execution happens when an action such as:

```python
rdd2.collect()
```

is called.

Conceptually:

```
Transformation
      |
      v
Transformation
      |
      v
Transformation
      |
      v
Action
      |
      v
Spark executes the lineage
```

***

## 2. map()

### Definition

`map()` applies a function to every element of an RDD and returns a new RDD.

### Signature

```python
RDD.map(f, preservesPartitioning=False)
```

### Parameters

#### `f`

A Python function that accepts one element and returns the transformed element.

Example:

```python
lambda x: x * 2
```

#### `preservesPartitioning`

Boolean value.

```python
False
```

by default.

This is mainly relevant to Pair RDDs. If `True`, Spark can preserve an existing partitioner when the transformation does not change the key structure.

For ordinary beginner RDDs, leave this as the default.

### Example

```python
rdd = sc.parallelize([1, 2, 3, 4])

result = rdd.map(lambda x: x * 2)

print(result.collect())
```

Output:

```
[2, 4, 6, 8]
```

### Deep Breakdown

```python
rdd = sc.parallelize([1, 2, 3, 4])
```

Creates:

```
RDD
 |
 +-- 1
 +-- 2
 +-- 3
 +-- 4
```

Then:

```python
rdd.map(lambda x: x * 2)
```

Spark conceptually applies:

```
1 -> 2
2 -> 4
3 -> 6
4 -> 8
```

The lambda:

```python
lambda x: x * 2
```

is equivalent to:

```python
def multiply(x):
    return x * 2
```

Therefore:

```python
rdd.map(multiply)
```

does the same thing.

### One-to-One Behavior

Normally:

```
1 input → 1 output
```

Example:

```
[1, 2, 3]

map(x * 10)

[10, 20, 30]
```

### Important Point

`map()` is a transformation, so this:

```python
result = rdd.map(lambda x: x * 2)
```

does not by itself force the final result to be calculated.

This does:

```python
result.collect()
```

***

## 3. flatMap()

### Definition

`flatMap()` applies a function to every element and then flattens the returned collections.

### Signature

```python
RDD.flatMap(f, preservesPartitioning=False)
```

### Parameters

#### `f`

Function applied to every element.

#### `preservesPartitioning`

Boolean indicating whether the partitioner should be preserved.

Usually left as:

```python
False
```

### Example

```python
rdd = sc.parallelize([
    "Python Spark",
    "RDD is powerful"
])

result = rdd.flatMap(lambda line: line.split())

print(result.collect())
```

Output:

```
['Python', 'Spark', 'RDD', 'is', 'powerful']
```

### Deep Breakdown

Input:

```
"Python Spark"
"RDD is powerful"
```

The function:

```python
lambda line: line.split()
```

produces:

```
"Python Spark"       -> ["Python", "Spark"]

"RDD is powerful"   -> ["RDD", "is", "powerful"]
```

Without flattening, the result would conceptually look like:

```
[
    ["Python", "Spark"],
    ["RDD", "is", "powerful"]
]
```

`flatMap()` flattens it:

```
[
    "Python",
    "Spark",
    "RDD",
    "is",
    "powerful"
]
```

### map() vs flatMap()

```python
rdd.map(lambda x: x.split())
```

produces nested collections.

```python
rdd.flatMap(lambda x: x.split())
```

produces a single flattened sequence.

### Practical Use

Very common for:

* Word processing
* Log processing
* Tokenization
* Parsing multiple records from one input record

***

## 4. filter()

### Definition

`filter()` keeps only elements for which the supplied function returns `True`.

### Signature

```python
RDD.filter(f)
```

### Parameter

#### `f`

Function returning a Boolean.

Example:

```python
lambda x: x > 10
```

### Example

```python
rdd = sc.parallelize([10, 15, 20, 25, 30])

result = rdd.filter(lambda x: x > 20)

print(result.collect())
```

Output:

```
[25, 30]
```

### Deep Breakdown

Spark evaluates:

```
10 > 20 -> False -> discard
15 > 20 -> False -> discard
20 > 20 -> False -> discard
25 > 20 -> True  -> keep
30 > 20 -> True  -> keep
```

Result:

```
[25, 30]
```

### Another Example

```python
students = sc.parallelize([
    ("Arun", 75),
    ("Priya", 45),
    ("Karthik", 82)
])

passed = students.filter(lambda x: x[1] >= 50)

print(passed.collect())
```

Output:

```
[('Arun', 75), ('Karthik', 82)]
```

Here:

```python
x[0]
```

is the name.

```python
x[1]
```

is the mark.

***

## 5. mapPartitions()

### Definition

`mapPartitions()` applies a function to an entire partition rather than calling the function separately for every element.

### Signature

```python
RDD.mapPartitions(f, preservesPartitioning=False)
```

### Parameters

#### `f`

A function receiving an iterator containing the partition's elements.

#### `preservesPartitioning`

Whether to preserve the partitioner.

### Example

```python
def process_partition(iterator):
    for x in iterator:
        yield x * 10

rdd = sc.parallelize([1, 2, 3, 4], 2)

result = rdd.mapPartitions(process_partition)

print(result.collect())
```

Output:

```
[10, 20, 30, 40]
```

### Why Does the Function Receive an Iterator?

Because Spark processes data partition-by-partition.

Conceptually:

```
Partition 0
[1, 2]

Partition 1
[3, 4]
```

The function receives:

```
Partition 0 iterator -> 1, 2
Partition 1 iterator -> 3, 4
```

### Important Practical Advantage

Suppose you need a database connection.

Bad pattern:

```python
rdd.map(lambda row: create_connection_and_save(row))
```

This can repeatedly create expensive resources.

Better pattern:

```python
def save_partition(iterator):
    connection = create_connection()

    for row in iterator:
        save(connection, row)

    connection.close()

rdd.foreachPartition(save_partition)
```

One resource can be created per partition.

***

## 6. mapPartitionsWithIndex()

### Definition

Processes each partition while also providing the partition index.

### Signature

```python
RDD.mapPartitionsWithIndex(f, preservesPartitioning=False)
```

### Parameters

#### `f`

Function receiving:

```
partition_index
iterator
```

#### `preservesPartitioning`

Whether to preserve the existing partitioner.

### Example

```python
def inspect_partition(index, iterator):
    for value in iterator:
        yield (index, value)

rdd = sc.parallelize([10, 20, 30, 40], 2)

result = rdd.mapPartitionsWithIndex(inspect_partition)

print(result.collect())
```

Possible output:

```
[(0, 10), (0, 20), (1, 30), (1, 40)]
```

Partition distribution can vary depending on the partitioning.

***

## 7. glom()

### Definition

`glom()` converts the contents of each partition into a list.

### Signature

```python
RDD.glom()
```

No parameters.

### Example

```python
rdd = sc.parallelize([1, 2, 3, 4, 5, 6], 2)

result = rdd.glom()

print(result.collect())
```

Possible output:

```
[[1, 2, 3], [4, 5, 6]]
```

### Why Is It Useful?

It is particularly useful for understanding:

* Partitions
* Data distribution
* Partition-level debugging

***

## 8. mapValues()

### Definition

`mapValues()` transforms the values of a Pair RDD without changing its keys.

### Signature

```python
PairRDD.mapValues(f)
```

### Example

```python
rdd = sc.parallelize([
    ("A", 10),
    ("B", 20),
    ("C", 30)
])

result = rdd.mapValues(lambda x: x * 2)

print(result.collect())
```

Output:

```
[('A', 20), ('B', 40), ('C', 60)]
```

### Deep Breakdown

Input:

```
(A, 10)
(B, 20)
(C, 30)
```

Function is applied only to:

```
10
20
30
```

Keys remain:

```
A
B
C
```

Result:

```
(A, 20)
(B, 40)
(C, 60)
```

### Difference from map()

Using `map()`:

```python
rdd.map(lambda x: (x[0], x[1] * 2))
```

Using `mapValues()`:

```python
rdd.mapValues(lambda x: x * 2)
```

`mapValues()` expresses the intention more clearly and can preserve partitioning.

***

## 9. flatMapValues()

### Definition

Applies a function to values of a Pair RDD and creates multiple values while keeping the key.

### Example

```python
rdd = sc.parallelize([
    ("A", [10, 20]),
    ("B", [30, 40])
])

result = rdd.flatMapValues(lambda values: values)

print(result.collect())
```

Output:

```
[('A', 10), ('A', 20), ('B', 30), ('B', 40)]
```

The key is repeated for each generated value.

***

## 10. keys()

### Definition

Returns only keys from a Pair RDD.

### Signature

```python
PairRDD.keys()
```

### Example

```python
rdd = sc.parallelize([
    ("A", 10),
    ("B", 20),
    ("C", 30)
])

print(rdd.keys().collect())
```

Output:

```
['A', 'B', 'C']
```

***

## 11. values()

### Definition

Returns only values from a Pair RDD.

### Example

```python
rdd = sc.parallelize([
    ("A", 10),
    ("B", 20),
    ("C", 30)
])

print(rdd.values().collect())
```

Output:

```
[10, 20, 30]
```

***

## 12. keyBy()

### Definition

Creates a Pair RDD by calculating a key for each element.

### Example

```python
rdd = sc.parallelize([
    "Arun",
    "Bala",
    "Anitha"
])

result = rdd.keyBy(lambda x: x[0])

print(result.collect())
```

Output:

```
[
    ('A', 'Arun'),
    ('B', 'Bala'),
    ('A', 'Anitha')
]
```

The function:

```python
lambda x: x[0]
```

takes the first character as the key.

***

## 13. reduceByKey()

### Definition

`reduceByKey()` combines values belonging to the same key using a reduction function.

### Signature

```python
PairRDD.reduceByKey(
    func,
    numPartitions=None,
    partitionFunc=<portable_hash>
)
```

### Parameters

#### `func`

Function used to combine two values.

Example:

```python
lambda a, b: a + b
```

The function receives **values**, not complete `(key, value)` tuples.

#### `numPartitions`

Optional number of output partitions.

If not supplied, Spark uses the existing partitioning information or Spark's default behavior.

Example:

```python
rdd.reduceByKey(lambda a, b: a + b, numPartitions=4)
```

#### `partitionFunc`

Function used to determine which partition receives a key.

Normally you do not need to customize it.

### Example

```python
rdd = sc.parallelize([
    ("Python", 1),
    ("Java", 1),
    ("Python", 1),
    ("Python", 1)
])

result = rdd.reduceByKey(lambda a, b: a + b)

print(result.collect())
```

Output:

```
[('Python', 3), ('Java', 1)]
```

### Deep Breakdown

Input:

```
Python -> 1
Java   -> 1
Python -> 1
Python -> 1
```

For `Python`:

```
1 + 1 + 1 = 3
```

For `Java`:

```
1 = 1
```

Result:

```
Python -> 3
Java   -> 1
```

### Why Is It Called reduceByKey?

Because:

```
reduce
+
by key
```

It reduces all values belonging to each key.

### Shuffle

`reduceByKey()` is a wide transformation.

Conceptually:

```
Partition 1       Partition 2
Python 1          Python 1
Python 1          Java 1
       \            /
        \          /
          SHUFFLE
             |
             v
       Python -> 3
       Java   -> 1
```

### Map-Side Combining

Spark can combine values locally before sending them over the network.

Example:

```
Partition 1:
Python 1
Python 1

Local combine:
Python -> 2
```

Then only `2` needs to be shuffled instead of two separate records.

This can significantly reduce network traffic.

***

## 14. groupByKey()

### Definition

Groups all values belonging to the same key.

### Signature

```python
PairRDD.groupByKey(
    numPartitions=None,
    partitionFunc=<portable_hash>
)
```

### Parameters

#### `numPartitions`

Controls the number of partitions used for the resulting grouped RDD.

#### `partitionFunc`

Determines how keys are assigned to partitions.

### Example

```python
rdd = sc.parallelize([
    ("IT", "Arun"),
    ("IT", "Karthik"),
    ("HR", "Priya"),
    ("HR", "Anitha")
])

result = rdd.groupByKey()

print(result.mapValues(list).collect())
```

Output:

```
[
    ('IT', ['Arun', 'Karthik']),
    ('HR', ['Priya', 'Anitha'])
]
```

### Deep Breakdown

Before:

```
IT -> Arun
IT -> Karthik
HR -> Priya
HR -> Anitha
```

After grouping:

```
IT -> [Arun, Karthik]
HR -> [Priya, Anitha]
```

### Important Difference

`groupByKey()` groups values.

`reduceByKey()` combines values.

Use:

```python
reduceByKey(...)
```

when you need aggregation.

Use:

```python
groupByKey()
```

when you genuinely need all grouped values.

***

## 15. aggregateByKey()

### Definition

Performs custom aggregation for each key.

### Signature

```python
PairRDD.aggregateByKey(
    zeroValue,
    seqFunc,
    combFunc,
    numPartitions=None,
    partitionFunc=<portable_hash>
)
```

### Parameters

#### `zeroValue`

Initial value for the aggregation.

Example:

```python
0
```

or:

```python
(0, 0)
```

#### `seqFunc`

Combines a current accumulator with one input value.

#### `combFunc`

Combines accumulators from different partitions.

#### `numPartitions`

Optional output partition count.

#### `partitionFunc`

Partitioning function.

### Example

Calculate sum:

```python
rdd = sc.parallelize([
    ("A", 10),
    ("A", 20),
    ("B", 30),
    ("B", 40)
])

result = rdd.aggregateByKey(
    0,
    lambda acc, value: acc + value,
    lambda acc1, acc2: acc1 + acc2
)

print(result.collect())
```

Output:

```
[('A', 30), ('B', 70)]
```

### Why Two Functions?

Because Spark has two levels of aggregation.

#### Within a partition

```
seqFunc
```

#### Between partition results

```
combFunc
```

This makes `aggregateByKey()` more flexible than `reduceByKey()`.

***

## 16. foldByKey()

### Definition

Aggregates values by key using a zero value and the same combining function.

### Signature

```python
PairRDD.foldByKey(
    zeroValue,
    func,
    numPartitions=None,
    partitionFunc=<portable_hash>
)
```

### Example

```python
rdd = sc.parallelize([
    ("A", 10),
    ("A", 20),
    ("B", 30)
])

result = rdd.foldByKey(
    0,
    lambda a, b: a + b
)

print(result.collect())
```

Output:

```
[('A', 30), ('B', 30)]
```

***

## 17. combineByKey()

### Definition

`combineByKey()` is one of the most flexible Pair RDD aggregation operations.

### Signature

```python
PairRDD.combineByKey(
    createCombiner,
    mergeValue,
    mergeCombiners,
    numPartitions=None,
    partitionFunc=<portable_hash>
)
```

### Parameters

#### `createCombiner`

Converts the first value for a key into an initial accumulator.

#### `mergeValue`

Adds another value to an existing accumulator.

#### `mergeCombiners`

Combines accumulators from different partitions.

#### `numPartitions`

Controls resulting partitions.

#### `partitionFunc`

Controls key partitioning.

### Example: Average per Department

```python
rdd = sc.parallelize([
    ("IT", 50000),
    ("IT", 60000),
    ("HR", 40000),
    ("HR", 50000)
])

def create_combiner(value):
    return (value, 1)

def merge_value(accumulator, value):
    total, count = accumulator
    return (total + value, count + 1)

def merge_combiners(acc1, acc2):
    return (
        acc1[0] + acc2[0],
        acc1[1] + acc2[1]
    )

result = rdd.combineByKey(
    create_combiner,
    merge_value,
    merge_combiners
)

average = result.mapValues(
    lambda x: x[0] / x[1]
)

print(average.collect())
```

Output:

```
[('IT', 55000.0), ('HR', 45000.0)]
```

***

## 18. sortBy()

### Definition

Sorts an RDD according to a function.

### Signature

```python
RDD.sortBy(
    keyfunc,
    ascending=True,
    numPartitions=None
)
```

### Parameters

#### `keyfunc`

Function used to determine the sorting value.

#### `ascending`

Boolean.

```python
True
```

means ascending.

```python
False
```

means descending.

#### `numPartitions`

Number of partitions in the resulting RDD.

### Example

```python
rdd = sc.parallelize([
    ("Arun", 50000),
    ("Karthik", 80000),
    ("Priya", 60000)
])

result = rdd.sortBy(
    lambda x: x[1],
    ascending=False
)

print(result.collect())
```

Output:

```
[
    ('Karthik', 80000),
    ('Priya', 60000),
    ('Arun', 50000)
]
```

Here:

```python
lambda x: x[1]
```

means:

> Sort according to the salary.

***

## 19. sortByKey()

### Definition

Sorts a Pair RDD by key.

### Signature

```python
PairRDD.sortByKey(
    ascending=True,
    numPartitions=None,
    keyfunc=<identity>
)
```

### Parameters

#### `ascending`

Sort direction.

#### `numPartitions`

Number of resulting partitions.

#### `keyfunc`

Function applied to keys before comparison.

### Example

```python
rdd = sc.parallelize([
    (3, "C"),
    (1, "A"),
    (2, "B")
])

result = rdd.sortByKey()

print(result.collect())
```

Output:

```
[(1, 'A'), (2, 'B'), (3, 'C')]
```

***

## 20. distinct()

### Definition

Removes duplicate elements.

### Signature

```python
RDD.distinct(numPartitions=None)
```

### Parameter

#### `numPartitions`

Optional number of partitions for the resulting RDD.

### Example

```python
rdd = sc.parallelize([
    1, 2, 2, 3, 3, 3, 4
])

result = rdd.distinct()

print(result.collect())
```

Output:

```
[1, 2, 3, 4]
```

`distinct()` generally requires data redistribution because duplicate values can exist in different partitions.

***

## 21. union()

### Definition

Combines two RDDs.

### Signature

```python
RDD.union(other)
```

### Parameter

#### `other`

Another RDD compatible with the operation.

### Example

```python
rdd1 = sc.parallelize([1, 2, 3])
rdd2 = sc.parallelize([4, 5, 6])

result = rdd1.union(rdd2)

print(result.collect())
```

Output:

```
[1, 2, 3, 4, 5, 6]
```

### Important

`union()` does not automatically remove duplicates.

```python
rdd1 = sc.parallelize([1, 2])
rdd2 = sc.parallelize([2, 3])

print(rdd1.union(rdd2).collect())
```

Possible output:

```
[1, 2, 2, 3]
```

To remove duplicates:

```python
rdd1.union(rdd2).distinct()
```

***

## 22. intersection()

### Definition

Returns elements common to both RDDs.

### Signature

```python
RDD.intersection(other)
```

### Example

```python
rdd1 = sc.parallelize([1, 2, 3, 4])
rdd2 = sc.parallelize([3, 4, 5, 6])

print(rdd1.intersection(rdd2).collect())
```

Output:

```
[3, 4]
```

This requires coordination between the RDDs and can involve a shuffle.

***

## 23. subtract()

### Definition

Returns elements present in the first RDD but not in the second.

### Example

```python
rdd1 = sc.parallelize([1, 2, 3, 4])
rdd2 = sc.parallelize([3, 4])

print(rdd1.subtract(rdd2).collect())
```

Output:

```
[1, 2]
```

***

## 24. cartesian()

### Definition

Produces every possible pair between two RDDs.

### Example

```python
rdd1 = sc.parallelize([1, 2])
rdd2 = sc.parallelize(["A", "B"])

result = rdd1.cartesian(rdd2)

print(result.collect())
```

Output:

```
[
    (1, 'A'),
    (1, 'B'),
    (2, 'A'),
    (2, 'B')
]
```

If:

```
RDD1 has N elements
RDD2 has M elements
```

the Cartesian product can contain:

```
N × M
```

elements.

This can become extremely expensive.

***

## 25. join()

### Definition

Performs an inner join on two Pair RDDs.

### Example

```python
employees = sc.parallelize([
    (1, "Arun"),
    (2, "Priya"),
    (3, "Karthik")
])

salaries = sc.parallelize([
    (1, 50000),
    (2, 60000),
    (3, 70000)
])

result = employees.join(salaries)

print(result.collect())
```

Output:

```
[
    (1, ('Arun', 50000)),
    (2, ('Priya', 60000)),
    (3, ('Karthik', 70000))
]
```

Only matching keys are returned.

***

## 26. leftOuterJoin()

### Definition

Keeps every key from the left RDD.

```python
employees = sc.parallelize([
    (1, "Arun"),
    (2, "Priya"),
    (3, "Karthik")
])

salaries = sc.parallelize([
    (1, 50000),
    (3, 70000)
])

result = employees.leftOuterJoin(salaries)

print(result.collect())
```

Possible output:

```
[
    (1, ('Arun', 50000)),
    (2, ('Priya', None)),
    (3, ('Karthik', 70000))
]
```

`None` indicates that no matching value was found on the right.

***

## 27. rightOuterJoin()

### Definition

Keeps every key from the right RDD.

```python
result = employees.rightOuterJoin(salaries)

print(result.collect())
```

If a salary exists without an employee record, that employee side can contain `None`.

***

## 28. fullOuterJoin()

### Definition

Keeps keys from both RDDs.

```python
result = employees.fullOuterJoin(salaries)

print(result.collect())
```

Missing values are represented using `None`.

***

## 29. cogroup()

### Definition

Groups values from multiple Pair RDDs by key.

### Example

```python
rdd1 = sc.parallelize([
    ("A", 10),
    ("A", 20),
    ("B", 30)
])

rdd2 = sc.parallelize([
    ("A", 100),
    ("B", 200)
])

result = rdd1.cogroup(rdd2)

formatted = result.mapValues(
    lambda x: (list(x[0]), list(x[1]))
)

print(formatted.collect())
```

Possible output:

```
[
    ('A', ([10, 20], [100])),
    ('B', ([30], [200]))
]
```

***

## 30. subtractByKey()

### Definition

Removes records from the first Pair RDD whose keys exist in the second Pair RDD.

```python
rdd1 = sc.parallelize([
    ("A", 10),
    ("B", 20),
    ("C", 30)
])

rdd2 = sc.parallelize([
    ("A", 100),
    ("C", 300)
])

result = rdd1.subtractByKey(rdd2)

print(result.collect())
```

Output:

```
[('B', 20)]
```

Only key `B` remains.

***

## 31. partitionBy()

### Definition

Redistributes a Pair RDD according to a partitioner.

### Example

```python
from pyspark import HashPartitioner

rdd = sc.parallelize([
    ("A", 10),
    ("B", 20),
    ("C", 30)
])

result = rdd.partitionBy(
    HashPartitioner(2)
)

print(result.getNumPartitions())
```

Output:

```
2
```

### Why Is It Useful?

Partitioning determines where records are physically distributed across Spark partitions.

Good partitioning can reduce unnecessary shuffles in later operations.

***

## 32. sample()

### Definition

Returns a random sample of an RDD.

### Signature

```python
RDD.sample(
    withReplacement,
    fraction,
    seed=None
)
```

### Parameters

#### `withReplacement`

```python
True
```

allows the same element to be selected more than once.

```python
False
```

does not.

#### `fraction`

Sampling fraction.

For example:

```python
0.1
```

means approximately 10%, although it is probabilistic.

#### `seed`

Optional random seed for reproducibility.

### Example

```python
rdd = sc.parallelize(range(1, 101))

sample = rdd.sample(
    withReplacement=False,
    fraction=0.1,
    seed=42
)

print(sample.collect())
```

The exact result depends on sampling.

***

## 33. sampleByKey()

### Definition

Samples a Pair RDD using different sampling fractions for different keys.

### Example

```python
rdd = sc.parallelize([
    ("A", 1),
    ("A", 2),
    ("A", 3),
    ("B", 4),
    ("B", 5),
    ("B", 6)
])

fractions = {
    "A": 0.5,
    "B": 0.5
}

result = rdd.sampleByKey(
    withReplacement=False,
    fractions=fractions,
    seed=42
)

print(result.collect())
```

Different keys can have different fractions.

***

## 34. randomSplit()

### Definition

Splits an RDD into multiple RDDs according to weights.

### Signature

```python
RDD.randomSplit(weights, seed=None)
```

### Parameters

#### `weights`

List of weights.

Example:

```python
[0.8, 0.2]
```

#### `seed`

Optional random seed.

### Example

```python
rdd = sc.parallelize(range(1, 101))

train, test = rdd.randomSplit(
    [0.8, 0.2],
    seed=42
)

print(train.count())
print(test.count())
```

The counts are approximately 80/20.

The weights do not necessarily need to add to exactly `1.0`.

For example:

```python
[8, 2]
```

represents the same relative distribution.

***

## 35. coalesce()

### Definition

Reduces the number of partitions, usually without a full shuffle.

### Signature

```python
RDD.coalesce(
    numPartitions,
    shuffle=False
)
```

### Parameters

#### `numPartitions`

Target number of partitions.

#### `shuffle`

Whether to perform a shuffle.

Default:

```python
False
```

Example:

```python
rdd = sc.parallelize(range(100), 4)

result = rdd.coalesce(2)

print(result.getNumPartitions())
```

Output:

```
2
```

To explicitly shuffle:

```python
result = rdd.coalesce(
    2,
    shuffle=True
)
```

***

## 36. repartition()

### Definition

Changes the number of partitions using a shuffle.

### Signature

```python
RDD.repartition(numPartitions)
```

### Example

```python
rdd = sc.parallelize(range(100), 2)

result = rdd.repartition(5)

print(result.getNumPartitions())
```

Output:

```
5
```

### coalesce vs repartition

```
coalesce()
    ↓
Usually used to reduce partitions
    ↓
Avoids full shuffle by default

repartition()
    ↓
Can increase or decrease partitions
    ↓
Uses shuffle
```

***

## 37. zip()

### Definition

Combines elements from two RDDs position-by-position.

### Example

```python
rdd1 = sc.parallelize([1, 2, 3])
rdd2 = sc.parallelize(["A", "B", "C"])

result = rdd1.zip(rdd2)

print(result.collect())
```

Output:

```
[(1, 'A'), (2, 'B'), (3, 'C')]
```

The RDDs need compatible partitioning and element counts.

***

## 38. zipWithIndex()

### Definition

Adds an index to every element.

### Example

```python
rdd = sc.parallelize([
    "Python",
    "Java",
    "Spark"
])

result = rdd.zipWithIndex()

print(result.collect())
```

Output:

```
[
    ('Python', 0),
    ('Java', 1),
    ('Spark', 2)
]
```

***

## 39. zipWithUniqueId()

### Definition

Assigns unique IDs to RDD elements without requiring a full global sequential index.

### Example

```python
rdd = sc.parallelize([
    "A",
    "B",
    "C"
])

result = rdd.zipWithUniqueId()

print(result.collect())
```

The exact IDs depend on partitioning.

***

## 40. pipe()

### Definition

Sends RDD data through an external command.

### Example

```python
rdd = sc.parallelize([
    "hello",
    "world"
])

result = rdd.pipe("tr a-z A-Z")

print(result.collect())
```

Possible output:

```
['HELLO', 'WORLD']
```

This is an advanced integration technique.

***

## PART 2 --- RDD ACTIONS

## 41. What Is an RDD Action?

An action triggers computation of the RDD lineage and either:

* returns a result to the driver,
* performs an operation for every record,
* or writes the result to external storage.

Example:

```python
rdd = sc.parallelize([1, 2, 3])

result = rdd.map(lambda x: x * 2)

print(result.collect())
```

`map()` defines the computation.

`collect()` triggers it.

***

## 42. collect()

### Definition

Returns all RDD elements to the driver as a Python list.

### Signature

```python
RDD.collect()
```

### Parameters

None.

### Example

```python
rdd = sc.parallelize([1, 2, 3, 4])

result = rdd.collect()

print(result)
```

Output:

```
[1, 2, 3, 4]
```

### Important Warning

Never casually use:

```python
large_rdd.collect()
```

on huge datasets.

If the RDD contains millions of records, all those records are brought to the driver.

This can cause:

```
Driver memory exhaustion
```

Use:

```python
take()
```

or:

```python
takeSample()
```

when you only need a small portion.

***

## 43. count()

### Definition

Returns the total number of elements.

### Example

```python
rdd = sc.parallelize([
    10, 20, 30, 40
])

print(rdd.count())
```

Output:

```
4
```

***

## 44. first()

### Definition

Returns the first element of the RDD.

### Example

```python
rdd = sc.parallelize([
    "Python",
    "Java",
    "Spark"
])

print(rdd.first())
```

Output:

```
Python
```

It is useful for quick inspection.

***

## 45. take()

### Definition

Returns the first `n` elements.

### Signature

```python
RDD.take(num)
```

### Parameter

#### `num`

Number of elements to return.

### Example

```python
rdd = sc.parallelize(range(1, 101))

result = rdd.take(5)

print(result)
```

Output:

```
[1, 2, 3, 4, 5]
```

This is generally safer than collecting the entire RDD.

***

## 46. takeOrdered()

### Definition

Returns the smallest `n` elements.

### Signature

```python
RDD.takeOrdered(num, key=None)
```

### Parameters

#### `num`

Number of records.

#### `key`

Optional function used to determine ordering.

### Example

```python
rdd = sc.parallelize([
    50, 10, 40, 20, 30
])

print(rdd.takeOrdered(3))
```

Output:

```
[10, 20, 30]
```

### Descending

```python
print(
    rdd.takeOrdered(
        3,
        key=lambda x: -x
    )
)
```

Output:

```
[50, 40, 30]
```

***

## 47. top()

### Definition

Returns the largest `n` elements.

### Signature

```python
RDD.top(num, key=None)
```

### Example

```python
rdd = sc.parallelize([
    10, 50, 20, 40, 30
])

print(rdd.top(3))
```

Output:

```
[50, 40, 30]
```

***

## 48. takeSample()

### Definition

Returns a random sample to the driver.

### Signature

```python
RDD.takeSample(
    withReplacement,
    num,
    seed=None
)
```

### Parameters

#### `withReplacement`

Whether an element can be selected multiple times.

#### `num`

Number of elements requested.

#### `seed`

Optional seed for reproducibility.

### Example

```python
rdd = sc.parallelize(range(1, 101))

result = rdd.takeSample(
    False,
    5,
    seed=42
)

print(result)
```

The exact output depends on the random sample.

***

## 49. reduce()

### Definition

Combines all elements using a function.

### Signature

```python
RDD.reduce(f)
```

### Parameter

#### `f`

Function accepting two values and returning one value.

### Example

```python
rdd = sc.parallelize([
    1, 2, 3, 4
])

result = rdd.reduce(
    lambda a, b: a + b
)

print(result)
```

Output:

```
10
```

### Execution Concept

Spark may calculate:

```
Partition 1:
1 + 2 = 3

Partition 2:
3 + 4 = 7
```

Then:

```
3 + 7 = 10
```

Therefore the reduction function needs to work correctly in a distributed manner.

***

## 50. fold()

### Definition

Aggregates the elements using a zero value.

### Signature

```python
RDD.fold(zeroValue, op)
```

### Parameters

#### `zeroValue`

Initial value used for each partition.

#### `op`

Function combining two values.

### Example

```python
rdd = sc.parallelize([
    1, 2, 3, 4
])

result = rdd.fold(
    0,
    lambda a, b: a + b
)

print(result)
```

Output:

```
10
```

### Important

The operation should be associative and should work correctly with the zero value.

***

## 51. aggregate()

### Definition

`aggregate()` allows a result type different from the input type and provides separate functions for processing values and merging partition results.

### Signature

```python
RDD.aggregate(
    zeroValue,
    seqOp,
    combOp
)
```

### Parameters

#### `zeroValue`

Initial accumulator.

#### `seqOp`

Processes one element within a partition.

#### `combOp`

Combines results from different partitions.

### Example

Calculate:

```
(sum, count)
```

```python
rdd = sc.parallelize([
    10, 20, 30, 40
])

result = rdd.aggregate(
    (0, 0),

    lambda acc, value: (
        acc[0] + value,
        acc[1] + 1
    ),

    lambda acc1, acc2: (
        acc1[0] + acc2[0],
        acc1[1] + acc2[1]
    )
)

print(result)
```

Output:

```
(100, 4)
```

Average:

```python
average = result[0] / result[1]

print(average)
```

Output:

```
25.0
```

***

## 52. countByKey()

### Definition

Counts how many values exist for each key.

### Example

```python
rdd = sc.parallelize([
    ("A", 10),
    ("A", 20),
    ("B", 30),
    ("B", 40),
    ("B", 50)
])

result = rdd.countByKey()

print(dict(result))
```

Output:

```
{
    'A': 2,
    'B': 3
}
```

***

## 53. countByValue()

### Definition

Counts occurrences of each unique value.

### Example

```python
rdd = sc.parallelize([
    "Python",
    "Java",
    "Python",
    "Spark",
    "Python"
])

result = rdd.countByValue()

print(dict(result))
```

Output:

```
{
    'Python': 3,
    'Java': 1,
    'Spark': 1
}
```

***

## 54. collectAsMap()

### Definition

Collects a Pair RDD into a Python dictionary-like mapping.

### Example

```python
rdd = sc.parallelize([
    ("A", 100),
    ("B", 200),
    ("C", 300)
])

result = rdd.collectAsMap()

print(result)
```

Output:

```
{
    'A': 100,
    'B': 200,
    'C': 300
}
```

### Warning

The complete result is brought to the driver.

***

## 55. lookup()

### Definition

Returns all values associated with a specific key.

### Signature

```python
PairRDD.lookup(key)
```

### Parameter

#### `key`

The key to search for.

### Example

```python
rdd = sc.parallelize([
    ("A", 10),
    ("A", 20),
    ("B", 30)
])

result = rdd.lookup("A")

print(result)
```

Output:

```
[10, 20]
```

***

## 56. sum()

### Definition

Returns the sum of numeric elements.

### Example

```python
rdd = sc.parallelize([
    10, 20, 30, 40
])

print(rdd.sum())
```

Output:

```
100
```

***

## 57. max()

### Definition

Returns the maximum element.

### Example

```python
rdd = sc.parallelize([
    10, 50, 20, 40
])

print(rdd.max())
```

Output:

```
50
```

***

## 58. min()

### Definition

Returns the minimum element.

### Example

```python
rdd = sc.parallelize([
    10, 50, 20, 40
])

print(rdd.min())
```

Output:

```
10
```

***

## 59. mean()

### Definition

Returns the arithmetic mean.

### Example

```python
rdd = sc.parallelize([
    10, 20, 30, 40
])

print(rdd.mean())
```

Output:

```
25.0
```

Conceptually:

```
(10 + 20 + 30 + 40) / 4

= 100 / 4

= 25
```

***

## 60. variance()

### Definition

Returns the population variance.

### Example

```python
rdd = sc.parallelize([
    10, 20, 30, 40
])

print(rdd.variance())
```

Variance measures how far values are distributed around their mean.

***

## 61. stdev()

### Definition

Returns the standard deviation.

### Example

```python
rdd = sc.parallelize([
    10, 20, 30, 40
])

print(rdd.stdev())
```

Standard deviation is the square root of variance.

***

## 62. stats()

### Definition

Returns a statistics object containing several summary values.

### Example

```python
rdd = sc.parallelize([
    10, 20, 30, 40
])

result = rdd.stats()

print("Count:", result.count())
print("Mean:", result.mean())
print("Sum:", result.sum())
print("Min:", result.min())
print("Max:", result.max())
print("Variance:", result.variance())
print("Standard Deviation:", result.stdev())
```

This is useful when several statistics are needed from the same numeric RDD.

***

## 63. foreach()

### Definition

Applies a function to every element.

### Signature

```python
RDD.foreach(f)
```

### Parameter

#### `f`

Function that accepts one element.

### Example

```python
rdd = sc.parallelize([
    10, 20, 30
])

rdd.foreach(
    lambda x: print(x)
)
```

### Important Difference from collect()

`collect()` sends data to the driver.

`foreach()` executes the function on worker-side processing.

Therefore, using `foreach(print)` should not be treated as a reliable way to display a large dataset in the driver's console.

For debugging, prefer:

```python
rdd.take(10)
```

or:

```python
print(rdd.collect())
```

for small datasets.

***

## 64. foreachPartition()

### Definition

Applies a function once per partition.

### Signature

```python
RDD.foreachPartition(f)
```

### Parameter

#### `f`

Function receiving a partition iterator.

### Example

```python
def process_partition(iterator):

    for value in iterator:
        print(value)

rdd = sc.parallelize([
    10, 20, 30, 40
], 2)

rdd.foreachPartition(process_partition)
```

### Practical Database Pattern

```python
def save_partition(iterator):

    connection = create_connection()

    for row in iterator:
        save_row(connection, row)

    connection.close()

rdd.foreachPartition(save_partition)
```

This is generally better than creating a database connection for every individual record.

***

## 65. countApprox()

### Definition

Returns an approximate count within a specified timeout.

### Example

```python
rdd = sc.parallelize(range(1, 1000000))

result = rdd.countApprox(
    timeout=1000,
    confidence=0.95
)

print(result)
```

#### Parameters

`timeout`:

Maximum time allowed for the approximate computation, in milliseconds.

`confidence`:

Confidence level for the estimate.

Example:

```
0.95 = 95%
```

This is useful when an approximate answer is acceptable.

***

## 66. countApproxDistinct()

### Definition

Estimates the number of distinct elements.

### Example

```python
rdd = sc.parallelize([
    1, 2, 3, 3, 4, 4, 5
])

result = rdd.countApproxDistinct()

print(result)
```

It is an estimate rather than an exact distinct count.

***

## 67. saveAsTextFile()

### Definition

Writes RDD elements as text files.

### Signature

```python
RDD.saveAsTextFile(path, compressionCodecClass=None)
```

### Parameters

#### `path`

Output location.

#### `compressionCodecClass`

Optional compression codec.

### Example

```python
rdd = sc.parallelize([
    "Python",
    "Spark",
    "RDD"
])

rdd.saveAsTextFile(
    "output/rdd_text"
)
```

Spark creates an output directory containing partition files.

Conceptually:

```
output/rdd_text/
    part-00000
    part-00001
    ...
    _SUCCESS
```

The number of `part-*` files generally corresponds to the number of output partitions.

***

## 68. saveAsSequenceFile()

### Definition

Saves a Pair RDD as a Hadoop SequenceFile.

### Example

```python
rdd = sc.parallelize([
    ("A", 100),
    ("B", 200)
])

rdd.saveAsSequenceFile(
    "output/sequence"
)
```

It is primarily useful in Hadoop-compatible workflows.

***

## 69. saveAsPickleFile()

### Definition

Saves an RDD using Python pickle serialization.

### Example

```python
rdd = sc.parallelize([
    ("A", 100),
    ("B", 200)
])

rdd.saveAsPickleFile(
    "output/pickle"
)
```

This is Python-specific and should not be confused with a general interoperable data format such as Parquet.

***

## 70. Transformation vs Action --- Final Mental Model

Consider:

```python
rdd = sc.parallelize([
    "Python Spark",
    "Spark RDD"
])

words = rdd.flatMap(
    lambda x: x.split()
)

pairs = words.map(
    lambda word: (word, 1)
)

counts = pairs.reduceByKey(
    lambda a, b: a + b
)

result = counts.collect()
```

Execution flow:

```
                 TRANSFORMATIONS

Original RDD
    |
    | flatMap()
    v
Words RDD
    |
    | map()
    v
Pair RDD
    |
    | reduceByKey()
    v
Aggregated RDD
    |
    | collect()
    v

                  ACTION

              Driver receives
                  result
```

The first three operations are transformations.

```
flatMap()
map()
reduceByKey()
```

They build the computation.

The final operation:

```
collect()
```

is the action that triggers execution.

***

## 71. Transformation Categories

### Element Transformations

```
map()
flatMap()
filter()
mapPartitions()
mapPartitionsWithIndex()
glom()
```

### Pair RDD Transformations

```
mapValues()
flatMapValues()
keys()
values()
keyBy()
```

### Aggregation Transformations

```
reduceByKey()
groupByKey()
aggregateByKey()
foldByKey()
combineByKey()
```

### Join Transformations

```
join()
leftOuterJoin()
rightOuterJoin()
fullOuterJoin()
cogroup()
groupWith()
subtractByKey()
```

### Set Transformations

```
distinct()
union()
intersection()
subtract()
cartesian()
```

### Sorting / Sampling

```
sortBy()
sortByKey()
sample()
sampleByKey()
randomSplit()
```

### Partition Transformations

```
partitionBy()
coalesce()
repartition()
```

### Utility Transformations

```
zip()
zipWithIndex()
zipWithUniqueId()
pipe()
```

***

## 72. Action Categories

### Retrieval

```
collect()
first()
take()
takeOrdered()
top()
takeSample()
```

### Counting

```
count()
countApprox()
countApproxDistinct()
countByKey()
countByValue()
```

### Aggregation

```
reduce()
fold()
aggregate()
```

### Pair RDD Retrieval

```
collectAsMap()
lookup()
```

### Statistics

```
sum()
min()
max()
mean()
variance()
stdev()
stats()
```

### Processing

```
foreach()
foreachPartition()
```

### Output

```
saveAsTextFile()
saveAsSequenceFile()
saveAsPickleFile()
```

***

## 73. The Three Questions to Ask for Every RDD Method

When learning any RDD API method, always ask:

### Question 1 --- What does it receive?

Example:

```python
map()
```

receives one element.

```python
mapPartitions()
```

receives an iterator for an entire partition.

```python
reduceByKey()
```

works with key-value records and combines values belonging to the same key.

***

### Question 2 --- What does it return?

Example:

```python
map()
```

returns another RDD.

```python
collect()
```

returns a Python list.

```python
count()
```

returns an integer.

```python
reduce()
```

returns a single value.

***

### Question 3 --- Does it require a shuffle?

This is particularly important for Spark performance.

Examples that can require shuffle:

```
reduceByKey()
groupByKey()
sortByKey()
distinct()
join()
intersection()
repartition()
```

Operations such as:

```
map()
filter()
flatMap()
mapValues()
```

normally do not require a shuffle.

***

## 74. Core Rule to Remember

```
TRANSFORMATION

Input RDD
    |
    v
New RDD
```

Examples:

```
map()
filter()
flatMap()
reduceByKey()
join()
sortBy()
```

They build the computation.

***

```
ACTION

RDD
 |
 v
Execution
 |
 +----> Driver result
 |
 +----> External storage
 |
 +----> Side effect
```

Examples:

```
collect()
count()
reduce()
foreach()
saveAsTextFile()
```

They trigger execution.

***

## 75. Final Summary

The most important RDD transformations to master deeply are:

```
map()
filter()
flatMap()

mapPartitions()
mapPartitionsWithIndex()

mapValues()
flatMapValues()

reduceByKey()
groupByKey()
aggregateByKey()
foldByKey()
combineByKey()

sortBy()
sortByKey()

distinct()
union()
intersection()
subtract()

join()
leftOuterJoin()
rightOuterJoin()
fullOuterJoin()
cogroup()
subtractByKey()

sample()
sampleByKey()
randomSplit()

coalesce()
repartition()
partitionBy()

zip()
zipWithIndex()
zipWithUniqueId()
```

The most important RDD actions to master deeply are:

```
collect()
count()
first()
take()
takeOrdered()
top()
takeSample()

reduce()
fold()
aggregate()

countByKey()
countByValue()
collectAsMap()
lookup()

sum()
min()
max()
mean()
variance()
stdev()
stats()

foreach()
foreachPartition()

countApprox()
countApproxDistinct()

saveAsTextFile()
saveAsSequenceFile()
saveAsPickleFile()
```

The central concept is:

```
                RDD PROGRAM

     +-----------------------------+
     |      TRANSFORMATIONS        |
     |                             |
     | map                         |
     | filter                      |
     | flatMap                     |
     | reduceByKey                 |
     | groupByKey                  |
     | join                        |
     | sortBy                      |
     +-------------+---------------+
                   |
                   | Lazy
                   v
              RDD LINEAGE
                   |
                   |
                   v
     +-------------+---------------+
     |          ACTION             |
     |                             |
     | collect                     |
     | count                       |
     | reduce                      |
     | foreach                     |
     | saveAsTextFile              |
     +-------------+---------------+
                   |
                   v
             SPARK EXECUTION
```

**Transformations build the computation.**

**Actions trigger the computation.**

**Shuffle-heavy transformations are usually the most important ones to understand for Spark performance.**
