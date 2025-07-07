# E-Commerce Executive Dashboard: Sales, Returns & AOV Trends (2021–2025)

## 📌 Overview

This project is a personal portfolio dashboard built using Power BI to analyze e-commerce sales performance. It showcases key metrics like Total Sales, Average Order Value (AOV), Customer Lifetime Value (CLV), Conversion Rate, and Return Rate.

---

## 🎯 Why I Created This Dashboard

Recently, I interviewed for a company where I confidently answered most of their questions. However, when I was asked to recall DAX formulas for AOV and CLV, I couldn't remember them clearly — even though I had used them in a previous role. That moment made me realize I needed to rebuild a dashboard from scratch for my portfolio — not only to showcase my skills but also to help me **retain and explain important DAX measures** I’ve used in real-world projects.

---

## ⚙️ Project Structure

```
ecommerce-dashboard/
│
├── dataset/
│   ├── ecommerce_data.csv
│   └── E-Commerce Dashboard.pbix
│
├── images/
│   ├── data-model.png
│   ├── dashboard-overview.png
│   ├── aov-dax.png
│   ├── clv-dax.png
│   ├── conversion-dax.png
│   ├── returnrate-dax.png
│   └── targetaov-dax.png
│
└── README.md
```

---

## 🚧 Challenges Faced

1. **Finding the right data**: In my previous job, I had access to live data warehouses. Here, I had to manually collect data from public sources like Kaggle, Google Datasets, and synthetic data generators.
2. **Building on a Mac**: Power BI isn’t available on macOS. I had to set up a virtual Windows environment and troubleshoot file sharing issues between Mac and Windows VM. Eventually, I uploaded the CSV to the Windows side and built the model from there.
3. **Modeling from scratch**: Unlike work environments where schema and transformations are defined, here I had to model everything from raw CSV files myself.

---

## 📊 Data Model

![Data Model](https://github.com/Agastyav-1/ecommerce-powerbi-dashboard/blob/db73b1f9c91a2c6f0b07b42026f997794d9fae0d/Ecommerce%20Dashboard/Modeling%20and%20measures%20images/Data_Model.png)

This dashboard follows a **star schema** with the central Fact_Sales table connected to dimension tables for Customer, Product, Campaign, and Date. Measures are stored in a separate `_measures` table for better manageability.

---

## 📐 DAX Measures and Logic

### 1️⃣ **Average Order Value (AOV)**  
```DAX
AOV = DIVIDE(SUM(Fact_sales[TotalAmount]), SUM(Fact_sales[Quantity]))
```
**Why this formula?**  
To calculate AOV, I divided total sales revenue by the number of items sold. This gives a precise average per order across all transactions.

📷 Screenshot: ![AOV DAX](images/aov-dax.png)

---

### 2️⃣ **Customer Conversion Rate (%)**  
```DAX
Conversion Rate (%) = 
DIVIDE(
    DISTINCTCOUNT(Fact_Sales[CustomerID]),
    CALCULATE(DISTINCTCOUNT(Fact_Sales[CustomerID]), ALL(Fact_Sales))
) * 100
```
**Why this formula?**  
This DAX is designed to show conversion relative to total distinct customers. Using `ALL(Fact_Sales)` in the denominator removes filters and helps understand segment performance within the entire customer base.

📷 Screenshot: ![Conversion DAX](images/conversion-dax.png)

---

### 3️⃣ **Return Rate (%)**  
```DAX
Return Rate (%) = 
DIVIDE(
    CALCULATE(COUNTROWS(Fact_Sales), Fact_Sales[Returned] = 1),
    COUNTROWS(Fact_Sales)
)
```
**Why this formula?**  
It calculates the proportion of transactions marked as returned. Using a calculated condition inside `CALCULATE` helps isolate only returned rows while keeping the base denominator as total sales rows.

📷 Screenshot: ![Return Rate DAX](images/returnrate-dax.png)

---

### 4️⃣ **Target AOV**  
```DAX
Target_AOV =
VAR _YearMonth = SELECTEDVALUE(Dim_Date[YearMonth])
RETURN
    IF (
        NOT ISBLANK(_YearMonth),
        200,
        BLANK()
    )
```
**Why this formula?**  
I used this to dynamically compare AOV against a fixed business goal ($200) only when a valid period (YearMonth) is selected. This prevents cluttering the visuals when no time range is applied.

📷 Screenshot: ![Target AOV DAX](images/targetaov-dax.png)

---

## 📊 Dashboard Preview

![Dashboard](images/dashboard-overview.png)

The dashboard supports filtering by year, month, product category, and campaign type. KPIs are visualized using a mix of cards, clustered column charts, and matrix tables for quick executive insights.

---

## 🛠 Tools Used

- **Power BI Desktop**
- **DAX (Data Analysis Expressions)**
- **Star schema data modeling**
- **Mac M2 + VirtualBox (Windows VM)** for Power BI compatibility
- **Excel & Google Sheets** (for initial data transformation)

---

## 🙌 Thank You

This project helped me rebuild my confidence in using DAX, while also solving real challenges like data acquisition, modeling, and cross-OS setup. I hope this helps other analysts struggling with similar blockers.
