# 🌮 TacoExpress Delivery Performance Analysis

> **Using Python and Excel analytics to uncover revenue drivers, customer spending behaviour, tipping patterns and delivery performance for TacoExpress.**

---

## Overview

This project presents a data analytics workflow for analysing TacoExpress delivery transactions from 2024–2025.

The analysis uses Python and Excel to examine restaurant revenue, location performance, customer ordering behaviour, spending and tipping patterns, and delivery performance.

The goal is to transform raw delivery transaction data into actionable business insights that can help TacoExpress improve revenue, operational efficiency and customer experience.

---

## Project Objectives

The main business question was:

**How can TacoExpress use historical delivery data to improve revenue, customer experience and operational performance?**

The project aims to:

- Identify restaurants contributing the most revenue.
- Identify locations generating the highest revenue.
- Analyse customer ordering patterns.
- Examine customer spending behaviour.
- Analyse tipping behaviour.
- Investigate the relationship between customer spending and tipping.
- Compare delivery performance across restaurants and locations.
- Identify operational challenges and opportunities.
- Develop data-driven business recommendations.

---

## What Was Built

The project produces an end-to-end analytics workflow that:

1. Loads the TacoExpress transaction dataset.
2. Reviews dataset structure, variables and data types.
3. Checks for missing values and duplicate records.
4. Converts date and time variables into appropriate formats.
5. Creates additional analytical variables.
6. Performs exploratory data analysis.
7. Calculates revenue by restaurant and location.
8. Analyses customer ordering patterns.
9. Examines spending and tipping behaviour.
10. Evaluates delivery performance.
11. Generates business-focused visualisations.
12. Develops actionable recommendations.

---

## 🛠️ Tools & Technologies

### Python

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Jupyter Notebook



## 📊 Dataset

The project uses historical TacoExpress delivery transaction data covering 2024–2025.

### Variables

| Variable | Description |
|---|---|
| `Order ID` | Unique identifier for each order |
| `Restaurant Name` | Restaurant associated with the order |
| `Location` | Restaurant/order location |
| `Order Time` | Date and time the order was placed |
| `Delivery Time` | Date and time the order was delivered |
| `Delivery Duration (min)` | Delivery duration in minutes |
| `Taco Size` | Size of taco ordered |
| `Taco Type` | Type of taco ordered |
| `Toppings Count` | Number of toppings |
| `Distance (km)` | Delivery distance |
| `Price ($)` | Customer spending/order price |
| `Tip ($)` | Customer tip |
| `Weekend Order` | Indicates whether the order occurred on a weekend |

---

# 🔄 Analytical Workflow

```text
Raw TacoExpress Data
        │
        ▼
Data Loading
        │
        ▼
Data Quality Checks
        │
        ├── Variables
        ├── Records
        ├── Missing Values
        └── Duplicates
        │
        ▼
Data Cleaning
        │
        ▼
Feature Engineering
        │
        ├── Month
        ├── Hour
        ├── Day
        └── Tip %
        │
        ▼
Exploratory Data Analysis
        │
        ▼
Visualisation
        │
        ▼
Business Insights
        │
        ▼
Recommendations


##🧹 Data Cleaning & Preparation

The first stage of the project focused on understanding and preparing the dataset.

The following checks were performed:

Number of records
Number of variables
Column names
Data types
Missing values
Duplicate records
Unique values
Descriptive statistics

Example:

import pandas as pd

df = pd.read_csv("taco_sales_(2024-2025).csv")

print("Number of Records:", df.shape[0])
print("Number of Variables:", df.shape[1])

print("\nVariables:")
print(df.columns)

print("\nMissing Values:")
print(df.isnull().sum())

print("\nDuplicate Records:")
print(df.duplicated().sum())

print("\nData Types:")
print(df.dtypes)


##⚙️ Feature Engineering

Additional variables were created to support the analysis.

Month
df["Month"] = df["Order Time"].dt.month_name()
Hour
df["Hour"] = df["Order Time"].dt.hour
Day
df["Day"] = df["Order Time"].dt.day_name()
Tip Percentage
df["Tip %"] = (
    df["Tip ($)"] / df["Price ($)"]
) * 100

These variables allow the analysis to examine ordering times, trends and customer tipping behaviour.


##📈 Revenue Analysis

Revenue was analysed at both restaurant and location levels.

Revenue by Restaurant
restaurant_revenue = (
    df.groupby("Restaurant Name")["Price ($)"]
    .sum()
    .sort_values(ascending=False)
)

This analysis identifies the restaurants contributing the largest share of TacoExpress revenue.


##Business Question

Which restaurants contribute the most to revenue?

Revenue by Location
location_revenue = (
    df.groupby("Location")["Price ($)"]
    .sum()
    .sort_values(ascending=False)
)

This allows TacoExpress to identify locations with stronger customer demand.

##Business Question

Which locations generate the highest revenue?

#🕐 Customer Ordering Patterns

Customer order volume was analysed by hour.

orders_hour = (
    df.groupby("Hour")["Order ID"]
    .count()
)

A line chart was used to identify peak ordering periods.

##Business Application

Understanding peak ordering periods can help TacoExpress:

Schedule delivery personnel.
Allocate restaurant resources.
Improve order fulfilment.
Plan targeted promotions.
Reduce delivery delays.

#💰 Spending Behaviour

Customer spending was analysed using the Price ($) variable.

The analysis examines:

Average order value.
Distribution of order prices.
Spending ranges.
Spending patterns across restaurants and locations.

Example:

average_spending = df["Price ($)"].mean()

print("Average Spending:", average_spending)
💵 Tipping Behaviour

#Customer tipping behaviour was analysed using both absolute tip values and tip percentages.

Average Tip
average_tip = df["Tip ($)"].mean()

print("Average Tip:", average_tip)
Average Tip Percentage
df["Tip %"] = (
    df["Tip ($)"] / df["Price ($)"]
) * 100

average_tip_percentage = df["Tip %"].mean()

print(
    "Average Tip Percentage:",
    average_tip_percentage
)

#📊 Tipping vs Spending

The relationship between customer spending and tipping was also examined.

The main variables are:

Price ($) → Customer spending
Tip ($) → Customer tip

A scatter plot can be used to examine the relationship between spending and tipping.

import matplotlib.pyplot as plt

plt.figure(figsize=(10,6))

plt.scatter(
    df["Price ($)"],
    df["Tip ($)"],
    alpha=0.6
)

plt.title("Customer Tipping vs Spending")
plt.xlabel("Spending ($)")
plt.ylabel("Tip ($)")

plt.grid(True)

plt.show()

An alternative grouped line chart examines average tips across spending ranges.

bins = [0, 10, 20, 30, 40, 50, 60]

labels = [
    "$0-10",
    "$10-20",
    "$20-30",
    "$30-40",
    "$40-50",
    "$50-60"
]

df["Spending Range"] = pd.cut(
    df["Price ($)"],
    bins=bins,
    labels=labels
)

avg_tip = (
    df.groupby(
        "Spending Range",
        observed=True
    )["Tip ($)"]
    .mean()
)

plt.figure(figsize=(10,6))

plt.plot(
    avg_tip.index.astype(str),
    avg_tip.values,
    marker="o"
)

plt.title("Average Tip by Spending Range")
plt.xlabel("Spending Range")
plt.ylabel("Average Tip ($)")

plt.grid(True)

plt.show()
Business Question

Do customers who spend more also tend to tip more?


##🚚 Delivery Performance

Delivery performance was analysed using:

#Delivery Duration (min)

Average delivery time was calculated by restaurant.

delivery_time = (
    df.groupby("Restaurant Name")
    ["Delivery Duration (min)"]
    .mean()
    .sort_values()
)

This helps identify restaurants with faster and slower delivery performance.

The analysis can also be extended to:

Location
Distance
Restaurant
Time of day
Weekend vs weekday


#📅 Weekend vs Weekday Analysis

Revenue was compared between weekend and weekday orders.

weekend_revenue = (
    df.groupby("Weekend Order")["Price ($)"]
    .sum()
)

print(weekend_revenue)

This analysis helps determine whether weekend demand differs significantly from weekday demand.


##Business Application

If weekend demand is higher, TacoExpress can consider:

Increasing weekend staffing.
Increasing delivery capacity.
Running weekend promotions.
Preparing restaurants for higher order volumes.


##📊 Key Visualisations

The project produces several business-focused visualisations:

Revenue by Restaurant
Revenue by Location
Orders by Hour
Average Delivery Time
Weekend vs Weekday Revenue
Customer Spending Distribution
Customer Tipping Distribution
Tipping vs Spending
Average Tip by Spending Range
Tip Percentage Analysis
Delivery Performance Analysis


##💡 Key Insights

The analysis is designed to answer several important business questions.

Revenue

Identify the restaurants and locations responsible for the largest revenue contribution.

Customer Behaviour

Identify peak ordering periods and customer spending patterns.

Tipping

Determine how customer tips change with spending and identify overall tipping patterns.

Operations

Identify restaurants and locations with longer delivery times.

Business Growth

Identify high-performing areas that could receive additional marketing, staffing and operational investment.


##🚀 Recommendations

Based on the analysis, TacoExpress can consider the following strategies.

1. Optimise Peak-Hour Staffing

Increase delivery capacity during periods with high order volumes.

2. Improve Delivery Efficiency

Investigate locations and restaurants with consistently long delivery times.

3. Focus on High-Performing Restaurants

Allocate marketing and promotional resources toward restaurants generating strong revenue.

4. Improve Underperforming Locations

Investigate customer demand, pricing, delivery times and restaurant performance in lower-revenue locations.

5. Encourage Customer Loyalty

Use spending and ordering patterns to develop targeted loyalty programmes.

6. Use Data-Driven Staffing

Use historical ordering patterns to forecast future demand and optimise staffing.
