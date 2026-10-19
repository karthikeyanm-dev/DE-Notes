# Data Generation

> Imported from [`python/Pandas/Data/Data Generation.ipynb`](https://github.com/karthikeyanm-dev/DE-Notes/blob/main/python/Pandas/Data/Data%20Generation.ipynb). Code outputs are omitted; run the source notebook for live results.

```python
"""
Generates a realistic-but-messy E-COMMERCE ORDERS dataset for pandas practice.

Real-world messiness baked in on purpose:
- Missing values (NaN / blank / "N/A" strings) scattered across columns
- Duplicate rows (some exact, some near-duplicates with different order_id)
- Inconsistent text casing / spacing ("Chennai", "chennai ", " CHENNAI")
- Inconsistent date formats (YYYY-MM-DD, DD/MM/YYYY, MM-DD-YYYY, some blank)
- Mixed data types in a column that should be numeric (e.g. "1200", "1,200", "₹1200", "unknown")
- Outliers (absurdly high/low prices, negative quantities)
- Whitespace issues in emails/phone numbers
- Inconsistent category naming ("Electronics" vs "electronics" vs "Electronic")
- A currency-symbol column, a boolean-like column stored as strings ("Yes"/"no"/"Y"/"1")
"""

import random
import numpy as np
import pandas as pd
from datetime import datetime, timedelta

random.seed(42)
np.random.seed(42)

N = 1000  # number of order records

# ---- reference data ----
first_names = ["Arun", "Priya", "Karthik", "Divya", "Suresh", "Meena", "Vignesh",
               "Anitha", "Rahul", "Sneha", "Manoj", "Lakshmi", "Vijay", "Kavya",
               "Naveen", "Pooja", "Harish", "Deepa", "Sanjay", "Ritu"]
last_names = ["Kumar", "Raj", "Iyer", "Nair", "Reddy", "Sharma", "Pillai",
              "Menon", "Rao", "Gupta", "Chandran", "Subramaniam"]

cities_clean = ["Chennai", "Bengaluru", "Hyderabad", "Mumbai", "Delhi",
                "Pune", "Kolkata", "Coimbatore", "Kochi", "Ahmedabad"]

def messy_city(city):
    variant = random.choice(["normal", "lower", "upper", "space", "abbr"])
    if variant == "normal":
        return city
    elif variant == "lower":
        return city.lower()
    elif variant == "upper":
        return city.upper()
    elif variant == "space":
        return f" {city}  "
    elif variant == "abbr" and city == "Bengaluru":
        return "Bangalore"
    return city

categories_clean = ["Electronics", "Clothing", "Home & Kitchen", "Books",
                     "Beauty", "Sports", "Toys", "Grocery"]

def messy_category(cat):
    variant = random.choice(["normal", "lower", "upper", "typo"])
    if variant == "normal":
        return cat
    elif variant == "lower":
        return cat.lower()
    elif variant == "upper":
        return cat.upper()
    elif variant == "typo" and cat == "Electronics":
        return "Electronic"
    return cat

payment_methods = ["Credit Card", "Debit Card", "UPI", "Net Banking", "Cash on Delivery"]

product_by_category = {
    "Electronics": ["Wireless Earbuds", "Bluetooth Speaker", "Smartwatch", "Power Bank", "USB-C Cable"],
    "Clothing": ["Cotton T-Shirt", "Formal Shirt", "Denim Jeans", "Kurti", "Jacket"],
    "Home & Kitchen": ["Non-stick Pan", "Mixer Grinder", "LED Bulb", "Storage Container", "Water Bottle"],
    "Books": ["Novel", "Self-help Book", "Cookbook", "Textbook", "Comic Book"],
    "Beauty": ["Face Wash", "Sunscreen", "Lipstick", "Shampoo", "Moisturizer"],
    "Sports": ["Yoga Mat", "Cricket Bat", "Football", "Dumbbell Set", "Running Shoes"],
    "Toys": ["Building Blocks", "RC Car", "Puzzle Set", "Action Figure", "Board Game"],
    "Grocery": ["Basmati Rice 5kg", "Cooking Oil 1L", "Atta 10kg", "Sugar 1kg", "Tea Powder 500g"],
}

def random_date_messy():
    """Return a date string in one of several inconsistent formats, sometimes blank."""
    start = datetime(2024, 1, 1)
    end = datetime(2025, 12, 31)
    d = start + timedelta(days=random.randint(0, (end - start).days))
    fmt_choice = random.choice(["iso", "dmy_slash", "mdy_dash", "blank", "text"])
    if fmt_choice == "iso":
        return d.strftime("%Y-%m-%d")
    elif fmt_choice == "dmy_slash":
        return d.strftime("%d/%m/%Y")
    elif fmt_choice == "mdy_dash":
        return d.strftime("%m-%d-%Y")
    elif fmt_choice == "blank":
        return ""
    elif fmt_choice == "text":
        return d.strftime("%d %b %Y")  # e.g. "14 Mar 2024"

def messy_price(base_price):
    """Return price as inconsistent string/number representation."""
    variant = random.choice(["plain", "comma", "currency", "unknown", "plain", "plain"])
    if variant == "plain":
        return str(base_price)
    elif variant == "comma":
        return f"{base_price:,}"
    elif variant == "currency":
        return f"₹{base_price}"
    elif variant == "unknown":
        return "unknown"

def messy_bool():
    return random.choice(["Yes", "No", "Y", "N", "yes", "no", "1", "0", ""])

def messy_email(name):
    domain = random.choice(["gmail.com", "yahoo.com", "outlook.com", "rediffmail.com"])
    email = f"{name.lower().replace(' ', '.')}@{domain}"
    if random.random() < 0.15:
        email = f" {email} "  # stray whitespace
    if random.random() < 0.05:
        email = email.upper()
    return email

def messy_phone():
    num = "".join([str(random.randint(0, 9)) for _ in range(10)])
    variant = random.choice(["plain", "dashed", "spaced", "country_code", "plain"])
    if variant == "plain":
        return num
    elif variant == "dashed":
        return f"{num[:5]}-{num[5:]}"
    elif variant == "spaced":
        return f"{num[:5]} {num[5:]}"
    elif variant == "country_code":
        return f"+91{num}"

rows = []
for i in range(1, N + 1):
    order_id = f"ORD{10000 + i}"
    fname = random.choice(first_names)
    lname = random.choice(last_names)
    customer_name = f"{fname} {lname}"

    category_clean = random.choice(categories_clean)
    category = messy_category(category_clean)
    product_name = random.choice(product_by_category[category_clean])

    base_price = random.choice([149, 199, 249, 299, 349, 399, 499, 599, 699,
                                 799, 999, 1199, 1499, 1999, 2499, 2999, 4999])
    # inject occasional outliers
    if random.random() < 0.02:
        base_price = random.choice([99999, 150000, -500, 0])

    quantity = random.randint(1, 5)
    if random.random() < 0.015:
        quantity = random.choice([-1, -3, 0, 500])  # bad data

    city = messy_city(random.choice(cities_clean))
    payment = random.choice(payment_methods)

    rating = random.choice([1, 2, 3, 4, 5, np.nan, np.nan])  # some missing ratings

    row = {
        "order_id": order_id,
        "customer_name": customer_name if random.random() > 0.02 else None,
        "email": messy_email(customer_name) if random.random() > 0.05 else "",
        "phone": messy_phone() if random.random() > 0.08 else np.nan,
        "city": city if random.random() > 0.03 else "N/A",
        "category": category,
        "product_name": product_name,
        "price": messy_price(base_price),
        "quantity": quantity if random.random() > 0.04 else np.nan,
        "order_date": random_date_messy(),
        "payment_method": payment if random.random() > 0.03 else None,
        "is_prime_member": messy_bool(),
        "rating": rating,
        "delivered": messy_bool(),
    }
    rows.append(row)

df = pd.DataFrame(rows)

# ---- inject duplicates (exact copies of random existing rows, with new order_id) ----
dup_indices = np.random.choice(df.index, size=30, replace=False)
dup_rows = df.loc[dup_indices].copy()
dup_rows["order_id"] = [f"ORD{20000 + i}" for i in range(len(dup_rows))]
df = pd.concat([df, dup_rows], ignore_index=True)

# ---- inject a few fully exact duplicate rows (same order_id too) ----
exact_dups = df.sample(5, random_state=1)
df = pd.concat([df, exact_dups], ignore_index=True)

# shuffle rows so duplicates aren't neatly grouped
df = df.sample(frac=1, random_state=7).reset_index(drop=True)

# save
csv_path = "ecommerce_orders_dirty.csv"
xlsx_path = "ecommerce_orders_dirty.xlsx"
df.to_csv(csv_path, index=False)
df.to_excel(xlsx_path, index=False, sheet_name="orders")

print(f"Rows: {len(df)}, Columns: {len(df.columns)}")
print(df.head(10).to_string())
print("\nMissing values per column:\n", df.isna().sum().to_string())
print("\nDuplicate order_ids:", df['order_id'].duplicated().sum())
```
