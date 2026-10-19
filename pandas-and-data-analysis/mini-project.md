# Page

## Mini Project

> Imported from [`python/Pandas/Mini Project.ipynb`](https://github.com/karthikeyanm-dev/DE-Notes/blob/main/python/Pandas/Mini%20Project.ipynb). Code outputs are omitted; run the source notebook for live results.

## Mini Project — E-Commerce Data Cleaning & Final Excel Report

We already created three related datasets:

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

Now we will turn them into a small **real-world Pandas ETL project**.

The goal is:

```
Raw CSV Files
      ↓
Extract
      ↓
Data Profiling
      ↓
Data Cleaning
      ↓
Data Validation
      ↓
Join Customers + Products + Sales
      ↓
Create Business Metrics
      ↓
Generate Summary Reports
      ↓
Single Excel File
```

Final output:

```
ecommerce_final_report.xlsx
```

The Excel workbook will contain multiple sheets:

```
ecommerce_final_report.xlsx
│
├── Customers
├── Products
├── Clean_Sales
├── Sales_Detail
├── Customer_Summary
├── Product_Summary
├── City_Summary
├── Monthly_Summary
├── Status_Summary
└── Data_Quality
```

This is much closer to a practical ETL/reporting project.

***

## 1. Project Scenario

Imagine we are working for an e-commerce company.

We receive:

```
customers.csv
products.csv
sales.csv
```

The source data may contain:

```
Extra spaces
Different capitalization
Missing values
Invalid emails
Invalid amounts
Duplicate records
Incorrect dates
Invalid customer IDs
Invalid product IDs
```

Our job is to:

```
1. Read the source data
2. Inspect the data
3. Clean the data
4. Validate relationships
5. Join the datasets
6. Calculate business metrics
7. Generate summaries
8. Export everything into one Excel workbook
```

***

## 2. Input Data Relationship

### Customers

```
customer_id
customer_name
email
phone
city
state
pincode
age
gender
customer_since
customer_type
```

### Products

```
product_id
product_name
category
price
```

### Sales

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
```

Relationship:

```
Customers
-----------
customer_id
     │
     │ 1
     │
     │ many
     ↓
Sales
-----------
customer_id
product_id
     │
     │ many
     │
     │ 1
     ↓
Products
-----------
product_id
```

This is important because we are not simply cleaning three independent files.

We are cleaning **related datasets**.

***

## 3. Install Required Libraries

```python
pip install pandas openpyxl
```

For Jupyter:

```python
!pip install pandas openpyxl
```

***

## 4. Import Libraries

```python
import pandas as pd
import numpy as np

from pathlib import Path
```

***

## 5. Define Input Files

Assume our folder structure is:

```
project/
│
├── customers.csv
├── products.csv
└── sales.csv
```

Code:

```python
CUSTOMERS_FILE = "customers.csv"
PRODUCTS_FILE = "products.csv"
SALES_FILE = "sales.csv"
```

***

## 6. Extract — Read the Data

```python
customers = pd.read_csv(
    CUSTOMERS_FILE
)

products = pd.read_csv(
    PRODUCTS_FILE
)

sales = pd.read_csv(
    SALES_FILE
)
```

Check:

```python
print(
    "Customers:",
    customers.shape
)

print(
    "Products:",
    products.shape
)

print(
    "Sales:",
    sales.shape
)
```

***

## 7. Initial Data Profiling

Before cleaning anything, inspect the source.

```python
print("CUSTOMERS")
print(customers.head())

print("\nPRODUCTS")
print(products.head())

print("\nSALES")
print(sales.head())
```

***

## 8. Inspect Data Types

```python
print("\nCustomer Data Types")
print(customers.dtypes)

print("\nProduct Data Types")
print(products.dtypes)

print("\nSales Data Types")
print(sales.dtypes)
```

***

## 9. Check Missing Values

```python
print(
    "\nCustomers Missing Values"
)

print(
    customers.isna().sum()
)
```

```python
print(
    "\nProducts Missing Values"
)

print(
    products.isna().sum()
)
```

```python
print(
    "\nSales Missing Values"
)

print(
    sales.isna().sum()
)
```

***

## 10. Standardize Column Names

We want consistent column naming.

Example:

```
Customer ID
Customer Name
Order Date
```

should become:

```
customer_id
customer_name
order_date
```

Use:

```python
def clean_column_names(df):

    df = df.copy()

    df.columns = (
        df.columns
        .str.strip()
        .str.lower()
        .str.replace(" ", "_")
        .str.replace("-", "_")
    )

    return df
```

Apply:

```python
customers = clean_column_names(
    customers
)

products = clean_column_names(
    products
)

sales = clean_column_names(
    sales
)
```

***

## 11. Clean Customer Data

Create a dedicated function.

```python
def clean_customers(df):

    df = df.copy()

    # Remove duplicate customer IDs
    df = df.drop_duplicates(
        subset=["customer_id"],
        keep="first"
    )

    # Clean customer name
    df["customer_name"] = (
        df["customer_name"]
        .astype("string")
        .str.strip()
        .str.title()
    )

    # Clean email
    df["email"] = (
        df["email"]
        .astype("string")
        .str.strip()
        .str.lower()
    )

    # Clean city
    df["city"] = (
        df["city"]
        .astype("string")
        .str.strip()
        .str.title()
    )

    # Clean state
    df["state"] = (
        df["state"]
        .astype("string")
        .str.strip()
        .str.title()
    )

    # Convert age
    df["age"] = pd.to_numeric(
        df["age"],
        errors="coerce"
    )

    # Convert customer registration date
    df["customer_since"] = pd.to_datetime(
        df["customer_since"],
        errors="coerce"
    )

    # Standardize gender
    df["gender"] = (
        df["gender"]
        .astype("string")
        .str.strip()
        .str.title()
    )

    # Standardize customer type
    df["customer_type"] = (
        df["customer_type"]
        .astype("string")
        .str.strip()
        .str.title()
    )

    return df
```

Apply:

```python
customers = clean_customers(
    customers
)
```

***

## 12. Email Validation

We can use Regex.

```python
email_pattern = (
    r"^[\w.-]+@[\w.-]+\.\w+$"
)
```

Check valid email:

```python
valid_email = customers["email"].str.match(
    email_pattern,
    na=False
)
```

Invalid emails:

```python
invalid_emails = customers[
    ~valid_email
]
```

We can convert them to missing:

```python
customers.loc[
    ~valid_email,
    "email"
] = pd.NA
```

***

## 13. Clean Product Data

```python
def clean_products(df):

    df = df.copy()

    # Remove duplicate products
    df = df.drop_duplicates(
        subset=["product_id"],
        keep="first"
    )

    # Clean product name
    df["product_name"] = (
        df["product_name"]
        .astype("string")
        .str.strip()
        .str.title()
    )

    # Clean category
    df["category"] = (
        df["category"]
        .astype("string")
        .str.strip()
        .str.title()
    )

    # Convert price
    df["price"] = pd.to_numeric(
        df["price"],
        errors="coerce"
    )

    # Remove invalid prices
    df = df[
        df["price"].notna()
    ]

    # Price should not be negative
    df = df[
        df["price"] >= 0
    ]

    return df
```

Apply:

```python
products = clean_products(
    products
)
```

***

## 14. Clean Sales Data

```python
def clean_sales(df):

    df = df.copy()

    # --------------------------------
    # IDs
    # --------------------------------

    df["order_id"] = pd.to_numeric(
        df["order_id"],
        errors="coerce"
    )

    df["customer_id"] = pd.to_numeric(
        df["customer_id"],
        errors="coerce"
    )

    df["product_id"] = pd.to_numeric(
        df["product_id"],
        errors="coerce"
    )

    # --------------------------------
    # Dates
    # --------------------------------

    df["order_date"] = pd.to_datetime(
        df["order_date"],
        errors="coerce"
    )

    # --------------------------------
    # Numeric columns
    # --------------------------------

    numeric_columns = [
        "quantity",
        "unit_price",
        "gross_amount",
        "discount_percent",
        "discount_amount",
        "tax_percent",
        "tax_amount",
        "final_amount"
    ]

    for column in numeric_columns:

        df[column] = pd.to_numeric(
            df[column],
            errors="coerce"
        )

    # --------------------------------
    # String columns
    # --------------------------------

    string_columns = [
        "payment_method",
        "order_status"
    ]

    for column in string_columns:

        df[column] = (
            df[column]
            .astype("string")
            .str.strip()
            .str.title()
        )

    # --------------------------------
    # Remove duplicate orders
    # --------------------------------

    df = df.drop_duplicates(
        subset=["order_id"],
        keep="first"
    )

    # --------------------------------
    # Remove missing critical fields
    # --------------------------------

    df = df.dropna(
        subset=[
            "order_id",
            "customer_id",
            "product_id",
            "order_date",
            "quantity",
            "final_amount"
        ]
    )

    # --------------------------------
    # Business validation
    # --------------------------------

    df = df[
        df["quantity"] > 0
    ]

    df = df[
        df["final_amount"] >= 0
    ]

    return df
```

Apply:

```python
sales = clean_sales(
    sales
)
```

***

## 15. Referential Integrity

Now we need to make sure:

```
Every customer_id in sales
exists in customers
```

and:

```
Every product_id in sales
exists in products
```

This is an important database/data-engineering concept.

***

## 16. Find Invalid Customer IDs

```python
invalid_customer_ids = sales[
    ~sales["customer_id"].isin(
        customers["customer_id"]
    )
]
```

Check:

```python
print(
    invalid_customer_ids
)
```

***

## 17. Find Invalid Product IDs

```python
invalid_product_ids = sales[
    ~sales["product_id"].isin(
        products["product_id"]
    )
]
```

Check:

```python
print(
    invalid_product_ids
)
```

***

## 18. Remove Orphan Records

For this project, we will remove records whose customer or product does not exist.

```python
sales = sales[
    sales["customer_id"].isin(
        customers["customer_id"]
    )
]

sales = sales[
    sales["product_id"].isin(
        products["product_id"]
    )
]
```

***

## 19. Create the Sales Detail Dataset

Now we combine:

```
Sales
 +
Customers
 +
Products
```

First join sales with products:

```python
sales_detail = sales.merge(
    products,
    on="product_id",
    how="left",
    suffixes=(
        "",
        "_master"
    )
)
```

Then join customers:

```python
sales_detail = sales_detail.merge(
    customers[
        [
            "customer_id",
            "customer_name",
            "email",
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

***

## 20. Final Sales Detail

Now we have:

```
Order Information
       +
Product Information
       +
Customer Information
```

Check:

```python
print(
    sales_detail.head()
)
```

***

## 21. Calculate Revenue Metrics

Gross amount:

```
quantity × unit price
```

We can recalculate this instead of blindly trusting source data.

```python
sales_detail["calculated_gross"] = (
    sales_detail["quantity"] *
    sales_detail["unit_price"]
)
```

***

## 22. Calculate Discount

```python
sales_detail["calculated_discount"] = (
    sales_detail["calculated_gross"] *
    sales_detail["discount_percent"] /
    100
)
```

***

## 23. Calculate Taxable Amount

```python
sales_detail["taxable_amount"] = (
    sales_detail["calculated_gross"]
    -
    sales_detail["calculated_discount"]
)
```

***

## 24. Calculate Tax

```python
sales_detail["calculated_tax"] = (
    sales_detail["taxable_amount"] *
    sales_detail["tax_percent"] /
    100
)
```

***

## 25. Calculate Final Revenue

```python
sales_detail["net_amount"] = (
    sales_detail["taxable_amount"] +
    sales_detail["calculated_tax"]
)
```

Now we have a derived business metric.

***

## 26. Add Year and Month

```python
sales_detail["order_year"] = (
    sales_detail["order_date"].dt.year
)

sales_detail["order_month"] = (
    sales_detail["order_date"].dt.month
)

sales_detail["month_name"] = (
    sales_detail["order_date"]
    .dt.month_name()
)
```

***

## 27. Create Customer Summary

We want to know:

```
Customer
Total Orders
Total Quantity
Total Revenue
Average Order Value
```

Use:

```python
customer_summary = (
    sales_detail
    .groupby(
        [
            "customer_id",
            "customer_name",
            "city",
            "state",
            "customer_type"
        ],
        as_index=False
    )
    .agg(
        total_orders=(
            "order_id",
            "nunique"
        ),
        total_quantity=(
            "quantity",
            "sum"
        ),
        total_revenue=(
            "net_amount",
            "sum"
        ),
        average_order_value=(
            "net_amount",
            "mean"
        )
    )
)
```

***

## 28. Round Financial Columns

```python
customer_summary[
    [
        "total_revenue",
        "average_order_value"
    ]
] = customer_summary[
    [
        "total_revenue",
        "average_order_value"
    ]
].round(2)
```

***

## 29. Create Product Summary

We want:

```
Product
Category
Total Orders
Units Sold
Revenue
Average Selling Value
```

```python
product_summary = (
    sales_detail
    .groupby(
        [
            "product_id",
            "product_name",
            "category"
        ],
        as_index=False
    )
    .agg(
        total_orders=(
            "order_id",
            "nunique"
        ),
        units_sold=(
            "quantity",
            "sum"
        ),
        total_revenue=(
            "net_amount",
            "sum"
        ),
        average_order_value=(
            "net_amount",
            "mean"
        )
    )
)
```

***

## 30. Create City Summary

```python
city_summary = (
    sales_detail
    .groupby(
        [
            "city",
            "state"
        ],
        as_index=False
    )
    .agg(
        total_orders=(
            "order_id",
            "nunique"
        ),
        total_customers=(
            "customer_id",
            "nunique"
        ),
        total_revenue=(
            "net_amount",
            "sum"
        )
    )
)
```

***

## 31. Create Monthly Summary

```python
monthly_summary = (
    sales_detail
    .groupby(
        [
            "order_year",
            "order_month",
            "month_name"
        ],
        as_index=False
    )
    .agg(
        total_orders=(
            "order_id",
            "nunique"
        ),
        total_quantity=(
            "quantity",
            "sum"
        ),
        total_revenue=(
            "net_amount",
            "sum"
        )
    )
)
```

Sort:

```python
monthly_summary = (
    monthly_summary
    .sort_values(
        [
            "order_year",
            "order_month"
        ]
    )
)
```

***

## 32. Create Status Summary

This tells us how many orders are in each status.

```python
status_summary = (
    sales_detail
    .groupby(
        "order_status",
        as_index=False
    )
    .agg(
        order_count=(
            "order_id",
            "nunique"
        ),
        total_revenue=(
            "net_amount",
            "sum"
        )
    )
)
```

***

## 33. Create Payment Summary

```python
payment_summary = (
    sales_detail
    .groupby(
        "payment_method",
        as_index=False
    )
    .agg(
        order_count=(
            "order_id",
            "nunique"
        ),
        total_revenue=(
            "net_amount",
            "sum"
        )
    )
)
```

***

## 34. Create Category Summary

```python
category_summary = (
    sales_detail
    .groupby(
        "category",
        as_index=False
    )
    .agg(
        total_orders=(
            "order_id",
            "nunique"
        ),
        units_sold=(
            "quantity",
            "sum"
        ),
        total_revenue=(
            "net_amount",
            "sum"
        )
    )
)
```

***

## 35. Create Overall KPI Summary

We can create a small management summary.

```python
total_orders = (
    sales_detail["order_id"]
    .nunique()
)

total_customers = (
    sales_detail["customer_id"]
    .nunique()
)

total_products = (
    sales_detail["product_id"]
    .nunique()
)

total_quantity = (
    sales_detail["quantity"]
    .sum()
)

total_revenue = (
    sales_detail["net_amount"]
    .sum()
)

average_order_value = (
    sales_detail["net_amount"]
    .mean()
)
```

Create a DataFrame:

```python
kpi_summary = pd.DataFrame({
    "Metric": [
        "Total Orders",
        "Total Customers",
        "Total Products",
        "Total Quantity",
        "Total Revenue",
        "Average Order Value"
    ],

    "Value": [
        total_orders,
        total_customers,
        total_products,
        total_quantity,
        round(
            total_revenue,
            2
        ),
        round(
            average_order_value,
            2
        )
    ]
})
```

***

## 36. Data Quality Report

A professional ETL process should record what happened to the data.

Create:

```python
data_quality = pd.DataFrame({
    "Dataset": [
        "Customers",
        "Products",
        "Sales"
    ],

    "Rows_After_Cleaning": [
        len(customers),
        len(products),
        len(sales)
    ],

    "Columns": [
        len(customers.columns),
        len(products.columns),
        len(sales.columns)
    ],

    "Missing_Values": [
        customers.isna().sum().sum(),
        products.isna().sum().sum(),
        sales.isna().sum().sum()
    ],

    "Duplicate_Rows": [
        customers.duplicated().sum(),
        products.duplicated().sum(),
        sales.duplicated().sum()
    ]
})
```

***

## 37. More Detailed Data Quality Checks

Check orphan customers:

```python
orphan_customers = (
    sales[
        ~sales["customer_id"].isin(
            customers["customer_id"]
        )
    ]
    .shape[0]
)
```

Check orphan products:

```python
orphan_products = (
    sales[
        ~sales["product_id"].isin(
            products["product_id"]
        )
    ]
    .shape[0]
)
```

Add these:

```python
data_quality["Orphan_Customer_References"] = [
    orphan_customers,
    np.nan,
    np.nan
]

data_quality["Orphan_Product_References"] = [
    np.nan,
    np.nan,
    orphan_products
]
```

***

## 38. Final Clean Customers

Before exporting:

```python
customers = (
    customers
    .sort_values("customer_id")
    .reset_index(drop=True)
)
```

***

## 39. Final Clean Products

```python
products = (
    products
    .sort_values("product_id")
    .reset_index(drop=True)
)
```

***

## 40. Final Clean Sales

```python
sales = (
    sales
    .sort_values("order_id")
    .reset_index(drop=True)
)
```

***

## 41. Sort Sales Detail

```python
sales_detail = (
    sales_detail
    .sort_values("order_date")
    .reset_index(drop=True)
)
```

***

## 42. Create the Excel Report

Now we combine all our outputs into **one Excel workbook**.

```python
output_file = "ecommerce_final_report.xlsx"
```

Use:

```python
with pd.ExcelWriter(
    output_file,
    engine="openpyxl"
) as writer:

    kpi_summary.to_excel(
        writer,
        sheet_name="KPI_Summary",
        index=False
    )

    customers.to_excel(
        writer,
        sheet_name="Customers",
        index=False
    )

    products.to_excel(
        writer,
        sheet_name="Products",
        index=False
    )

    sales.to_excel(
        writer,
        sheet_name="Clean_Sales",
        index=False
    )

    sales_detail.to_excel(
        writer,
        sheet_name="Sales_Detail",
        index=False
    )

    customer_summary.to_excel(
        writer,
        sheet_name="Customer_Summary",
        index=False
    )

    product_summary.to_excel(
        writer,
        sheet_name="Product_Summary",
        index=False
    )

    city_summary.to_excel(
        writer,
        sheet_name="City_Summary",
        index=False
    )

    monthly_summary.to_excel(
        writer,
        sheet_name="Monthly_Summary",
        index=False
    )

    status_summary.to_excel(
        writer,
        sheet_name="Status_Summary",
        index=False
    )

    payment_summary.to_excel(
        writer,
        sheet_name="Payment_Summary",
        index=False
    )

    category_summary.to_excel(
        writer,
        sheet_name="Category_Summary",
        index=False
    )

    data_quality.to_excel(
        writer,
        sheet_name="Data_Quality",
        index=False
    )
```

***

## 43. Final Workbook

The output will be:

```
ecommerce_final_report.xlsx
```

With:

```
┌───────────────────────────────────┐
│ ecommerce_final_report.xlsx       │
├───────────────────────────────────┤
│ KPI_Summary                       │
│ Customers                         │
│ Products                          │
│ Clean_Sales                       │
│ Sales_Detail                      │
│ Customer_Summary                  │
│ Product_Summary                   │
│ City_Summary                      │
│ Monthly_Summary                   │
│ Status_Summary                    │
│ Payment_Summary                   │
│ Category_Summary                  │
│ Data_Quality                      │
└───────────────────────────────────┘
```

***

## 44. Format the Excel Workbook

We can make the final report more presentable using `openpyxl`.

```python
from openpyxl import load_workbook
from openpyxl.styles import Font, PatternFill, Alignment
from openpyxl.utils import get_column_letter
```

Load workbook:

```python
wb = load_workbook(
    "ecommerce_final_report.xlsx"
)
```

***

## 45. Format Headers

```python
header_fill = PatternFill(
    fill_type="solid",
    fgColor="1F4E78"
)

header_font = Font(
    bold=True,
    color="FFFFFF"
)

for ws in wb.worksheets:

    for cell in ws[1]:

        cell.fill = header_fill
        cell.font = header_font
        cell.alignment = Alignment(
            horizontal="center"
        )
```

***

## 46. Auto-Adjust Column Width

```python
for ws in wb.worksheets:

    for column_cells in ws.columns:

        max_length = 0

        column_letter = get_column_letter(
            column_cells[0].column
        )

        for cell in column_cells:

            try:

                cell_length = len(
                    str(cell.value)
                )

                max_length = max(
                    max_length,
                    cell_length
                )

            except Exception:
                pass

        ws.column_dimensions[
            column_letter
        ].width = min(
            max_length + 2,
            35
        )
```

***

## 47. Freeze Headers

```python
for ws in wb.worksheets:

    ws.freeze_panes = "A2"
```

Now when scrolling:

```
Header row
```

remains visible.

***

## 48. Add Auto Filter

```python
for ws in wb.worksheets:

    if ws.max_row > 1:

        ws.auto_filter.ref = (
            ws.dimensions
        )
```

This makes it easier to filter the report directly inside Excel.

***

## 49. Format Currency Columns

Some sheets contain financial columns.

We can apply Excel currency formatting.

```python
currency_columns = {
    "Sales_Detail": [
        "unit_price",
        "gross_amount",
        "discount_amount",
        "tax_amount",
        "calculated_gross",
        "calculated_discount",
        "taxable_amount",
        "calculated_tax",
        "net_amount"
    ],

    "Customer_Summary": [
        "total_revenue",
        "average_order_value"
    ],

    "Product_Summary": [
        "total_revenue",
        "average_order_value"
    ],

    "City_Summary": [
        "total_revenue"
    ],

    "Monthly_Summary": [
        "total_revenue"
    ],

    "Status_Summary": [
        "total_revenue"
    ],

    "Payment_Summary": [
        "total_revenue"
    ],

    "Category_Summary": [
        "total_revenue"
    ]
}
```

Apply formatting:

```python
for sheet_name, columns in currency_columns.items():

    ws = wb[sheet_name]

    headers = {
        cell.value: cell.column
        for cell in ws[1]
    }

    for column_name in columns:

        if column_name not in headers:
            continue

        column_number = headers[
            column_name
        ]

        for row in range(
            2,
            ws.max_row + 1
        ):

            ws.cell(
                row=row,
                column=column_number
            ).number_format = (
                '₹#,##0.00'
            )
```

***

## 50. Save the Formatted Workbook

```python
wb.save(
    "ecommerce_final_report.xlsx"
)
```

Output:

```
ecommerce_final_report.xlsx
```

***

## 51. Complete Final Script

The complete project can be organized like this:

```python
import pandas as pd
import numpy as np

from pathlib import Path

from openpyxl import load_workbook
from openpyxl.styles import Font, PatternFill, Alignment
from openpyxl.utils import get_column_letter


# ============================================================
# CONFIGURATION
# ============================================================

CUSTOMERS_FILE = "customers.csv"
PRODUCTS_FILE = "products.csv"
SALES_FILE = "sales.csv"

OUTPUT_FILE = "ecommerce_final_report.xlsx"


# ============================================================
# HELPER FUNCTION
# ============================================================

def clean_column_names(df):

    df = df.copy()

    df.columns = (
        df.columns
        .str.strip()
        .str.lower()
        .str.replace(" ", "_")
        .str.replace("-", "_")
    )

    return df


# ============================================================
# CLEAN CUSTOMERS
# ============================================================

def clean_customers(df):

    df = df.copy()

    df = df.drop_duplicates(
        subset=["customer_id"],
        keep="first"
    )

    df["customer_name"] = (
        df["customer_name"]
        .astype("string")
        .str.strip()
        .str.title()
    )

    df["email"] = (
        df["email"]
        .astype("string")
        .str.strip()
        .str.lower()
    )

    df["city"] = (
        df["city"]
        .astype("string")
        .str.strip()
        .str.title()
    )

    df["state"] = (
        df["state"]
        .astype("string")
        .str.strip()
        .str.title()
    )

    df["age"] = pd.to_numeric(
        df["age"],
        errors="coerce"
    )

    df["customer_since"] = pd.to_datetime(
        df["customer_since"],
        errors="coerce"
    )

    df["gender"] = (
        df["gender"]
        .astype("string")
        .str.strip()
        .str.title()
    )

    df["customer_type"] = (
        df["customer_type"]
        .astype("string")
        .str.strip()
        .str.title()
    )

    # Email validation

    email_pattern = (
        r"^[\w.-]+@[\w.-]+\.\w+$"
    )

    valid_email = df["email"].str.match(
        email_pattern,
        na=False
    )

    df.loc[
        ~valid_email,
        "email"
    ] = pd.NA

    return df


# ============================================================
# CLEAN PRODUCTS
# ============================================================

def clean_products(df):

    df = df.copy()

    df = df.drop_duplicates(
        subset=["product_id"],
        keep="first"
    )

    df["product_name"] = (
        df["product_name"]
        .astype("string")
        .str.strip()
        .str.title()
    )

    df["category"] = (
        df["category"]
        .astype("string")
        .str.strip()
        .str.title()
    )

    df["price"] = pd.to_numeric(
        df["price"],
        errors="coerce"
    )

    df = df.dropna(
        subset=["price"]
    )

    df = df[
        df["price"] >= 0
    ]

    return df


# ============================================================
# CLEAN SALES
# ============================================================

def clean_sales(df):

    df = df.copy()

    id_columns = [
        "order_id",
        "customer_id",
        "product_id"
    ]

    for column in id_columns:

        df[column] = pd.to_numeric(
            df[column],
            errors="coerce"
        )

    df["order_date"] = pd.to_datetime(
        df["order_date"],
        errors="coerce"
    )

    numeric_columns = [
        "quantity",
        "unit_price",
        "gross_amount",
        "discount_percent",
        "discount_amount",
        "tax_percent",
        "tax_amount",
        "final_amount"
    ]

    for column in numeric_columns:

        df[column] = pd.to_numeric(
            df[column],
            errors="coerce"
        )

    string_columns = [
        "payment_method",
        "order_status"
    ]

    for column in string_columns:

        df[column] = (
            df[column]
            .astype("string")
            .str.strip()
            .str.title()
        )

    # Remove duplicate orders

    df = df.drop_duplicates(
        subset=["order_id"],
        keep="first"
    )

    # Required fields

    df = df.dropna(
        subset=[
            "order_id",
            "customer_id",
            "product_id",
            "order_date",
            "quantity",
            "final_amount"
        ]
    )

    # Business rules

    df = df[
        df["quantity"] > 0
    ]

    df = df[
        df["final_amount"] >= 0
    ]

    return df


# ============================================================
# EXTRACT
# ============================================================

customers = pd.read_csv(
    CUSTOMERS_FILE
)

products = pd.read_csv(
    PRODUCTS_FILE
)

sales = pd.read_csv(
    SALES_FILE
)


# ============================================================
# STANDARDIZE COLUMN NAMES
# ============================================================

customers = clean_column_names(
    customers
)

products = clean_column_names(
    products
)

sales = clean_column_names(
    sales
)


# ============================================================
# DATA QUALITY BEFORE CLEANING
# ============================================================

raw_customer_rows = len(
    customers
)

raw_product_rows = len(
    products
)

raw_sales_rows = len(
    sales
)


# ============================================================
# TRANSFORMATION / CLEANING
# ============================================================

customers = clean_customers(
    customers
)

products = clean_products(
    products
)

sales = clean_sales(
    sales
)


# ============================================================
# REFERENTIAL INTEGRITY
# ============================================================

invalid_customer_ids = sales[
    ~sales["customer_id"].isin(
        customers["customer_id"]
    )
]

invalid_product_ids = sales[
    ~sales["product_id"].isin(
        products["product_id"]
    )
]

orphan_customer_count = len(
    invalid_customer_ids
)

orphan_product_count = len(
    invalid_product_ids
)


# Remove orphan records

sales = sales[
    sales["customer_id"].isin(
        customers["customer_id"]
    )
]

sales = sales[
    sales["product_id"].isin(
        products["product_id"]
    )
]


# ============================================================
# SALES DETAIL
# ============================================================

sales_detail = sales.merge(
    products,
    on="product_id",
    how="left",
    suffixes=(
        "",
        "_master"
    )
)

sales_detail = sales_detail.merge(
    customers[
        [
            "customer_id",
            "customer_name",
            "email",
            "city",
            "state",
            "gender",
            "customer_type"
        ]
    ],
    on="customer_id",
    how="left"
)


# ============================================================
# BUSINESS CALCULATIONS
# ============================================================

sales_detail["calculated_gross"] = (
    sales_detail["quantity"] *
    sales_detail["unit_price"]
)

sales_detail["calculated_discount"] = (
    sales_detail["calculated_gross"] *
    sales_detail["discount_percent"] /
    100
)

sales_detail["taxable_amount"] = (
    sales_detail["calculated_gross"]
    -
    sales_detail["calculated_discount"]
)

sales_detail["calculated_tax"] = (
    sales_detail["taxable_amount"] *
    sales_detail["tax_percent"] /
    100
)

sales_detail["net_amount"] = (
    sales_detail["taxable_amount"] +
    sales_detail["calculated_tax"]
)

sales_detail["order_year"] = (
    sales_detail["order_date"].dt.year
)

sales_detail["order_month"] = (
    sales_detail["order_date"].dt.month
)

sales_detail["month_name"] = (
    sales_detail["order_date"]
    .dt.month_name()
)


# ============================================================
# CUSTOMER SUMMARY
# ============================================================

customer_summary = (
    sales_detail
    .groupby(
        [
            "customer_id",
            "customer_name",
            "city",
            "state",
            "customer_type"
        ],
        as_index=False
    )
    .agg(
        total_orders=(
            "order_id",
            "nunique"
        ),
        total_quantity=(
            "quantity",
            "sum"
        ),
        total_revenue=(
            "net_amount",
            "sum"
        ),
        average_order_value=(
            "net_amount",
            "mean"
        )
    )
)

customer_summary[
    [
        "total_revenue",
        "average_order_value"
    ]
] = customer_summary[
    [
        "total_revenue",
        "average_order_value"
    ]
].round(2)


# ============================================================
# PRODUCT SUMMARY
# ============================================================

product_summary = (
    sales_detail
    .groupby(
        [
            "product_id",
            "product_name",
            "category"
        ],
        as_index=False
    )
    .agg(
        total_orders=(
            "order_id",
            "nunique"
        ),
        units_sold=(
            "quantity",
            "sum"
        ),
        total_revenue=(
            "net_amount",
            "sum"
        ),
        average_order_value=(
            "net_amount",
            "mean"
        )
    )
)

product_summary[
    [
        "total_revenue",
        "average_order_value"
    ]
] = product_summary[
    [
        "total_revenue",
        "average_order_value"
    ]
].round(2)


# ============================================================
# CITY SUMMARY
# ============================================================

city_summary = (
    sales_detail
    .groupby(
        [
            "city",
            "state"
        ],
        as_index=False
    )
    .agg(
        total_orders=(
            "order_id",
            "nunique"
        ),
        total_customers=(
            "customer_id",
            "nunique"
        ),
        total_revenue=(
            "net_amount",
            "sum"
        )
    )
)

city_summary["total_revenue"] = (
    city_summary["total_revenue"]
    .round(2)
)


# ============================================================
# MONTHLY SUMMARY
# ============================================================

monthly_summary = (
    sales_detail
    .groupby(
        [
            "order_year",
            "order_month",
            "month_name"
        ],
        as_index=False
    )
    .agg(
        total_orders=(
            "order_id",
            "nunique"
        ),
        total_quantity=(
            "quantity",
            "sum"
        ),
        total_revenue=(
            "net_amount",
            "sum"
        )
    )
)

monthly_summary[
    "total_revenue"
] = monthly_summary[
    "total_revenue"
].round(2)

monthly_summary = (
    monthly_summary
    .sort_values(
        [
            "order_year",
            "order_month"
        ]
    )
)


# ============================================================
# STATUS SUMMARY
# ============================================================

status_summary = (
    sales_detail
    .groupby(
        "order_status",
        as_index=False
    )
    .agg(
        order_count=(
            "order_id",
            "nunique"
        ),
        total_revenue=(
            "net_amount",
            "sum"
        )
    )
)

status_summary[
    "total_revenue"
] = (
    status_summary[
        "total_revenue"
    ].round(2)
)


# ============================================================
# PAYMENT SUMMARY
# ============================================================

payment_summary = (
    sales_detail
    .groupby(
        "payment_method",
        as_index=False
    )
    .agg(
        order_count=(
            "order_id",
            "nunique"
        ),
        total_revenue=(
            "net_amount",
            "sum"
        )
    )
)

payment_summary[
    "total_revenue"
] = (
    payment_summary[
        "total_revenue"
    ].round(2)
)


# ============================================================
# CATEGORY SUMMARY
# ============================================================

category_summary = (
    sales_detail
    .groupby(
        "category",
        as_index=False
    )
    .agg(
        total_orders=(
            "order_id",
            "nunique"
        ),
        units_sold=(
            "quantity",
            "sum"
        ),
        total_revenue=(
            "net_amount",
            "sum"
        )
    )
)

category_summary[
    "total_revenue"
] = (
    category_summary[
        "total_revenue"
    ].round(2)
)


# ============================================================
# KPI SUMMARY
# ============================================================

total_orders = (
    sales_detail["order_id"]
    .nunique()
)

total_customers = (
    sales_detail["customer_id"]
    .nunique()
)

total_products = (
    sales_detail["product_id"]
    .nunique()
)

total_quantity = (
    sales_detail["quantity"]
    .sum()
)

total_revenue = (
    sales_detail["net_amount"]
    .sum()
)

average_order_value = (
    sales_detail["net_amount"]
    .mean()
)

kpi_summary = pd.DataFrame({

    "Metric": [
        "Total Orders",
        "Total Customers",
        "Total Products",
        "Total Quantity",
        "Total Revenue",
        "Average Order Value"
    ],

    "Value": [
        total_orders,
        total_customers,
        total_products,
        total_quantity,
        round(
            total_revenue,
            2
        ),
        round(
            average_order_value,
            2
        )
    ]
})


# ============================================================
# DATA QUALITY REPORT
# ============================================================

data_quality = pd.DataFrame({

    "Dataset": [
        "Customers",
        "Products",
        "Sales"
    ],

    "Raw_Rows": [
        raw_customer_rows,
        raw_product_rows,
        raw_sales_rows
    ],

    "Clean_Rows": [
        len(customers),
        len(products),
        len(sales)
    ],

    "Columns": [
        len(customers.columns),
        len(products.columns),
        len(sales.columns)
    ],

    "Missing_Values": [
        customers.isna().sum().sum(),
        products.isna().sum().sum(),
        sales.isna().sum().sum()
    ],

    "Duplicate_Rows": [
        customers.duplicated().sum(),
        products.duplicated().sum(),
        sales.duplicated().sum()
    ]
})


data_quality["Orphan_Customer_References"] = [
    np.nan,
    np.nan,
    orphan_customer_count
]

data_quality["Orphan_Product_References"] = [
    np.nan,
    np.nan,
    orphan_product_count
]


# ============================================================
# FINAL SORTING
# ============================================================

customers = (
    customers
    .sort_values("customer_id")
    .reset_index(drop=True)
)

products = (
    products
    .sort_values("product_id")
    .reset_index(drop=True)
)

sales = (
    sales
    .sort_values("order_id")
    .reset_index(drop=True)
)

sales_detail = (
    sales_detail
    .sort_values("order_date")
    .reset_index(drop=True)
)


# ============================================================
# WRITE EXCEL REPORT
# ============================================================

with pd.ExcelWriter(
    OUTPUT_FILE,
    engine="openpyxl"
) as writer:

    kpi_summary.to_excel(
        writer,
        sheet_name="KPI_Summary",
        index=False
    )

    customers.to_excel(
        writer,
        sheet_name="Customers",
        index=False
    )

    products.to_excel(
        writer,
        sheet_name="Products",
        index=False
    )

    sales.to_excel(
        writer,
        sheet_name="Clean_Sales",
        index=False
    )

    sales_detail.to_excel(
        writer,
        sheet_name="Sales_Detail",
        index=False
    )

    customer_summary.to_excel(
        writer,
        sheet_name="Customer_Summary",
        index=False
    )

    product_summary.to_excel(
        writer,
        sheet_name="Product_Summary",
        index=False
    )

    city_summary.to_excel(
        writer,
        sheet_name="City_Summary",
        index=False
    )

    monthly_summary.to_excel(
        writer,
        sheet_name="Monthly_Summary",
        index=False
    )

    status_summary.to_excel(
        writer,
        sheet_name="Status_Summary",
        index=False
    )

    payment_summary.to_excel(
        writer,
        sheet_name="Payment_Summary",
        index=False
    )

    category_summary.to_excel(
        writer,
        sheet_name="Category_Summary",
        index=False
    )

    data_quality.to_excel(
        writer,
        sheet_name="Data_Quality",
        index=False
    )


# ============================================================
# FORMAT EXCEL
# ============================================================

wb = load_workbook(
    OUTPUT_FILE
)

header_fill = PatternFill(
    fill_type="solid",
    fgColor="1F4E78"
)

header_font = Font(
    bold=True,
    color="FFFFFF"
)

for ws in wb.worksheets:

    # Header formatting

    for cell in ws[1]:

        cell.fill = header_fill
        cell.font = header_font
        cell.alignment = Alignment(
            horizontal="center"
        )

    # Freeze header

    ws.freeze_panes = "A2"

    # Auto filter

    if ws.max_row > 1:

        ws.auto_filter.ref = (
            ws.dimensions
        )

    # Column width

    for column_cells in ws.columns:

        max_length = 0

        column_letter = (
            get_column_letter(
                column_cells[0].column
            )
        )

        for cell in column_cells:

            try:

                cell_length = len(
                    str(cell.value)
                )

                max_length = max(
                    max_length,
                    cell_length
                )

            except Exception:
                pass

        ws.column_dimensions[
            column_letter
        ].width = min(
            max_length + 2,
            35
        )


# ============================================================
# CURRENCY FORMATTING
# ============================================================

currency_columns = {

    "KPI_Summary": [
        "Value"
    ],

    "Sales_Detail": [
        "unit_price",
        "gross_amount",
        "discount_amount",
        "tax_amount",
        "calculated_gross",
        "calculated_discount",
        "taxable_amount",
        "calculated_tax",
        "net_amount"
    ],

    "Customer_Summary": [
        "total_revenue",
        "average_order_value"
    ],

    "Product_Summary": [
        "total_revenue",
        "average_order_value"
    ],

    "City_Summary": [
        "total_revenue"
    ],

    "Monthly_Summary": [
        "total_revenue"
    ],

    "Status_Summary": [
        "total_revenue"
    ],

    "Payment_Summary": [
        "total_revenue"
    ],

    "Category_Summary": [
        "total_revenue"
    ]
}


for sheet_name, columns in (
    currency_columns.items()
):

    ws = wb[sheet_name]

    headers = {
        cell.value: cell.column
        for cell in ws[1]
    }

    for column_name in columns:

        if column_name not in headers:
            continue

        column_number = headers[
            column_name
        ]

        for row in range(
            2,
            ws.max_row + 1
        ):

            ws.cell(
                row=row,
                column=column_number
            ).number_format = (
                '₹#,##0.00'
            )


# ============================================================
# SAVE
# ============================================================

wb.save(
    OUTPUT_FILE
)

print(
    f"Final report created: {OUTPUT_FILE}"
)
```

***

## 52. What You Have Built

This is no longer just a Pandas exercise.

It represents a small **ETL + Reporting pipeline**:

```
                    SOURCE
                      │
       ┌──────────────┼──────────────┐
       ↓              ↓              ↓
  customers.csv   products.csv   sales.csv
       │              │              │
       └──────────────┼──────────────┘
                      ↓
                   EXTRACT
                      ↓
                Pandas DataFrames
                      ↓
                 DATA PROFILING
                      ↓
               DATA CLEANING
                      ↓
        ┌─────────────┼─────────────┐
        ↓             ↓             ↓
   Customers      Products        Sales
        │             │             │
        └─────────────┼─────────────┘
                      ↓
                  VALIDATION
                      ↓
              Referential Integrity
                      ↓
                    MERGE
                      ↓
              Sales Detail Dataset
                      ↓
               BUSINESS METRICS
                      ↓
        ┌─────────────┼─────────────┐
        ↓             ↓             ↓
 Customer Summary  Product Summary  City Summary
        │             │             │
        └─────────────┼─────────────┘
                      ↓
                Monthly Summary
                      ↓
                 KPI Summary
                      ↓
              DATA QUALITY REPORT
                      ↓
                    LOAD
                      ↓
       ecommerce_final_report.xlsx
```

***

## 53. Final Excel Report Structure

The final Excel file acts like a small reporting database:

```
KPI_Summary
    ↓
Management overview

Customers
    ↓
Clean customer master

Products
    ↓
Clean product master

Clean_Sales
    ↓
Clean transaction data

Sales_Detail
    ↓
Fully enriched transaction dataset

Customer_Summary
    ↓
Customer-level analysis

Product_Summary
    ↓
Product-level analysis

City_Summary
    ↓
Geographic analysis

Monthly_Summary
    ↓
Time-based analysis

Status_Summary
    ↓
Order status analysis

Payment_Summary
    ↓
Payment-method analysis

Category_Summary
    ↓
Category performance

Data_Quality
    ↓
ETL/data-quality audit
```

***

## 54. Skills Practiced in This Project

This one project gives practice with:

```
Python
│
├── Functions
├── Loops
├── Exception Handling
│
Pandas
│
├── read_csv()
├── DataFrame
├── Data Cleaning
├── Missing Values
├── Duplicates
├── String Cleaning
├── Regex
├── Type Conversion
├── Datetime
├── Filtering
├── merge()
├── groupby()
├── agg()
├── Sorting
└── Export
│
Excel
│
├── Multiple Sheets
├── Formatting
├── Filters
├── Freeze Panes
└── Currency Formatting
│
Data Engineering
│
├── Extract
├── Transform
├── Validate
├── Load
├── Referential Integrity
├── Data Quality
└── Reporting
```

***

## 55. Next Evolution of This Project

The same project can now be extended naturally:

```
Version 1
CSV → Pandas → Excel
```

Then:

```
Version 2
CSV / Excel → Pandas → SQLAlchemy → MySQL
```

Then:

```
Version 3
MySQL → SQLAlchemy → Pandas
              ↓
            Clean
              ↓
          Target Table
```

Then:

```
Version 4
CSV / Database
      ↓
   PySpark
      ↓
   Transform
      ↓
   Parquet
```

Eventually:

```
CSV / Excel / API / Database
              ↓
           Extract
              ↓
        Spark / Databricks
              ↓
      Bronze / Silver / Gold
              ↓
       Data Warehouse
              ↓
      BI / Power BI
```

This makes the project reusable throughout your Data Engineering learning path rather than being a one-time Pandas exercise.
