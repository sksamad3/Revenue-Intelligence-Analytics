# Revenue Intelligence: Sales, Profit & Discount Analysis
### *Includes period-over-period comparisons, top/bottom product analysis, regional performance, and KPI tracking.*

### Dashboard Link : https://drive.google.com/drive/folders/1qbxzzC9Vi8yRpzOJ_t0rhXW-cSljy15H?usp=sharing

## Problem Statement

The business generates large volumes of sales data, but currently lacks a clear and interactive way to track product performance, profitability, discounts, and regional sales. Management is unable to easily identify which products are driving revenue, which are causing losses, or how sales and profit are changing over time.

Because of this, it is difficult to compare different time periods, evaluate the impact of discounts, and detect sales trends across cities and customer segments. These gaps lead to inefficient pricing, weak promotional decisions, and missed growth opportunities.

This dashboard solves the problem by providing real-time visibility into sales, profit, quantity sold, net sales, and discount behavior, allowing users to analyze performance across products, locations, customers, and time to support better business decisions.



## Project Objective

- The objective of this dashboard is to help business stakeholders:
- Monitor overall sales and profit performance
- Identify top and bottom performing products
- Analyze the impact of discounts on revenue and profitability
- Track sales trends across different time periods
- Compare performance between regions, promotions, and customer segments



#### Key Features
- Interactive filters for Year, Quarter, and Month
- Drill-down analysis by product, promotion, and location
- KPIs highlighting revenue sacrificed to discounts and return on discount
- Time-series trend analysis for sales performance
- Comparative visuals for sales vs profit across states and promotions



## Data Description

The dashboard is built using a star schema data model consisting of one fact table and multiple dimension tables:

### Fact Table
- Index
- Customer ID
- Order Date
- Promotion ID
- Product ID
- Units sold 
- Discount value
- Net Sales 
- Pricer Per Unit (INR) 
- Total Sales
- Discount Percentage
- Profit 



### Dimension Tables

#### Product 
- ProductID 
- Product Name
- Product Line
- Price per Unit


#### Customer 
- Customer ID 
- Customer Name
- City
- State
- Pincode 
- Email ID  
- Phone number



#### Promotion
- PromotionID 
- Promotion Name 
- Ad Type
- Coupon Code
- Price Reduction type 
- Discount Percentage


#### Date 
- Date 
- Month
- Quarter
- Year


### Data Modeling Enhancements
- To improve analytical capabilities and follow best practices:
- Dedicated Date tables were created to enable time intelligence analysis such as month-over-month and year-over-year comparisons.
- A separate Measures table was created to organize all DAX measures for better readability and maintainability.
- Additional calculated columns were added where necessary to support business analysis.





## Data Preparation
- Before building the dashboard, the data was prepared and validated to ensure accuracy and consistency.
- Loaded all datasets into the Power BI model
- Opened Power Query Editor and renamed tables and columns for clarity
- Performed column profiling, column quality checks, and value distribution checks to understand the data
- Verified and corrected data types for every column
- This step helped in understanding the structure of the data and prepared a strong foundation for data modeling.
- Key Learning:
Always verify column data types, because incorrect data types can break relationships and lead to inaccurate report results.



## Data Modeling Steps
- Several important transformations were performed before building relationships:
- Some columns in the Fact table such as Price Per Unit, Total Sales, Discount Percentage, Discount Value, and Net Sales contained null values and needed to be derived.

**Steps taken:**
- **Price Per Unit :**
Merged the Product table with the Fact table using Product ID to bring the price into the fact table.
- **Total Sales:**
Calculated using:
```Total Sales = Price Per Unit × Units Sold```
- **Discount Percentage:** Merged the Promotion table with the Fact table using Promotion ID to identify the discount applicable for each transaction.
- Null values were replaced with 0 to avoid calculation errors.

- Discount Value
Calculated using:
```Discount Value = (Discount Percentage × Total Sales) / 100```




### Relationship Preparation

- Before creating relationships, key columns were checked to ensure:
- Matching data types
- No missing or inconsistent values
- This prevents broken relationships and incorrect DAX results later.



### Data Model Design
- Implemented a Star Schema in Model View
- Fact table placed at the center
- Dimension tables arranged around it
- Relationships created between dimension tables and fact table using key columns


## Business Questions & Dashboard Implementation

The dashboard was developed to answer key business questions using appropriate visuals, filters, and DAX calculations. Each visual was designed not only to display data but to support decision-making.

---

### 1.  Top / Bottom 5 Products by Sales, Profit, and Quantity Sold for Accessories , clothing and footwear. 

**Visual :**
A stacked bar chart was used to display product performance. Visual-level Top N filters were applied to dynamically show the Top 5 or Bottom 5 products

**Insight:**
This analysis highlights the best-performing products driving revenue and profitability, while also revealing low-performing products that may need pricing changes, promotional support, or possible discontinuation.

 **Highest Performing**
- Raymond suit - 274 units sold 
- Raymond suit - 4.1 Million sales 
- Raymond suit - 0.14 million profit.

**Lowest Performing**
- Fab India Kurta - 210 units sold
- H&M Tshirt  - 0.37 million sales
- H&M Tshirt - 0.04 million profit



---

### 2.  Sales Trends Over Time (Daily, Monthly, Quarterly, Annually)

**Implementation:**
Used Three slicers (Year, Quarter, Month)  added  
 a line chart that dynamically updates based on the selected time period, allowing multi-level trend analysis.

**Insight:**
- Sales spikes indicate high-demand periods where the business can focus on marketing and stock readiness, 
while lower sales months highlight the need for promotions and inventory control to maintain steady performance.
---




### 3. Compare Sales, Profit, and Quantity Sold Between Two Selected Periods

**Implementation:**
Two separate Date tables were created using `CALENDARAUTO()` and connected to the fact table using active and inactive relationships.

Two slicers were created:

* First slicer controls Base Period (active relationship)
* Second slicer controls Comparison Period (inactive relationship activated using DAX)

A clustered column chart was used to compare values between the two periods.  3 visuals for Quantity , sales , Profit. 

**DAX Measures Used:**

```DAX
Comparison Period Sum of Profit  =
CALCULATE(
    SUM('Fact Table'[Profit]),
    ALL('Date Table 1'),
    USERELATIONSHIP('Date Table 2'[Date], 'Fact Table'[General Date])
)
```

```DAX
Comparison Period Sum of Sales  =
CALCULATE(
    SUM('Fact Table'[Net Sales]),
    ALL('Date Table 1'),
    USERELATIONSHIP('Date Table 2'[Date], 'Fact Table'[General Date])
)
```

```DAX
Comparison Period Sum of Quantity  =
CALCULATE(
    SUM('Fact Table'[Units Sold]),
    ALL('Date Table 1'),
    USERELATIONSHIP('Date Table 2'[Date], 'Fact Table'[General Date])
)
```

**Insight:**
This enables direct performance comparison between two time periods, helping stakeholders measure growth, evaluate promotional effectiveness, and understand business performance changes over time.

---

### 5. Average Discount Offered in Each Promotion Category

**Implementation:**
A stacked column chart was used to display the average discount percentage across promotion categories.

**Insight:**
The analysis shows how aggressive each promotion type is. It helps determine whether higher discounts are actually leading to stronger performance or simply reducing profit margins.

---

### 6. Total Number of Orders

**Implementation:**
A KPI card was used to display the total number of orders.

**Insight:**
This provides a quick measure of business activity and helps track order volume trends when combined with time filters.

---

### 7. Order-Level Detailed Analysis

**Implementation:**
A detailed table visual was created to show Sales, Profit, Discount, Net Sales, and other fields at the individual order level.
This table can be filtered using slicers for Product, Date, Customer ID, and Promotion Category.

**Insight:**
This view allows granular investigation of specific transactions, supporting operational analysis such as identifying unusually high discounts, low-profit orders, or customer-level purchasing behavior.

---

### 8.  Sales & Profit by Different Cities

**Implementation:**
Used a Clustered column chart to compare sales and profit by each city .

**Insight:**
This helps identify high-performing regions that contribute most to revenue and underperforming locations where marketing efforts or distribution strategies may need improvement.





## Key KPIs and Advanced Metrics
### **1. Revenue Given as Discount (%)**

**Visual Used:**
Card


<img width="362" height="340" alt="Image" src="https://github.com/user-attachments/assets/e26ba1d6-2732-41f1-939d-bffbfc8460ec" />

**Implementation:**
Calculated the average discount percentage only for transactions where a discount was applied.

```DAX 
Revenue Given as Discount (%) = AVERAGEX(
                                FILTER(
                                        'Fact Table', 
                                        'Fact Table'[Discount Percentage] > 0),
                                'Fact Table'[Discount Percentage]
                                )
```


**Insight:**
On orders where a discount was applied, the average discount rate is 27%, 
This showed promotions are relatively deep when used. 
This helps assess whether discounting strategies are driving sales effectively without excessively reducing margins.



### **2. Return on Discount (ROD)**

**Visual Used:**
Card


<img width="363" height="327" alt="Image" src="https://github.com/user-attachments/assets/6fdc2aba-2c41-4971-93d1-d7bbfa1639e4" />



**Implementation:**
Measured how much profit is generated for every unit of discount offered.

```
Return on Discount (ROD) % = DIVIDE(
                                    SUM('Fact Table'[Profit]),
                                    SUM('Fact Table'[Discount Value])
                                    )
```
**Insight:**
The result was 1.70 , which shows that for every 1Rupee spent on discount the business earned 70 paise as profit that sums to 1.7rs, 
The ROD value of 1.70 shows that for every ₹1 spent on discounts, the business earned ₹1.70 in profit. This indicates that discount
 strategies are generating profitable returns rather than simply reducing margins.



### **3. Total Orders**

**Visual Used:**
Card




**Implementation:**
Counted the total number of transactions to measure overall order volume.

**Insight:**
This KPI provides a quick view of business activity and helps track demand trends over time when combined with date filters.



---

### 4.  Period Comparison KPIs

####  Comparison Period Sales

**Visual Used:**
Bar Chart and Card


<img width="1425" height="793" alt="Image" src="https://github.com/user-attachments/assets/fb26dccf-fff4-452c-8ea4-6e27d516abbb" />


**Implementation:**

Created a system that allows users to select a base period and a comparison period to evaluate performance side by side. Measures were designed to calculate Sales, Profit, and Quantity for both periods, along with the difference and growth rate between them.

Implemented period comparison using two date tables — 
one active for the base period and 
one inactive for the comparison period. 

The USERELATIONSHIP() function was used inside CALCULATE() to activate the secondary date relationship while removing the primary date filter, enabling independent time-based calculations for Sales, Profit, and Quantity.

**Insight:**
This allows the business to evaluate sales performance in a different time frame without affecting the primary reporting period, enabling flexible period-over-period analysis.




---
### **5.  Sales Difference**

**Visual Used:**
Card


<img width="213" height="156" alt="Image" src="https://github.com/user-attachments/assets/81863c6f-6651-4d59-b768-934c0c5a0034" />


**Implementation:**
Calculated the absolute difference between **comparison period sales** and **base period sales**.

**Insight:**
This KPI shows how much sales have increased or decreased between two selected periods, helping identify performance improvement or decline in clear monetary terms.

---

### **6.  Sales Growth (%)**

**Visual Used:**
Card


<img width="236" height="162" alt="Image" src="https://github.com/user-attachments/assets/44cc1a19-a13c-4016-ac86-a15f2910c3fe" />




**Implementation:**
Divided the sales difference by base period sales to measure **percentage change in performance** between the two periods.

**Insight:**
This metric highlights the rate of growth or decline over time, helping stakeholders quickly assess business momentum and the effectiveness of recent strategies.

---

These KPIs provide a **complete period-over-period performance view**, showing both absolute and percentage change, which supports performance evaluation.
        
  - Step 18 : The report was then published to Power BI Service.
 







![Publish_Message](https://user-images.githubusercontent.com/102996550/174094520-3a845196-97e6-4d44-8760-34a64abc3e77.jpg)

# Snapshot of Dashboard (Power BI Service)

![dashboard_snapo](https://user-images.githubusercontent.com/102996550/174096257-11f1aae5-203d-44fc-bfca-25d37faf3237.jpg)

 
 # Report Snapshot (Power BI DESKTOP)

<img width="1426" height="800" alt="Image" src="https://github.com/user-attachments/assets/5bb60c8f-40cc-4c30-8bde-53e42a357548" />


<img width="1420" height="792" alt="Image" src="https://github.com/user-attachments/assets/00f9f3d8-5162-4d65-ae30-895156e0e39c" />

<img width="1421" height="798" alt="Image" src="https://github.com/user-attachments/assets/e1bce28b-66d4-4256-923b-9f8cde92edb6" />





# Insights
Ah yes! Let’s include **the period-over-period numbers** in a human-friendly way with figures and percentages so it reads naturally:

---

## Key Insights from Dashboard

### [1] Top Products

* Raymond Suit sold 274 units, generating ₹4.1M in sales and ₹0.14M profit.
* High-demand, high-margin products — consider focused promotions and inventory planning.

### [2] Low Products

* H&M T-shirt and Fab India Kurta have low sales and profit.
* Could benefit from price adjustments, targeted promotions, or potential discontinuation.

### [3] Sales Trends

* Sales spikes occur in peak months — stock and marketing should be ready.
* Low-sales months indicate opportunities for promotions to boost revenue.

### [4] Discounts

* Average discount on discounted orders is 27%.
* Return on Discount (ROD) is 1.70, meaning every ₹1 spent on discount earned ₹1.70 in profit.

### [5] Period-over-Period Performance
For any two selected periods
* Sales increased by **X%** compared to the previous period.
* Profit grew by **Y%**, and quantity sold went up by **Z%**.
* This shows overall positive momentum, with growth in revenue, profit, and sales volume.

### [6] Regional Sales

* Certain cities consistently generate higher sales and profit.
* Underperforming areas may need targeted marketing or distribution improvements.

### [7] Orders

* Total orders reached **X**, showing steady customer activity and demand trends.




A single page report was created on Power BI Desktop & it was then published to Power BI Service.

