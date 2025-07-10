# Online Retail Transaction Dashboard
The Online Retail Transaction Dashboard is a comprehensive data analytics solution designed to simplify customer behaviour analysis and sales trend exploration. Using the Online Retail dataset, we developed an interactive dashboard to support data-driven decision-making across areas such as product performance, customer segmentation, and geographic sales analysis.
---
## Dataset Overview
The dataset contains records of transactions made through an online retail platform between December 2010 and December 2011. It includes the following fields:
- **InvoiceNo:** Transaction ID
- **StockCode:** Product ID
- **Description:** Product name
- **Quantity:** Quantity sold (can be negative for returns)
- **InvoiceDate:** Date and time of transaction
- **UnitPrice:** Price per item
- **CustomerID:** Customer ID (nullable)
- **Country:** Customer’s location
---
## Business Objectives
- **Understand Sales Trends Over Time:**
  Analyse daily, weekly, and monthly sales patterns to improve inventory planning and marketing strategies.
- **Segment Customers by Purchasing Behaviour:**
  Identify customer segments based on frequency, recency, and monetary value to inform loyalty and retention strategies.
- **Identify Top-Performing Products:**
  Recognise the products that contribute the most to overall revenue to guide inventory and promotional efforts.
- **Geographic Sales Analysis:**
  Determine high-performing regions to optimise international marketing and shipping logistics.
- **Measure Impact of Returns on Revenue:**
  Assess how returns (transactions with negative quantities) influence overall profitability and customer satisfaction.
- **Develop an Interactive Dashboard for Stakeholders:**
  Provide an intuitive and interactive visual interface to help stakeholders explore business performance metrics.
---
## Hypotheses and Validation Approach
This section outlines the key hypotheses derived from our business objectives and user stories. Each hypothesis includes a corresponding plan for validation through data analysis and visualisation.
### Summary of Key Hypotheses
| ID  | Hypothesis |
| --- | ----------- |
| H1  | The United Kingdom accounts for the majority of total revenue. |
| H2  | A small group of products generates the majority of the revenue. |
| H3  | Return transactions (negative quantities) significantly impact monthly revenue. |
| H4  | High-frequency customers spend significantly more per transaction. |
### Hypotheses Mapped to User Stories
| User Story | Hypothesis | Validation Approach |
|------------|------------|---------------------|
| 1. Understand Sales Trends Over Time | Sales volume and revenue follow clear temporal patterns (daily, weekly, monthly). | Visualise sales over time (line charts) grouped by day, week, and month. Identify patterns in peaks and troughs. |
| 2. Segment Customers by Purchasing Behaviour | A small subset of customers contributes most of the revenue and purchases most frequently. | Conduct RFM (Recency, Frequency, Monetary) analysis. Visualise customer segments using bar charts or scatterplots. |
| 3. Identify Top-Performing Products | A small number of products account for most total revenue. | Rank products by total revenue (Quantity × UnitPrice). Visualise using Pareto or bar charts. |
| 4. Geographic Sales Analysis | Certain countries (e.g., UK, Netherlands) consistently outperform others in revenue and order count. | Aggregate sales by country. Visualise using maps or bar charts, with optional filters by product or date. |
| 5. Measure Impact of Returns on Revenue | Returns significantly affect monthly revenue trends. | Compare gross and net monthly revenue using line charts or stacked visuals. |
| 6. Develop an Interactive Dashboard for Stakeholders | Interactivity improves usability for non-technical stakeholders. | Add filters (date, country, product) and test with end users to ensure insights are accessible. |
## Data Cleaning Summary

This project focuses on preparing transaction data for accurate, consistent, and meaningful sales analysis. Below is a clear summary of the data cleaning steps followed.

---

### Data Cleaning Steps

- **Dropped Missing Product Descriptions:**  
  Any rows without a product description were deleted, as these records were incomplete and not useful for analysis.

- **Filtered Out Adjustments and Negative Quantities:**  
  Transactions marked as adjustments or with negative quantities (such as product returns) were excluded to focus only on valid sales transactions.

- **Removed Cancelled Orders:**  
  Orders marked as cancelled (identified by invoice numbers containing `'C'`) were removed to retain only completed sales.

- **Removed Duplicates:**  
  Duplicate checks were performed using key columns:
  - **Invoice Number** (`InvoiceNo`)
  - **Product Code** (`StockCode`)
  - **Quantity**
  - **Customer ID** (`CustomerID`)  
  This ensured that no repeated transactions remained.

- **Excluded Missing or Unspecified Countries:**  
  Transactions missing a country or marked as `'Unspecified'` were dropped to maintain accurate geographic analysis.

---

#### Caveats & Examples

- If a product description was missing, that row was removed (e.g., transactions without a product name).
- Cancelled orders (e.g., `InvoiceNo` like `'C12345'`) and returns (transactions with negative quantities) were excluded from the analysis.
- Only transactions with a valid country were kept for geographic reporting.

---

### Summary of Findings with the Raw Data and Cancellations

During our initial exploration of the raw Online Retail transactions dataset, we identified several extreme values where unusually high quantities of items were ordered. These cases likely represent accidental or erroneous orders, as they are often followed by prompt cancellations.

A key challenge in cleansing these records is that the `InvoiceNo` field does not directly link the original order to its corresponding return or cancellation invoice. This lack of linkage makes it difficult to automatically pair and remove both the original and cancelled transactions. Given the time constraints of this analysis, we chose to focus on removing these extreme cases using available data analysis techniques rather than attempting to fully reconcile all cancellations.

Other notable findings include the presence of high unit prices associated with certain entries. These typically involve:
- Amazon fees  
- Postage costs  
- Manually entered product descriptions  

Such special cases may require separate handling or exclusion depending on the specific analysis objectives.

---

### Additional Data Refinement Steps

In addition to the initial data cleaning, we applied further data refinement techniques to improve dataset quality, particularly for analysis within **Tableau**.

#### Key Actions:
- Excluded additional transactions identified as **cancellations** and **bad debt**.
- Removed invoices containing **alphabetic characters**, retaining only numeric invoice records.
- Applied a **Tableau calculation** to filter product descriptions:
  - Product descriptions without at least **two consecutive uppercase characters** were excluded.  
  These typically represent stock adjustments, write-offs, or internal records not relevant to sales analysis.
- Removed specific product descriptions identified as irrelevant for sales analysis:
  - `.COM POSTAGE`
  - `POSTAGE`
  - `AMAZON FEE`
  - `STORING STOCK IN AMAZON`
  - `SINGLE ITEM`
  - `EXPORT POSTAGE`
  - `CARRIAGE`

> **Note:**  
> The Tableau calculation for product description filtering was designed to retain only valid, meaningful product sales by identifying at least two consecutive uppercase letters in the product name.

---

### Feature Engineering Steps
The following additional fields were created to enhance the analysis:
1. **TotalPrice:**
   Calculates the total value of each transaction line.
   *Formula:* `TotalPrice = Quantity × UnitPrice`
2. **InvoiceMonth:**
   Extracts the month from `InvoiceDate` for monthly analysis.
   *Formula:* `InvoiceMonth = month(InvoiceDate)`
3. **InvoiceDayOfWeek:**
   Derives the day of the week from `InvoiceDate`.
   *Formula:* `InvoiceDayOfWeek = weekday(InvoiceDate)` (0 = Monday, 6 = Sunday)
4. **InvoiceWeekOfYear:**
   Extracts the week number of the year from `InvoiceDate`.
   *Formula:* `InvoiceWeekOfYear = week_number(InvoiceDate)`
5. **ReturnsFlag:**
   Identifies returns by checking if either `UnitPrice` or `Quantity` is negative.
   *Formula:* `ReturnsFlag = 1 if UnitPrice < 0 or Quantity < 0, else 0`
6. **Recency:**
   Calculates the number of days since each customer's most recent purchase relative to a defined reference date.
   *Formula:* `Recency = ReferenceDate – LastInvoiceDate (per CustomerID)`
7. **MonetaryValue:**
   Computes the total spending per customer.
   *Formula:* Sum of `TotalPrice` per CustomerID
8. **CustomerFrequency:**
   Counts the number of unique invoices per customer to assess engagement.
   *Formula:* Count of unique `InvoiceNo` per CustomerID
---
### Tools and Libraries Used
- **pandas:** For data manipulation and cleaning
- **datetime (via pandas):** For date and time operations
---
### Pseudocode
1. Load dataset into a pandas DataFrame.
2. Calculate `TotalPrice` (`Quantity` × `UnitPrice`).
3. Convert `InvoiceDate` to datetime format.
4. Extract `InvoiceMonth`, `InvoiceDayOfWeek`, and `InvoiceWeekOfYear`.
5. Generate `ReturnsFlag` (based on negative quantities or prices).
6. Calculate `Recency` per customer using a reference date.
7. Calculate `MonetaryValue` per customer.
8. Calculate `CustomerFrequency` (unique invoice count per customer).
9. Merge recency, monetary, and frequency metrics back into the main dataset.