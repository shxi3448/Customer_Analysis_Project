# Customer Shopping Behavior Analysis

## Project Overview

This project analyzes customer shopping behavior using Python, SQL, and Power BI. The dataset contains 3,900 customer purchase records across different product categories. My goal was to understand spending habits, purchasing preferences, and shopping patterns, then use those findings to suggest practical business improvements.

I started by cleaning and preparing the data in Python, used SQL to answer specific business questions, and created an interactive Power BI dashboard to present the results. This repository also includes a detailed report and a presentation summarizing the project.

## Business Problem

A retail company wants to better understand how its customers shop so it can improve sales, customer satisfaction, and long-term loyalty. Management has noticed changes in purchasing patterns across customer demographics, product categories, and online versus offline sales channels. They want to understand how factors such as discounts, product reviews, seasons, and payment preferences relate to purchasing decisions and repeat purchases.

The main question guiding this project is:

> How can the company leverage consumer shopping data to identify trends, improve customer engagement, and optimize marketing and product strategies?

To address this problem, the project was organized around five deliverables:

1. **Data preparation and modeling:** Clean and transform the raw data using Python.
2. **SQL analysis:** Explore customer segments, loyalty, and purchase drivers through structured queries.
3. **Visualization:** Build a Power BI dashboard to highlight important shopping patterns.
4. **Report and presentation:** Explain the findings and recommend actions the business could consider.
5. **GitHub repository:** Bring the code, dashboard, and supporting project materials together in one place.

The analysis in this repository focuses on the questions supported by the available dataset and SQL queries. Not every factor mentioned in the original business problem, such as online versus offline sales channels, is analyzed separately.

## Tools and Technologies

- **Python and Pandas:** Data exploration, cleaning, and feature engineering
- **Jupyter Notebook:** Documenting the Python workflow
- **MySQL:** Running SQL queries against the customer data
- **Visual Studio Code:** Connecting to MySQL and writing and executing queries
- **Power BI Service:** Building the dashboard in a web browser on macOS
- **CSV:** Transferring the prepared dataset into Power BI

## Development Environment and Challenges

I completed this project on a Mac, which presented some challenges when working with Power BI.

I was able to connect Visual Studio Code to MySQL and run my SQL queries successfully. However, I could not establish a direct connection between MySQL and Power BI using my setup. Power BI Desktop does not have a native macOS application, so I worked in the browser-based Power BI Service instead.

To work around the connection issue, I uploaded the prepared customer dataset as a CSV file into Power BI Service. This allowed me to create the dashboard without a direct MySQL connection. I also made the necessary data adjustments in Power BI when needed.

Although the workflow was different from what I originally planned, I was still able to complete the SQL analysis and visualize the results. It also gave me experience troubleshooting compatibility issues between tools.

## Dataset

The dataset contains **3,900 purchase records** and **18 original columns** covering customer demographics, purchase details, and shopping behavior.

| Feature | Description |
|---|---|
| Customer ID | Customer identifier |
| Age | Customer age |
| Gender | Customer gender |
| Item Purchased | Purchased product |
| Category | Product category |
| Purchase Amount | Amount spent in USD |
| Location | Customer location |
| Size and Color | Product characteristics |
| Season | Season associated with the purchase |
| Review Rating | Customer product rating |
| Subscription Status | Whether the customer has a subscription |
| Shipping Type | Selected shipping method |
| Discount Applied | Whether a discount was applied |
| Promo Code Used | Whether a promotional code was used |
| Previous Purchases | Number of previous purchases |
| Payment Method | Customer payment method |
| Frequency of Purchases | How often the customer purchases |

During the initial exploration, I found **37 missing values** in the Review Rating column.

## Project Workflow

### 1. Data Cleaning and Preparation in Python

I loaded the dataset into a Jupyter Notebook using Pandas and examined its structure, summary statistics, and missing values with functions including `df.info()`, `df.describe()`, and `df.isnull().sum()`.

**Handling missing ratings:** I filled missing Review Rating values using the median rating within each product category. The median is less affected by unusually high or low values than the mean, and calculating it by category helped account for differences between product types.

**Standardizing column names:** I changed column names to snake_case, making them easier to reference consistently in SQL. For example, `Purchase Amount (USD)` became `purchase_amount`.

**Creating age groups:** I used `qcut()` to divide customer ages into four groups: Young Adult, Adult, Middle-aged, and Senior. Quantile-based grouping helped keep the groups relatively balanced for comparison.

**Converting purchase frequency:** I created a `purchase_frequency_days` feature by mapping text-based purchase frequencies to numerical day values.

**Checking redundant columns:** I compared `discount_applied` and `promo_code_used`, found that their values matched, and removed the redundant promo code column.

After preparing the data, I used it for the SQL analysis and dashboard.

### 2. Customer Behavior Analysis in SQL

I wrote **10 SQL queries** to investigate spending, products, discounts, subscriptions, and customer segments.

| Question | Business question |
|---|---|
| Q1 | How much revenue comes from male versus female customers? |
| Q2 | Which discounted purchases were at least as large as the overall average purchase amount? |
| Q3 | Which five products have the highest average review ratings? |
| Q4 | How do average purchase amounts compare between Standard and Express shipping? |
| Q5 | Do subscribers spend more than non-subscribers, and how do the groups compare in total revenue? |
| Q6 | Which five products have the highest percentage of discounted purchases? |
| Q7 | How many customers fall into New, Returning, and Loyal segments? |
| Q8 | What are the top three ranked products within each category? |
| Q9 | What is the subscription breakdown among customers with more than five previous purchases? |
| Q10 | How much revenue does each age group contribute? |

#### SQL Techniques and Decisions

**Aggregations:** I used `COUNT()`, `SUM()`, and `AVG()` to calculate customer counts, revenue, average spending, and product ratings. `GROUP BY` made it possible to compare these metrics across customer groups and categories.

**Subqueries:** For Q2, I calculated the overall average purchase amount in a subquery, then filtered for discounted purchases meeting or exceeding that average.

**CASE statements and CTEs:** For Q7, I grouped customers by previous purchases: New (1), Returning (2 to 10), and Loyal (more than 10). I used a CTE to define these segments before counting them.

**Window functions:** For Q8, I used `RANK()` with `PARTITION BY` to rank products separately within each category. I chose `RANK()` instead of `ROW_NUMBER()` so products with equal purchase counts would share the same rank. In Clothing, Blouses and Pants both had 171 purchases and shared rank 1, while Shirts had 169 purchases and received rank 3.

**Sorting:** For Q5, I added `ORDER BY total_revenue DESC, avg_spend DESC`. This puts the highest-revenue group first and uses average spending to resolve any revenue ties. It makes the results easier to read but does not change the calculations.

The full SQL code is in [`customer_behavior_sql_queries.sql`](customer_behavior_sql_queries.sql).

### 3. Power BI Dashboard

I built a dashboard in Power BI Service to make the findings easier to explore and compare.

The dashboard displays three main metrics:

| Metric | Value |
|---|---:|
| Number of Customers | 3,900 |
| Average Review Rating | 3.75 |
| Average Purchase Amount | $59.76 |

The visuals compare subscription status, revenue and purchase counts by product category, and revenue and purchase counts by age group. I also added filters for subscription status, gender, category, and shipping type so viewers can focus on specific customer groups.

See [`Customer Behavior Dashboard.pdf`](Customer%20Behavior%20Dashboard.pdf) for the dashboard and [`Customer Behavior Dashboard.pbix`](Customer%20Behavior%20Dashboard.pbix) for the Power BI project file.

## Key Findings

### Subscription Behavior

Non-subscribers made up about **73%** of customers and generated most of the revenue.

| Subscription Status | Customers | Average Spend | Total Revenue |
|---|---:|---:|---:|
| No | 2,847 | $59.87 | $170,436 |
| Yes | 1,053 | $59.49 | $62,645 |

The average purchase amounts were close, with non-subscribers spending slightly more per transaction. This means subscription status alone did not indicate higher spending in this dataset.

### Revenue by Product Category

Clothing generated the most revenue and had the highest purchase count, followed by Accessories, Footwear, and Outerwear in revenue. This makes Clothing an important category to consider when planning inventory and promotions.

### Revenue by Age Group

Young Adults contributed approximately **$62K** in revenue and recorded **1,028 purchases**. Middle-aged customers contributed about **$59K**, while Adults and Seniors each contributed about **$56K**. Young Adults led in both revenue and purchase count.

### Popular Products

Within Clothing, Blouses and Pants tied for first with **171 purchases each**. Shirts followed with **169 purchases**. The ranking query makes these ties visible rather than arbitrarily assigning different positions.

### Discounts

The discount queries helped identify products with high discount usage and discounted purchases that still exceeded the average purchase amount. These comparisons can inform future promotion decisions, although purchase data alone cannot establish whether a discount caused a sale.

## Business Recommendations

1. **Encourage subscriptions.** Since most customers are non-subscribers, exclusive benefits or targeted promotions may help increase enrollment.
2. **Support customer loyalty.** A rewards program could encourage repeat purchases and help returning customers become more loyal.
3. **Review discount strategies.** Products frequently sold at a discount deserve a closer look to balance sales activity with profit margins.
4. **Highlight popular products.** Best-selling and highly rated products could receive more visibility in promotions and inventory planning.
5. **Use customer segments in marketing.** Age-group and category trends can help tailor campaigns to different audiences.

## Repository Files

| File | Description |
|---|---|
| [`Customer_Shopping_Behavior_Analysis.ipynb`](Customer_Shopping_Behavior_Analysis.ipynb) | Python exploration, cleaning, and feature engineering |
| [`customer_behavior_sql_queries.sql`](customer_behavior_sql_queries.sql) | Ten SQL business questions and their queries |
| [`Customer Behavior Dashboard.pbix`](Customer%20Behavior%20Dashboard.pbix) | Power BI project |
| [`Customer Behavior Dashboard.pdf`](Customer%20Behavior%20Dashboard.pdf) | Exported dashboard |
| [`Customer_Shopping_Behavior_Report_Clean.pdf`](Customer_Shopping_Behavior_Report_Clean.pdf) | Detailed project report |
| Presentation | Slide presentation of the project and findings (add the final filename here) |

## How to Explore the Project

1. Start with the Jupyter Notebook to see how the data was prepared.
2. Read the SQL file to see the questions and query logic.
3. Open the dashboard PDF to view the final visualizations.
4. Read the project report for additional explanations and business recommendations.
5. View the presentation for a summary of the project.

## Conclusion

This project gave me practice with the full data analysis workflow, from cleaning a dataset to answering business questions and presenting the findings. Python helped me prepare the data, SQL helped me explore customer behavior, and Power BI made the results easier to communicate.

One takeaway was that assumptions do not always match the data. For example, subscribers did not have a higher average purchase amount than non-subscribers. Working through these questions helped me strengthen my data cleaning, SQL, visualization, and analytical communication skills.
