# 🛍️ Customer Behaviour Analysis — End-to-End Data Analytics Project

![Python](https://img.shields.io/badge/Python-3.10-blue?logo=python) ![MySQL](https://img.shields.io/badge/MySQL-8.0-orange?logo=mysql) ![PowerBI](https://img.shields.io/badge/Power%20BI-Dashboard-yellow?logo=powerbi) ![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

---

##  Overview

This is a complete end-to-end data analytics project focused on understanding **customer shopping behaviour**. The project covers every stage of a real-world analytics workflow — from raw data loading and cleaning to SQL analysis, interactive dashboards, and a stakeholder-ready presentation.

The goal is to uncover insights about customer demographics, purchase patterns, product preferences, and seasonal trends to support business decision-making.

---

## 📂 Dataset

| Detail | Info |
|---|---|
| **Dataset Name** | Customer Shopping Behaviour Dataset |
| **Source** | MySQL Database (`customers`) |
| **Table** | `custom` |
| **Records** | ~4,000 customers |
| **Format** | Structured / Tabular |

### Columns Used

| Column | Description |
|---|---|
| `customer_id` | Unique customer identifier |
| `age` | Customer age |
| `gender` | Male / Female |
| `item_purchased` | Product name |
| `category` | Product category |
| `purchase_amount_(usd)` | Transaction value in USD |
| `location` | Customer city/state |
| `season` | Season of purchase |
| `review_rating` | Customer rating (1–5) |
| `subscription_status` | Subscribed Yes/No |
| `discount_applied` | Discount used Yes/No |
| `previous_purchases` | Number of past purchases |
| `payment_method` | Payment type used |
| `frequency_of_purchases` | How often customer buys |
| `age_group` | Grouped age category |

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| **Python** (Pandas, Matplotlib, Seaborn) | Data loading, EDA, cleaning |
| **MySQL** | Database storage and SQL queries |
| **Power BI Desktop** | Interactive dashboard |
| **Jupyter Notebook** | Analysis environment |
| **GitHub** | Version control and project sharing |

---

## 🔄 Project Workflow

```
Raw Dataset
    ↓
Python (Load + EDA + Clean)
    ↓
MySQL (SQL Queries + Analysis)
    ↓
Power BI (Dashboard)
```

---

## 📋 Steps

### Step 1 — Data Loading
- Connected to MySQL database using `mysql-connector-python`
- Loaded the `custom` table into a Pandas DataFrame
- Verified shape, data types, and null values

```python
import pandas as pd
import mysql.connector

conn = mysql.connector.connect(
    host="localhost",
    port=3306,
    user="root",
    password="your_password",
    database="customers"
)

df = pd.read_sql("SELECT * FROM custom", conn)
print(df.shape)
print(df.head())
```

---

### Step 2 — Exploratory Data Analysis (EDA)
- Checked data distribution, value counts, and outliers
- Visualised key columns using Matplotlib and Seaborn
- Key EDA findings:
  - Female customers form the majority (53.75%)
  - Clothing is the highest revenue category
  - Fall season drives the highest purchase volume
  - Young Adults (18–25) have the highest purchase frequency

```python
import matplotlib.pyplot as plt
import seaborn as sns

# Revenue by category
df.groupby('category')['purchase_amount_(usd)'].sum().sort_values().plot(kind='barh')
plt.title('Revenue by Category')
plt.show()
```

---

### Step 3 — Data Cleaning
- Removed duplicate records
- Standardised column names (lowercase, underscores)
- Handled null/missing values
- Created derived column: `age_group`
- Validated data types for numeric columns

```python
# Remove duplicates
df.drop_duplicates(inplace=True)

# Create age group
def age_group(age):
    if age <= 25: return 'Young Adults'
    elif age <= 35: return 'Adults'
    elif age <= 50: return 'Middle Aged'
    else: return 'Senior'

df['age_group'] = df['age'].apply(age_group)
```

---

### Step 4 — SQL Analysis (MySQL)
Key business questions answered using SQL:

```sql
-- Total revenue by category
SELECT category, SUM(purchase_amount_usd) AS total_revenue
FROM custom
GROUP BY category
ORDER BY total_revenue DESC;

-- Subscription rate
SELECT
  ROUND(SUM(CASE WHEN subscription_status = 'Yes' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2)
  AS subscription_rate
FROM custom;

-- Top 5 items by revenue
SELECT item_purchased, SUM(purchase_amount_usd) AS revenue
FROM custom
GROUP BY item_purchased
ORDER BY revenue DESC
LIMIT 5;

-- Revenue by age group
SELECT age_group, SUM(purchase_amount_usd) AS revenue
FROM custom
GROUP BY age_group
ORDER BY revenue DESC;

-- Average rating by category
SELECT category, ROUND(AVG(review_rating), 2) AS avg_rating
FROM custom
GROUP BY category;
```

---

### Step 5 — Power BI Dashboard

Connected Power BI Desktop to MySQL database and built an interactive dashboard.

#### DAX Measures Created

```dax
Total Customers = DISTINCTCOUNT('customers custom'[customer_id])

Avg Purchase Amount = AVERAGE('customers custom'[purchase_amount_(usd)])

Avg Review Rating = AVERAGE('customers custom'[review_rating])

Subscription Rate % =
DIVIDE(
    COUNTROWS(FILTER('customers custom', 'customers custom'[subscription_status] = "Yes")),
    COUNTROWS('customers custom'), 0
) * 100

Discount Rate % =
DIVIDE(
    COUNTROWS(FILTER('customers custom', 'customers custom'[discount_applied] = "Yes")),
    COUNTROWS('customers custom'), 0
) * 100
```

#### Dashboard Features
- 5 KPI cards — Total Customers, Avg Purchase, Avg Rating, Subscription Rate, Discount Rate
- 7 interactive charts — Revenue by Category, Sales by Gender, Sales by Items, Revenue by Age Group, Frequency of Purchase, Sales by Season, Avg Rating by Category
- 3 slicers — Age Group (tile), Category (tile), Location (dropdown)
- Navy + Teal professional colour theme

---

### Step 6 — Report & Presentation
- Written report summarising all findings and recommendations
- Presentation created using **Gamma** (AI-powered slides)
- Covers: project overview, methodology, key insights, business recommendations

---

## 📊 Dashboard Preview

> Built using Power BI Desktop with Navy `#1B2A4A` + Teal `#00B4D8` theme

| Section | Charts |
|---|---|
| KPI Row | Total Customers · Avg Purchase · Avg Rating · Subscription % · Discount % |
| Row 2 | Revenue by Category · Sales by Gender · Sales by Items (Top 5) |
| Row 3 | Revenue by Age Group · Frequency of Purchase · Sales by Season · Avg Rating by Category |
| Slicers | Age Group · Category · Location (dropdown) |

---

## 📈 Key Results & Insights

| Insight | Finding |
|---|---|
| 👥 Total Customers | 4,000 |
| 💰 Avg Purchase Amount | $59.76 |
| ⭐ Avg Review Rating | 3.75 / 5 |
| 📋 Subscription Rate | 27% |
| 🏷️ Discount Usage | 43% |
| 🏆 Top Category | Clothing ($104K revenue) |
| 👗 Top Item | Blouse (10.4K purchases) |
| 📅 Best Season | Fall (60K purchases) |
| 🔁 Most Common Frequency | Every 3 Months (584 customers) |
| 👩 Gender Split | Female 53.75% · Male 46.25% |
| 🧑 Top Age Group by Revenue | Young Adults ($62K) |
| ⭐ Highest Rated Category | Footwear (3.79 avg) |

---

## 💡 Business Recommendations

1. **Focus marketing on Clothing** — highest revenue category, run seasonal campaigns
2. **Target Young Adults** — highest revenue segment, invest in digital/social marketing
3. **Boost subscription rate** — currently only 27%, offer incentives to non-subscribers
4. **Leverage Fall season** — plan inventory and promotions for peak season
5. **Discount strategy** — 43% usage is high, consider loyalty rewards instead of blanket discounts
6. **Improve ratings** — Clothing has lowest rating (3.72), investigate product quality feedback

---

## ▶️ How to Run

### Prerequisites
```
Python 3.8+
MySQL 8.0
Power BI Desktop (free)
Jupyter Notebook
```

### Installation
```bash
# Clone the repository
git clone https://github.com/yourusername/customer-behaviour-analysis.git
cd customer-behaviour-analysis

# Install Python dependencies
pip install pandas matplotlib seaborn mysql-connector-python jupyter
```

### Database Setup
```bash
# Import dataset into MySQL
mysql -u root -p customers < data/custom.sql
```

### Run the Notebook
```bash
jupyter notebook notebooks/customer_behaviour_eda.ipynb
```

### Open Dashboard
```
1. Open Power BI Desktop
2. File → Open → dashboard/customer_behaviour.pbix
3. Refresh data connection
```

---

## 📁 Project Structure

```
customer-behaviour-analysis/
│
├── data/
│   ├── custom.csv              # Raw dataset
│   └── custom.sql              # MySQL dump
│
├── notebooks/
│   └── customer_behaviour_eda.ipynb   # Python EDA notebook
│
├── sql/
│   └── analysis_queries.sql    # All SQL queries
│
├── dashboard/
│   └── customer_behaviour.pbix # Power BI file
│
├── report/
│   └── customer_behaviour_report.pdf
│
└── README.md
```

---

