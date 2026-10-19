# SQL Questions 2

> Source notebook: [DE SQL Notes/SQL\_Questions\_2.ipynb](https://github.com/karthikeyanm-dev/DE-Notes/blob/main/DE%20SQL%20Notes/SQL_Questions_2.ipynb). Notebook outputs are omitted; open the source to run cells.

```sql
%load_ext sql
%sql mysql+mysqlconnector://root:123456@localhost
```

```sql
%%sql
use swiggy_db
```

## Window Functions Practice — Question 1

### Question 1: Rank Customers by Total Spending

Using the existing `customers` and `orders` tables, calculate the **total amount spent by each customer**.

Then use a **Window Function** to assign a ranking based on total spending.

#### Requirements

* Consider only orders where `order_status = 'Delivered'`.
* Calculate the total spending for each customer.
* Display:
  * `customer_id`
  * `customer_name`
  * `total_spent`
  * `spending_rank`
* The customer with the highest total spending should receive rank `1`.
* Use a **Window Function** for the ranking.
* Do not use a subquery or CTE for this question.

#### Expected Output

| customer\_id | customer\_name | total\_spent | spending\_rank |
| ------------ | -------------- | ------------ | -------------- |
| 105          | Arun Kumar     | 18500.00     | 1              |
| 102          | Priya Sharma   | 16200.00     | 2              |
| 109          | Rahul Kumar    | 14800.00     | 3              |
| 101          | Karthik Raj    | 12500.00     | 4              |
| ...          | ...            | ...          | ...            |

#### Hint

You will need:

* `SUM()`
* `GROUP BY`
* `RANK()` or `DENSE_RANK()`
* `OVER()`

Write the SQL query and provide your answer.
