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
## 🧪 Hypothesis and How to Validate?

This section outlines the key hypotheses derived from our business objectives and user stories. Each hypothesis is accompanied by a plan for validation through data analysis and visualisation.

---

### 🔍 Summary of Core Hypotheses

| **ID** | **Hypothesis** |
|--------|----------------|
| H1 | The United Kingdom accounts for the majority of total revenue. |
| H2 | A small group of products generates the majority of the revenue. |
| H3 | Return transactions (negative quantities) significantly impact monthly revenue. |
| H4 | High-frequency customers spend significantly more per transaction. |

---

### 🧪 Hypotheses Linked to User Stories

| **User Story** | **Hypothesis** | **How to Validate It** |
|----------------|----------------|-------------------------|
| **1. Understand Sales Trends Over Time** | Sales volume and revenue follow clear temporal patterns (daily, weekly, monthly). | Plot sales (`TotalPrice`) over time using line charts grouped by day, week, and month. Identify recurring peaks and dips. |
| **2. Segment Customers by Purchasing Behavior** | A small subset of customers contributes the most revenue and purchases most frequently. | Perform RFM (Recency, Frequency, Monetary) analysis and segment customers. Visualise segments using bar charts or scatterplots. |
| **3. Identify Top-Performing Products** | A small number of products account for the majority of total revenue. | Rank products by total revenue (`Quantity * UnitPrice`). Use Pareto or bar charts to display their cumulative impact. |
| **4. Geographic Sales Analysis** | Certain countries (e.g. UK, Netherlands) consistently outperform others in revenue and order count. | Aggregate sales by `Country`. Visualise with maps or horizontal bar charts, optionally filtered by product or date. |
| **5. Measure Impact of Returns on Revenue** | Returned items (negative quantities) significantly affect monthly revenue trends. | Compare gross vs. net revenue by month. Visualise with dual or stacked line charts to show impact of returns. |
| **6. Develop an Interactive Dashboard for Stakeholders** | Interactivity (e.g. filters, slicers) improves the accessibility and insights for non-technical users. | Include interactive elements (date, country, product filters). Validate through team testing or stakeholder feedback. |

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

### Summary of Findings with the Raw Data and Cancellations

During our initial exploration of the raw Online Retail transactions dataset, we identified several extreme values where unusually high quantities of items were ordered. These cases likely represent accidental or erroneous orders, as they are often followed by prompt cancellations.

A challenge in cleansing these records is that the `InvoiceNo` field does not directly link the original order to its corresponding return or cancellation invoice. This lack of linkage means it is not straightforward to automatically pair and remove both the original and cancelled transactions. Given the time constraints of this analysis, we have chosen to focus on removing these extreme cases using available data analysis techniques rather than attempting to fully reconcile all cancellations.

Other notable findings include the presence of high unit prices associated with certain entries. These are typically seen with transactions involving Amazon fees, postage costs, and some manually entered product descriptions. These special cases may require separate handling or exclusion, depending on the analysis objectives.

---

### Additional Data Refinement Steps

In addition to the initial data cleaning steps, we applied further data refinement techniques to improve dataset quality, particularly for analysis within Tableau.

Key steps included:

- **Excluded transactions identified as cancellations and bad debt.**
- **Removed invoices containing alphabetic characters** to retain only valid numeric invoice records.
- **Applied a Tableau calculation to filter product descriptions.**
  - Product descriptions without at least two consecutive uppercase characters were excluded. These entries generally represent stock adjustments, write-offs, or internal records not relevant to sales analysis.
- Additionally, we removed several specific product descriptions identified as irrelevant for the analysis, including:
  - `.COM POSTAGE`
  - `POSTAGE`
  - `AMAZON FEE`
  - `STORING STOCK IN AMAZON`
  - `SINGLE ITEM`
  - `EXPORT POSTAGE`
  - `CARRIAGE`

> *Note: The Tableau calculation used for product description filtering was based on identifying at least two consecutive uppercase letters to retain only valid, meaningful product sales.*


### Feature Engineering Steps:

#### 1. **TotalPrice**
- Multiply `Quantity` by `UnitPrice` for each transaction line to compute total value.  
- Formula: `TotalPrice = Quantity * UnitPrice`

#### 2. **InvoiceMonth**
- Extract the month from `InvoiceDate` for monthly analysis.  
- Formula: `InvoiceMonth = month(InvoiceDate)`

#### 3. **InvoiceDayOfWeek**
- Extract the day of the week from `InvoiceDate` (0 = Monday, 6 = Sunday).  
- Formula: `InvoiceDayOfWeek = weekday(InvoiceDate)`

#### 4. **InvoiceWeekOfYear**
- Extract the week number of the year from `InvoiceDate`.  
- Formula: `InvoiceWeekOfYear = week_number(InvoiceDate)`

#### 5. **ReturnsFlag**
- Identify returns by checking if `UnitPrice` or `Quantity` is negative.  
- Flag: `ReturnsFlag = 1` if `UnitPrice < 0` or `Quantity < 0`, else `0`.

#### 6. **Recency**
- Calculate the number of days since the customer's most recent purchase relative to a reference date.  
- Formula:  
  `Recency = ReferenceDate - LastInvoiceDate per CustomerID`

#### 7. **MonetaryValue**
- Total spending by customer across all transactions.  
- Formula:  
  `MonetaryValue = Sum of TotalPrice per CustomerID`

#### 8. **CustomerFrequency**
- Number of unique invoices per `CustomerID` to capture engagement level.  
- Formula:  
  `CustomerFrequency = count_unique(InvoiceNo) per CustomerID`

---

### 🧰 Python Tools / Libraries:
- `pandas` for data manipulation  
- `datetime` (via pandas) for date operations

---

### 🔑 Pseudocode:
```plaintext
1. Load dataset into pandas DataFrame.
2. Compute TotalPrice: Quantity * UnitPrice.
3. Convert InvoiceDate to datetime.
4. Extract InvoiceMonth, InvoiceDayOfWeek, InvoiceWeekOfYear from InvoiceDate.
5. Create ReturnsFlag: UnitPrice < 0 or Quantity < 0 → ReturnsFlag = 1 else 0.
6. Calculate Recency per CustomerID:
   - Determine last purchase date per CustomerID.
   - Subtract from reference date.
7. Compute MonetaryValue: Total spending per CustomerID.
8. Compute CustomerFrequency: Number of unique invoices per CustomerID.
9. Merge Recency, MonetaryValue, and CustomerFrequency back to main DataFrame.