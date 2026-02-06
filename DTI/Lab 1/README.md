# Lab 1: Data Cleaning & Exploration

## 📖 Why Data Cleaning Matters

In the real world, data is messy. Financial datasets contain missing values, outliers, and inconsistencies. Before meaningful analysis, you must clean it.

**The Cost:** Poor data quality costs organisations millions annually on average.

**Your Role:** Finance professionals spend 60-80% of their time on data preparation. This lab teaches systematic techniques for real-world data quality issues.

---

## What You'll Learn

1. Inspect datasets to identify quality issues
2. Handle missing values using appropriate strategies
3. Detect outliers without losing critical information
4. Standardise data for consistency
5. Document decisions for audit trails

**Scenario:** Clean 10,000 financial transactions for quarterly reporting.

---

## Core Concepts

### Missing Values
Blank cells in your dataset. Handle by:
- **Critical fields** (amount, date) → Delete row
- **Non-critical fields** (merchant, category) → Fill with placeholder values

### Outliers
Unusually high/low values. In finance: **flag for investigation, don't delete**.

**Detection:** IQR (Interquartile Range) method flags values outside Q1-1.5×IQR to Q3+1.5×IQR.

### Standardisation
Ensure consistency: "North" vs "north" vs "NORTH" → all become "North"

---

## 📊 Getting Started

### Step 1: Download the Dataset

**Download from GitHub:**
1. Navigate to the dataset: **[financial_transactions.csv](data/financial_transactions.csv)**
2. Click the **Download** button (↓) in the top-right corner of the file preview

![Download button on GitHub](imgs/github_download.png)

3. **Save the file** to a local folder on your computer (e.g., `Documents/ACCA_Labs/Lab1/`)

---

### Step 2: Open in Excel

You have **two options** to work with the data:

#### **Option A: Copy/Paste (Quick Method)**

1. Open the downloaded `financial_transactions.csv` file (it will open in Excel or a text editor)
2. Select all data (`Ctrl + A`)
3. Copy (`Ctrl + C`)
4. Open a **new Excel workbook**
5. Paste into Sheet1 (`Ctrl + V`)
6. **Save as Excel Workbook** (`.xlsx`) format: `File > Save As > Excel Workbook`

---

#### **Option B: Import Data (Recommended)**

This method gives you more control and preserves data types correctly.

1. Open a **new Excel workbook**
2. Go to `Data` tab → `Get Data` → `From File` → `From Text/CSV`
3. Browse to your downloaded `financial_transactions.csv` file
4. In the preview window:
   - Check that columns look correct
   - Ensure `date` column is recognised as Date format
   - Ensure `amount` column is recognised as Number format
5. Click **Load**
6. **Save as Excel Workbook** (`.xlsx`) format: `File > Save As > Excel Workbook`
7. Name it: `Lab1_DataCleaning_[YourName].xlsx`

---

**✅ You're ready!** Proceed to the walkthrough below.

---

## 📋 Excel Walkthrough

### 1. Initial Inspection

**Open your dataset and get familiar with it:**
- How many rows? Use `Ctrl + Down` to jump to the last row
- Apply **AutoFilter**: `Data > Filter`
- Click through each column filter—notice anything unusual?

**Expected columns:** `transaction_id`, `date`, `amount`, `category`, `merchant_name`, `region`

---

### 2. Missing Value Analysis

#### A) Identify Missing Values

**Check each column for blanks:**
1. Click the filter dropdown on the **amount** column → Look for `(Blanks)`—how many?
2. Repeat for `date`, `category`, `merchant_name`, `region`

**Visual highlighting (optional):**
- Select all data (`Ctrl + A`)
- Go to `Home → Conditional Formatting → Highlight Cell Rules → Blanks`
- Choose a red fill

**You should find approximately:**
- 500 missing amounts
- 300 missing dates
- 800 missing categories
- 1,000 missing merchant names

---

#### B) Handle Missing Values

**Decision framework:**

| Field | Strategy | Rationale |
|-------|----------|-----------|
| **amount** | Delete row | Transaction without amount = unusable for financial analysis |
| **date** | Delete row | Essential for time-series analysis |
| **category** | Fill with "Uncategorised" | Non-critical; preserves transaction record |
| **merchant_name** | Fill with "Unknown Merchant" | Non-critical; preserves transaction record |
| **region** | Fill with most common value | Reasonable assumption for missing geographic data |

---

**Steps:**

**1. Missing "amount"—DELETE ROWS:**
- Filter `amount` column → Show only `(Blanks)`
- Select all visible rows (click row number, `Ctrl + Shift + Down`)
- Right-click → `Delete Row`
- Clear filter

**2. Missing "date"—DELETE ROWS:**
- Filter `date` column → Show only `(Blanks)`
- Delete visible rows (same method)
- Clear filter

**3. Missing "category"—FILL WITH "Uncategorised":**
- Filter `category` column → Show only `(Blanks)`
- Click first blank cell in the column
- Type `Uncategorised`
- Press `Ctrl + Enter` (fills all selected blanks)
- Clear filter

**4. Missing "merchant_name"—FILL WITH "Unknown Merchant":**
- Repeat the same process as category

**5. Missing "region"—FILL WITH MOST COMMON REGION:**
- First, identify the most common region:
  - Quick method: `Insert > PivotTable` → Drag `region` to Rows, transaction_id to Values (it should become count of transaction_id)
  - Or use `Data > Sort & Filter > Advanced` to count unique values
- Filter `region` → Show only `(Blanks)`
- Fill with the most common region (e.g., "North")
- Clear filter

---

### 3. Outlier Detection

**In finance, we investigate outliers—we don't delete them.** High-value transactions may be genuine.

---

#### A) Visualise Transaction Amounts

- Select the `amount` column
- Go to `Insert > Charts`
- Choose **Box and Whisker** (Excel 2016+) or **Histogram**
- Look for unusually high/low values

---

#### B) Calculate IQR (Interquartile Range)

Create a small reference table in empty cells (e.g., `K2:L7`):

| **Metric** | **Formula** |
|------------|-------------|
| Q1 | `=QUARTILE.INC(C:C,1)` |
| Q3 | `=QUARTILE.INC(C:C,3)` |
| IQR | `=L3-L2` |
| Lower Bound | `=L2-1.5*L4` |
| Upper Bound | `=L3+1.5*L4` |

*(Adjust column references if your `amount` column is not C)*

---

#### C) Flag Outliers (Do NOT Delete)

1. **Add a new column header:** `outlier_flag`
2. **In the first data row**, enter:
   ```excel
   =IF(OR([@amount]<$L$5,[@amount]>$L$6),1,0)
   ```
   *(If not using a Table, replace `[@amount]` with `C2` and adjust)*
3. **Copy the formula down** the entire column

**Result:** Rows with `1` are flagged as outliers for manual review.

---

#### D) Review Flagged Transactions

- Filter `outlier_flag` → Show only `1`
- Sort `amount` column from **highest to lowest**
- **Review the top 10 transactions:**
  - Do they look genuine (e.g., large supplier payment)?
  - Or suspicious (e.g., £999,999.99 test value)?

**Document your findings** in a separate sheet or comment box.

---

### 4. Data Standardisation

**Consistency is critical for accurate reporting.**

---

#### A) Fix Category Capitalisation

**Problem:** "office supplies" vs "Office Supplies" vs "OFFICE SUPPLIES"

**Solution using PROPER() function:**
1. Insert a new column next to `category` (call it `category_clean`)
2. Enter formula: `=PROPER(E2)` *(adjust column reference)*
3. Copy down the entire column
4. **Copy → Paste Values** to replace formulas with text
5. Delete the original `category` column
6. Rename `category_clean` to `category`

---

#### B) Fix Region Spelling Errors

**Common issues:** "Nrth", "Sth", "Est", "Wst", or lowercase "north"

**Use Find & Replace (`Ctrl + H`):**
- Find: `Nrth` → Replace: `North`
- Find: `Sth` → Replace: `South`
- Find: `Est` → Replace: `East`
- Find: `Wst` → Replace: `West`
- Find: `north` → Replace: `North` *(Match case OFF)*
- Repeat for "south", "east", "west"

---

#### C) Verify Standardisation

- Filter `category` → Confirm all values use **Proper Case**
- Filter `region` → Confirm only **"North", "South", "East", "West"** appear

---

### 5. Final Analysis & Documentation

#### A) Create Summary Tables

**DATA QUALITY SUMMARY**

Create this table in a new sheet or below your data:

| **Metric** | **Value** |
|------------|-----------|
| Original Row Count | 10,000 |
| Final Row Count | 9,217 |
| Rows Removed | 783 |
| % Data Retained | 92% |
| Transactions Flagged (Outliers) | 4,608 |

**Formulas:**
- `Final Row Count`: `=COUNTA(A:A)-1` *(minus header)*
- `Rows Removed`: `=10000-[Final Row Count]`
- `% Data Retained`: `=[Final Row Count]/10000`
- `Transactions Flagged`: `=COUNTIF([outlier_flag column],1)`

---

**TRANSACTION SUMMARY**

| **Metric** | **Value** |
|------------|-----------|
| Date Range (Start) | 01/01/2024 |
| Date Range (End) | 30/12/2024 |
| Total Transaction Value | £15,002,116.05 |
| Average Transaction | £1,627.66 |
| Median Transaction | £1,077.86 |

**Formulas:**
- `Date Range (Start)`: `=MIN([date column])`
- `Date Range (End)`: `=MAX([date column])`
- `Total Transaction Value`: `=SUM([amount column])`
- `Average Transaction`: `=AVERAGE([amount column])`
- `Median Transaction`: `=MEDIAN([amount column])`

*(Format currency cells as £ using `Ctrl + Shift + 4`)*

---

#### B) Create Visualisations

**Generate these three charts:**

**1. Transactions by Category (Bar Chart)**
- Data: Count of transactions per category
- Insert: `Insert > Bar Chart`
- Shows which categories dominate your data

**Example output:**

![Transactions by Category](imgs/transactions_by_category.png)

---

**2. Transactions Over Time (Line Chart)**
- Data: Count of transactions by month
- Insert: `Insert > Line Chart`
- Shows seasonal patterns or trends

**Example output:**

![Transactions Over Time](imgs/transactions_over_time.png)

---

**3. Regional Distribution (Pie Chart)**
- Data: Percentage of transactions by region
- Insert: `Insert > Pie Chart`
- Shows geographic distribution

**Example output:**

![Regional Distribution](imgs/regional_distribution.png)

---

**Tip:** Use PivotTables to aggregate data before charting:
- `Insert > PivotTable`
- Drag `category`/`date`/`region` to Rows
- Drag `transaction_id` to Values (Count)

---

#### C) Document Your Decisions

Create a **"Cleaning Log"** sheet with:

| **Issue** | **Action Taken** | **Rows Affected** | **Rationale** |
|-----------|------------------|-------------------|---------------|
| Missing amount | Deleted rows | 500 | Cannot analyse transactions without monetary value |
| Missing date | Deleted rows | 300 | Essential for time-series analysis |
| Missing category | Filled "Uncategorised" | 800 | Preserves transaction; allows later re-categorisation |
| Missing merchant | Filled "Unknown Merchant" | 1,000 | Non-critical field |
| Missing region | Filled with "North" | ~150 | Most common region; reasonable assumption |
| Outliers (amount) | Flagged for review | 4,608 | Finance rule: investigate, don't delete |
| Category inconsistency | Standardised to Proper Case | All rows | Ensures accurate grouping |
| Region typos | Corrected via Find & Replace | ~200 | Ensures accurate regional reporting |

**This audit trail is critical for professional practice.**

---

### 6. Save Your Work

**Export your cleaned dataset:**
1. `File > Save As`
2. Choose **CSV (Comma delimited)** format
3. Name it: `financial_transactions_CLEANED.csv`

**Save your full workbook** (with charts, tables, log):
- `File > Save As > Excel Workbook (.xlsx)`
- Name it: `Lab1_DataCleaning_[YourName].xlsx`

---

## ✅ What You'll Deliver

**Portfolio-ready artifacts:**

✅ Completed workbook with documented cleaning steps  
✅ Cleaned dataset (`financial_transactions_CLEANED.csv`)  
✅ Summary tables (Data Quality + Transaction Summary)  
✅ Three visualisations (category, time, region)  
✅ Cleaning Log documenting all decisions

**These demonstrate your ability to solve real business problems with data.**

---

## Success Criteria

Your cleaned dataset should have:
- Zero missing values in critical fields
- Standardised categorical data
- Outliers flagged (not deleted)
- Full documentation
- Summary metrics

**Time:** 60-90 minutes

---

## Key Takeaway

> **"Clean data is the foundation of reliable analysis."**

In finance, you **investigate outliers, not delete them**. Documentation is as important as the cleaning itself.

**Ready?** Start the lab above! 🚀

---

## 📥 Need a Reference Solution?

If you'd like to compare your work or need a completed example for reference:

**[Download the completed workbook: Data_Cleaning_Excel.xlsx](https://github.com/OlaYounis/ACCA/raw/refs/heads/main/DTI/Lab%201/Data_Cleaning_Excel.xlsx)**

This file includes:
- All cleaning steps completed
- Summary tables (Data Quality + Transaction Summary)
- All three visualisations
- Cleaning Log with documented decisions
- Cleaned dataset ready for Lab 2

**Note:** Try to complete the lab yourself first—hands-on practice is essential for building real data skills!
