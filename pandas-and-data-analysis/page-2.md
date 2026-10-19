# Page

## Data Generate

> Imported from [`python/Pandas/Prj/Data Generate.ipynb`](https://github.com/karthikeyanm-dev/DE-Notes/blob/main/python/Pandas/Prj/Data%20Generate.ipynb). Code outputs are omitted; run the source notebook for live results.

## Realistic Customer and Sales Data Generation Using Python

For our Pandas + SQLAlchemy + ETL practice, instead of using only 10–20 manually created records, we can generate a much larger dataset that behaves more like a real business system.

We will generate:

```
customers.csv
sales.csv
products.csv
```

The generated data will contain:

```
Hundreds / thousands of customers
Repeated customers making multiple orders
Different cities and states
Different products
Realistic order dates
Different payment methods
Different order statuses
Different quantities
Discounts
Tax
Order amounts
Customer registration dates
```

We will also make the data suitable for:

```
Pandas
SQL
SQLAlchemy
ETL
PySpark
Data Cleaning
Data Analysis
Joins
GroupBy
Window Functions
```

***

## 1. Install Required Libraries

```python
pip install pandas faker numpy openpyxl
```

In Jupyter:

```python
!pip install pandas faker numpy openpyxl
```

***

## 2. Import Libraries

```python
import pandas as pd
import numpy as np

from faker import Faker

from datetime import datetime, timedelta
```

Create Faker object:

```python
fake = Faker("en_IN")
```

The `"en_IN"` locale helps generate more India-style names, addresses, phone numbers, etc.

***

## 3. Set Random Seed

For learning and debugging, it is useful to make the generated data reproducible.

```python
np.random.seed(42)
Faker.seed(42)
```

Now, when you run the code again, you will get the same generated data.

Later, remove or change the seed when you want a completely new dataset.

***

## 4. Business Scenario

Imagine we are generating data for an e-commerce company.

Our system contains:

```
CUSTOMERS
   |
   | customer_id
   ↓
SALES
   |
   | product_id
   ↓
PRODUCTS
```

Relationship:

```
One customer
      ↓
Can place many orders
```

For example:

```
Customer 1001

Order 50001
Order 50025
Order 50078
Order 50120
```

This makes the data more realistic than simply generating one customer for every sale.

***

## 5. Generate Product Master Data

We will first create a small product catalog.

```python
products = [
    {
        "product_id": 501,
        "product_name": "Laptop",
        "category": "Electronics",
        "price": 55000
    },
    {
        "product_id": 502,
        "product_name": "Wireless Mouse",
        "category": "Accessories",
        "price": 1200
    },
    {
        "product_id": 503,
        "product_name": "Keyboard",
        "category": "Accessories",
        "price": 1800
    },
    {
        "product_id": 504,
        "product_name": "Monitor",
        "category": "Electronics",
        "price": 18000
    },
    {
        "product_id": 505,
        "product_name": "Smartphone",
        "category": "Electronics",
        "price": 28000
    },
    {
        "product_id": 506,
        "product_name": "Headphones",
        "category": "Accessories",
        "price": 3500
    },
    {
        "product_id": 507,
        "product_name": "Power Bank",
        "category": "Accessories",
        "price": 1800
    },
    {
        "product_id": 508,
        "product_name": "Tablet",
        "category": "Electronics",
        "price": 22000
    },
    {
        "product_id": 509,
        "product_name": "Smart Watch",
        "category": "Wearables",
        "price": 7500
    },
    {
        "product_id": 510,
        "product_name": "Bluetooth Speaker",
        "category": "Audio",
        "price": 4500
    }
]

products_df = pd.DataFrame(products)

print(products_df)
```

***

## 6. Generate Customers

We will generate:

```
1000 customers
```

You can change this:

```python
NUM_CUSTOMERS = 1000
```

Full customer generation:

```python
NUM_CUSTOMERS = 1000

cities = {
    "Chennai": "Tamil Nadu",
    "Coimbatore": "Tamil Nadu",
    "Madurai": "Tamil Nadu",
    "Bangalore": "Karnataka",
    "Mysore": "Karnataka",
    "Hyderabad": "Telangana",
    "Mumbai": "Maharashtra",
    "Pune": "Maharashtra",
    "Delhi": "Delhi",
    "Kochi": "Kerala"
}

customer_data = []

for customer_id in range(1001, 1001 + NUM_CUSTOMERS):

    city = np.random.choice(
        list(cities.keys())
    )

    state = cities[city]

    customer_data.append({
        "customer_id": customer_id,

        "customer_name": fake.name(),

        "email": fake.email(),

        "phone": fake.msisdn()[:10],

        "city": city,

        "state": state,

        "pincode": fake.postcode(),

        "age": np.random.randint(18, 65),

        "gender": np.random.choice(
            ["Male", "Female"],
            p=[0.52, 0.48]
        ),

        "customer_since": fake.date_between(
            start_date="-5y",
            end_date="today"
        ),

        "customer_type": np.random.choice(
            ["Regular", "Premium", "Corporate"],
            p=[0.70, 0.25, 0.05]
        )
    })

customers_df = pd.DataFrame(customer_data)

print(customers_df.head())
```

***

## 7. Inspect Customers

```python
print(customers_df.shape)
```

Expected:

```
(1000, 10)
```

Check:

```python
customers_df.info()
```

And:

```python
print(
    customers_df["city"].value_counts()
)
```

***

## 8. Generate Sales Transactions

Now we generate realistic transactions.

Suppose:

```
1,000 customers
```

make around:

```
20,000 orders
```

```python
NUM_SALES = 20000
```

Important:

We should NOT generate a new customer for every sale.

Instead, we randomly select from our existing customer IDs.

That gives us:

```
1000 customers
      ↓
20000 sales
```

This is much closer to real transactional data.

***

## 9. Generate Sales

```python
NUM_SALES = 20000

product_lookup = products_df.set_index(
    "product_id"
).to_dict("index")

customer_ids = customers_df[
    "customer_id"
].tolist()

product_ids = products_df[
    "product_id"
].tolist()

sales_data = []

for order_number in range(NUM_SALES):

    order_id = 100000 + order_number

    customer_id = np.random.choice(
        customer_ids
    )

    product_id = np.random.choice(
        product_ids
    )

    product = product_lookup[
        product_id
    ]

    quantity = np.random.choice(
        [1, 2, 3, 4, 5],
        p=[0.50, 0.25, 0.15, 0.07, 0.03]
    )

    unit_price = product["price"]

    gross_amount = (
        quantity * unit_price
    )

    discount_percent = np.random.choice(
        [0, 5, 10, 15, 20],
        p=[0.40, 0.25, 0.20, 0.10, 0.05]
    )

    discount_amount = (
        gross_amount *
        discount_percent / 100
    )

    taxable_amount = (
        gross_amount -
        discount_amount
    )

    tax_percent = 18

    tax_amount = (
        taxable_amount *
        tax_percent / 100
    )

    final_amount = (
        taxable_amount +
        tax_amount
    )

    order_status = np.random.choice(
        [
            "Delivered",
            "Shipped",
            "Processing",
            "Cancelled",
            "Returned"
        ],
        p=[0.68, 0.12, 0.08, 0.07, 0.05]
    )

    payment_method = np.random.choice(
        [
            "UPI",
            "Credit Card",
            "Debit Card",
            "Net Banking",
            "Cash on Delivery"
        ],
        p=[0.40, 0.20, 0.15, 0.10, 0.15]
    )

    order_date = fake.date_between(
        start_date="-1y",
        end_date="today"
    )

    sales_data.append({
        "order_id": order_id,

        "customer_id": customer_id,

        "product_id": product_id,

        "order_date": order_date,

        "quantity": quantity,

        "unit_price": unit_price,

        "gross_amount": round(
            gross_amount,
            2
        ),

        "discount_percent": discount_percent,

        "discount_amount": round(
            discount_amount,
            2
        ),

        "tax_percent": tax_percent,

        "tax_amount": round(
            tax_amount,
            2
        ),

        "final_amount": round(
            final_amount,
            2
        ),

        "payment_method": payment_method,

        "order_status": order_status
    })
```

Convert to DataFrame:

```python
sales_df = pd.DataFrame(
    sales_data
)
```

***

## 10. Inspect Sales

```python
print(sales_df.head())
```

Shape:

```python
print(sales_df.shape)
```

Expected:

```
(20000, 14)
```

***

## 11. Inspect Sales Statistics

```python
print(
    sales_df["final_amount"].describe()
)
```

Check order statuses:

```python
print(
    sales_df["order_status"].value_counts()
)
```

Payment methods:

```python
print(
    sales_df["payment_method"].value_counts()
)
```

***

## 12. Check Sales by Product

```python
print(
    sales_df["product_id"].value_counts()
)
```

***

## 13. Check Sales by Customer

```python
customer_sales_count = (
    sales_df[
        "customer_id"
    ]
    .value_counts()
)

print(
    customer_sales_count.head(10)
)
```

This demonstrates an important real-world property:

```
One customer
     ↓
Many orders
```

***

## 14. Merge Sales with Product Details

For an analytics dataset, we can combine sales with product information.

```python
sales_enriched = sales_df.merge(
    products_df,
    on="product_id",
    how="left"
)
```

Now the sales data includes:

```
product_id
product_name
category
price
```

***

## 15. Merge with Customer Data

We can then create a broader analytical dataset:

```python
sales_enriched = sales_enriched.merge(
    customers_df[
        [
            "customer_id",
            "customer_name",
            "city",
            "state",
            "gender",
            "customer_type"
        ]
    ],
    on="customer_id",
    how="left"
)
```

Now:

```
Sales
  +
Products
  +
Customers
```

are available in one DataFrame.

***

## 16. Final Dataset Structure

The enriched dataset will contain fields such as:

```
order_id
customer_id
product_id
order_date
quantity
unit_price
gross_amount
discount_percent
discount_amount
tax_percent
tax_amount
final_amount
payment_method
order_status
product_name
category
price
customer_name
city
state
gender
customer_type
```

This is much closer to a real analytical dataset.

***

## 17. Save the Data

Customers:

```python
customers_df.to_csv(
    "customers.csv",
    index=False
)
```

Products:

```python
products_df.to_csv(
    "products.csv",
    index=False
)
```

Sales:

```python
sales_df.to_csv(
    "sales.csv",
    index=False
)
```

Enriched sales:

```python
sales_enriched.to_csv(
    "sales_enriched.csv",
    index=False
)
```

***

## 18. Save Excel Versions

Customers:

```python
customers_df.to_excel(
    "customers.xlsx",
    index=False
)
```

Sales:

```python
sales_df.to_excel(
    "sales.xlsx",
    index=False
)
```

***

## 19. Complete Basic Generator

Here is the complete version in one script.

```python
import pandas as pd
import numpy as np

from faker import Faker


# ============================================================
# CONFIGURATION
# ============================================================

NUM_CUSTOMERS = 1000
NUM_SALES = 20000

fake = Faker("en_IN")

np.random.seed(42)
Faker.seed(42)


# ============================================================
# PRODUCT MASTER
# ============================================================

products = [
    {
        "product_id": 501,
        "product_name": "Laptop",
        "category": "Electronics",
        "price": 55000
    },
    {
        "product_id": 502,
        "product_name": "Wireless Mouse",
        "category": "Accessories",
        "price": 1200
    },
    {
        "product_id": 503,
        "product_name": "Keyboard",
        "category": "Accessories",
        "price": 1800
    },
    {
        "product_id": 504,
        "product_name": "Monitor",
        "category": "Electronics",
        "price": 18000
    },
    {
        "product_id": 505,
        "product_name": "Smartphone",
        "category": "Electronics",
        "price": 28000
    },
    {
        "product_id": 506,
        "product_name": "Headphones",
        "category": "Accessories",
        "price": 3500
    },
    {
        "product_id": 507,
        "product_name": "Power Bank",
        "category": "Accessories",
        "price": 1800
    },
    {
        "product_id": 508,
        "product_name": "Tablet",
        "category": "Electronics",
        "price": 22000
    },
    {
        "product_id": 509,
        "product_name": "Smart Watch",
        "category": "Wearables",
        "price": 7500
    },
    {
        "product_id": 510,
        "product_name": "Bluetooth Speaker",
        "category": "Audio",
        "price": 4500
    }
]

products_df = pd.DataFrame(products)


# ============================================================
# CUSTOMER MASTER
# ============================================================

cities = {
    "Chennai": "Tamil Nadu",
    "Coimbatore": "Tamil Nadu",
    "Madurai": "Tamil Nadu",
    "Bangalore": "Karnataka",
    "Mysore": "Karnataka",
    "Hyderabad": "Telangana",
    "Mumbai": "Maharashtra",
    "Pune": "Maharashtra",
    "Delhi": "Delhi",
    "Kochi": "Kerala"
}

customer_data = []

for customer_id in range(
    1001,
    1001 + NUM_CUSTOMERS
):

    city = np.random.choice(
        list(cities.keys())
    )

    state = cities[city]

    customer_data.append({

        "customer_id": customer_id,

        "customer_name": fake.name(),

        "email": fake.email(),

        "phone": fake.msisdn()[:10],

        "city": city,

        "state": state,

        "pincode": fake.postcode(),

        "age": np.random.randint(
            18,
            65
        ),

        "gender": np.random.choice(
            ["Male", "Female"],
            p=[0.52, 0.48]
        ),

        "customer_since": fake.date_between(
            start_date="-5y",
            end_date="today"
        ),

        "customer_type": np.random.choice(
            [
                "Regular",
                "Premium",
                "Corporate"
            ],
            p=[
                0.70,
                0.25,
                0.05
            ]
        )
    })


customers_df = pd.DataFrame(
    customer_data
)


# ============================================================
# SALES TRANSACTIONS
# ============================================================

product_lookup = (
    products_df
    .set_index("product_id")
    .to_dict("index")
)

customer_ids = (
    customers_df[
        "customer_id"
    ]
    .tolist()
)

product_ids = (
    products_df[
        "product_id"
    ]
    .tolist()
)

sales_data = []

for order_number in range(
    NUM_SALES
):

    order_id = (
        100000 +
        order_number
    )

    customer_id = np.random.choice(
        customer_ids
    )

    product_id = np.random.choice(
        product_ids
    )

    product = product_lookup[
        product_id
    ]

    quantity = np.random.choice(
        [1, 2, 3, 4, 5],
        p=[
            0.50,
            0.25,
            0.15,
            0.07,
            0.03
        ]
    )

    unit_price = product[
        "price"
    ]

    gross_amount = (
        quantity *
        unit_price
    )

    discount_percent = np.random.choice(
        [0, 5, 10, 15, 20],
        p=[
            0.40,
            0.25,
            0.20,
            0.10,
            0.05
        ]
    )

    discount_amount = (
        gross_amount *
        discount_percent /
        100
    )

    taxable_amount = (
        gross_amount -
        discount_amount
    )

    tax_percent = 18

    tax_amount = (
        taxable_amount *
        tax_percent /
        100
    )

    final_amount = (
        taxable_amount +
        tax_amount
    )

    order_status = np.random.choice(
        [
            "Delivered",
            "Shipped",
            "Processing",
            "Cancelled",
            "Returned"
        ],
        p=[
            0.68,
            0.12,
            0.08,
            0.07,
            0.05
        ]
    )

    payment_method = np.random.choice(
        [
            "UPI",
            "Credit Card",
            "Debit Card",
            "Net Banking",
            "Cash on Delivery"
        ],
        p=[
            0.40,
            0.20,
            0.15,
            0.10,
            0.15
        ]
    )

    order_date = fake.date_between(
        start_date="-1y",
        end_date="today"
    )

    sales_data.append({

        "order_id": order_id,

        "customer_id": customer_id,

        "product_id": product_id,

        "order_date": order_date,

        "quantity": quantity,

        "unit_price": unit_price,

        "gross_amount": round(
            gross_amount,
            2
        ),

        "discount_percent":
            discount_percent,

        "discount_amount": round(
            discount_amount,
            2
        ),

        "tax_percent":
            tax_percent,

        "tax_amount": round(
            tax_amount,
            2
        ),

        "final_amount": round(
            final_amount,
            2
        ),

        "payment_method":
            payment_method,

        "order_status":
            order_status
    })


sales_df = pd.DataFrame(
    sales_data
)


# ============================================================
# ENRICH SALES DATA
# ============================================================

sales_enriched = (
    sales_df
    .merge(
        products_df,
        on="product_id",
        how="left"
    )
    .merge(
        customers_df[
            [
                "customer_id",
                "customer_name",
                "city",
                "state",
                "gender",
                "customer_type"
            ]
        ],
        on="customer_id",
        how="left"
    )
)


# ============================================================
# SAVE CSV
# ============================================================

customers_df.to_csv(
    "customers.csv",
    index=False
)

products_df.to_csv(
    "products.csv",
    index=False
)

sales_df.to_csv(
    "sales.csv",
    index=False
)

sales_enriched.to_csv(
    "sales_enriched.csv",
    index=False
)


# ============================================================
# SAVE EXCEL
# ============================================================

customers_df.to_excel(
    "customers.xlsx",
    index=False
)

sales_df.to_excel(
    "sales.xlsx",
    index=False
)


# ============================================================
# SUMMARY
# ============================================================

print(
    "Customers:",
    customers_df.shape
)

print(
    "Products:",
    products_df.shape
)

print(
    "Sales:",
    sales_df.shape
)

print(
    "Enriched Sales:",
    sales_enriched.shape
)

print(
    "\nData generation completed successfully."
)
```

***

## 20. Generated Files

After running the script:

```
project/
│
├── customers.csv
├── customers.xlsx
│
├── products.csv
│
├── sales.csv
├── sales.xlsx
│
└── sales_enriched.csv
```

***

## 21. Why This Data Is More Realistic

This is not just random independent data.

We created relationships.

```
customers
    │
    │ customer_id
    ↓
sales
    │
    │ product_id
    ↓
products
```

For example:

```
Customer 1045
   ↓
Order 100123
   ↓
Product 507
```

And that customer might appear again:

```
Customer 1045
   ↓
Order 100812
   ↓
Product 502
```

This allows us to perform meaningful:

```
JOIN
GROUP BY
Aggregation
Customer analysis
Product analysis
Sales analysis
```

***

## 22. Example Analysis

### Total Sales

```python
total_sales = sales_df[
    "final_amount"
].sum()

print(total_sales)
```

***

### Average Order Value

```python
average_order = sales_df[
    "final_amount"
].mean()

print(average_order)
```

***

### Sales by Status

```python
print(
    sales_df.groupby(
        "order_status"
    )["final_amount"]
    .sum()
)
```

***

### Sales by Payment Method

```python
print(
    sales_df.groupby(
        "payment_method"
    )["final_amount"]
    .sum()
)
```

***

### Sales by Product

```python
product_sales = (
    sales_enriched
    .groupby(
        "product_name"
    )["final_amount"]
    .sum()
    .sort_values(
        ascending=False
    )
)

print(product_sales)
```

***

### Sales by City

```python
city_sales = (
    sales_enriched
    .groupby(
        "city"
    )["final_amount"]
    .sum()
    .sort_values(
        ascending=False
    )
)

print(city_sales)
```

***

### Top Customers

```python
top_customers = (
    sales_enriched
    .groupby(
        [
            "customer_id",
            "customer_name"
        ]
    )["final_amount"]
    .sum()
    .sort_values(
        ascending=False
    )
    .head(10)
)

print(top_customers)
```

***

## 23. Make It Look More Like a Real Raw Source

For your **ETL cleaning project**, we can take the clean generated data and intentionally introduce data-quality problems.

For example:

```
Original:

Chennai
Bangalore
Mumbai
```

Raw source:

```
chennai
 CHENNAI
Chennai 
Chennai
```

Email:

```
karthi@gmail.com
KARTHI@GMAIL.COM
invalid-email
```

Amounts:

```
500
700
INVALID
"1,200"
-500
```

Dates:

```
2026-01-10
10/01/2026
INVALID
NULL
```

This is much better for practicing a real ETL pipeline.

***

## 24. Add Realistic Data-Quality Issues

We can create a separate function:

```python
def make_dirty_data(df):

    df = df.copy()

    # --------------------------------
    # Add whitespace
    # --------------------------------

    sample_indexes = df.sample(
        frac=0.05,
        random_state=42
    ).index

    df.loc[
        sample_indexes,
        "customer_name"
    ] = (
        "  " +
        df.loc[
            sample_indexes,
            "customer_name"
        ].astype(str) +
        "  "
    )


    # --------------------------------
    # Change city capitalization
    # --------------------------------

    sample_indexes = df.sample(
        frac=0.05,
        random_state=10
    ).index

    df.loc[
        sample_indexes,
        "city"
    ] = (
        df.loc[
            sample_indexes,
            "city"
        ]
        .astype(str)
        .str.upper()
    )


    # --------------------------------
    # Add missing values
    # --------------------------------

    missing_indexes = df.sample(
        frac=0.02,
        random_state=20
    ).index

    df.loc[
        missing_indexes,
        "customer_name"
    ] = None


    # --------------------------------
    # Add invalid amounts
    # --------------------------------

    invalid_indexes = df.sample(
        frac=0.01,
        random_state=30
    ).index

    df.loc[
        invalid_indexes,
        "final_amount"
    ] = "INVALID"


    return df
```

Use:

```python
dirty_sales = make_dirty_data(
    sales_enriched
)
```

Save:

```python
dirty_sales.to_csv(
    "raw_sales_dirty.csv",
    index=False
)
```

Now your ETL practice becomes:

```
Generated realistic data
          ↓
Inject data-quality issues
          ↓
raw_sales_dirty.csv
          ↓
Pandas
          ↓
Clean
          ↓
Validate
          ↓
Transform
          ↓
clean_sales.csv
```

***

## 25. Recommended Dataset Sizes

For learning, don't immediately generate millions of rows.

Start with:

```
customers → 1,000
products  → 10
sales     → 20,000
```

Then increase:

```
10,000 customers
100 products
500,000 sales
```

Then:

```
100,000 customers
1,000 products
5,000,000 sales
```

The last level starts becoming useful for learning why tools such as:

```
PySpark
Databricks
Distributed Processing
```

become important.

***

## 26. Very Important Data Engineering Relationship

This generated dataset gives you an excellent progression:

```
Generated CSV
      ↓
Pandas
      ↓
Cleaning
      ↓
Excel Output
      ↓
SQLAlchemy
      ↓
SQLite / MySQL
      ↓
SQL Queries
      ↓
Database ETL
      ↓
PySpark
      ↓
Large Data
```

The exact same business data can be reused across all those topics.

***

## 27. Suggested Folder Structure

```
data_engineering_project/
│
├── data/
│   │
│   ├── raw/
│   │   ├── customers.csv
│   │   ├── products.csv
│   │   ├── sales.csv
│   │   └── raw_sales_dirty.csv
│   │
│   ├── processed/
│   │   ├── clean_customers.csv
│   │   ├── clean_sales.csv
│   │   └── sales_enriched.csv
│   │
│   └── output/
│       ├── sales_report.xlsx
│       └── sales_report.csv
│
├── notebooks/
│   └── pandas_etl.ipynb
│
├── scripts/
│   └── generate_data.py
│
└── database/
    └── etl_demo.db
```

This structure will also make it easier when you move from:

```
Pandas
```

to:

```
SQLAlchemy
```

and eventually:

```
PySpark
```

The important thing is that the **customer and sales relationship stays the same**, so you can use the same data throughout your Data Engineering course.
