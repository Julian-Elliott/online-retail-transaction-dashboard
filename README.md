# Online Retail Transaction Dashboard

**Online Retail Transaction Dashboard** is a comprehensive data analytics tool designed to streamline customer behavior analysis and sales trend exploration. Using the Online Retail dataset, we developed an interactive dashboard to support data-driven decision-making across product performance, customer segmentation, and geographic markets.

---

## 🗃️ Dataset Content

The dataset contains records of transactions made through an online retail platform between December 2010 and December 2011. It includes:
- `InvoiceNo`: Transaction ID
- `StockCode`: Product ID
- `Description`: Product name
- `Quantity`: Quantity sold (can be negative for returns)
- `InvoiceDate`: Date and time of transaction
- `UnitPrice`: Price per item
- `CustomerID`: Customer ID (nullable)
- `Country`: Customer’s location

---

## 📌 Business Requirements

1. **Understand Sales Trends Over Time**  
   Gain insights into how sales evolve daily, weekly, and monthly to better forecast inventory and marketing needs.

2. **Segment Customers by Purchasing Behavior**  
   Identify customer segments based on frequency, recency, and monetary value to inform loyalty strategies.

3. **Identify Top-Performing Products**  
   Recognize which products contribute most to revenue for better stock prioritisation and promotion.

4. **Geographic Sales Analysis**  
   Detect high-performing regions to optimise international targeting and shipping resources.

5. **Measure Impact of Returns on Revenue**  
   Evaluate how returns (invoices with negative quantities) affect overall profitability and customer satisfaction.

6. **Develop an Interactive Dashboard for Stakeholders**  
   Provide an accessible and dynamic visual interface for exploring business performance data.

---

## Data Cleaning Summary
This project focuses on preparing transaction data for accurate, consistent, and meaningful sales analysis. Below is a clear summary of the data cleaning steps followed.

### Data Cleaning Steps

1. Dropped Missing Product Descriptions:
Any rows without a product description were deleted, as these records were incomplete and not useful for analysis.

2. Filtered Out Adjustments and Negative Quantities:
Transactions marked as adjustments or with negative quantities (such as product returns) were excluded to focus only on valid sales transactions.

3. Removed Cancelled Orders:
Orders marked as cancelled (identified by invoice numbers containing 'C') were removed to retain only completed sales.

4. Removed Duplicates:
We performed an duplicate check using key columns such as:

- Invoice Number (InvoiceNo)
- Product Code (StockCode)
- Quantity
-Customer ID (CustomerID)
We did this to ensure no repeated transactions remained.

5. Excluded Missing or Unspecified Countries:
Transactions missing a country or marked as 'Unspecified' were dropped to maintain accurate geographic analysis.

### Caveats & Examples:
If a product description was missing, that row was removed (e.g., a transaction without a product name).

Cancelled orders (e.g., InvoiceNo like 'C12345') and returns (transactions with negative quantities) were excluded from the analysis.

Only transactions with a valid country were kept for geographic reporting.
