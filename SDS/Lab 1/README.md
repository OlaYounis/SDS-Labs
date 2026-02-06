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
- **Critical fields** (amount) → Delete row
- **Non-critical fields** (merchant) → Fill with "Unknown"

### Outliers
Unusually high/low values. In finance: **flag for investigation, don't delete**.

**Detection:** IQR (Interquartile Range) method flags values outside Q1-1.5×IQR to Q3+1.5×IQR.

### Standardisation
Ensure consistency: "North" vs "north" vs "NORTH" → all become "North"

---



### 🐍 Lab 1: Python (Google Colab)
**Best for:** Technical data science skills  

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/OlaYounis/ACCA/blob/main/SDS/Lab%201/Data_Cleaning.ipynb)


## What You'll Deliver

✅ Completed notebook/workbook  
✅ Cleaned dataset (CSV)  
✅ Summary statistics  
✅ Documentation of decisions

**Portfolio-ready artifacts to demonstrate practical data skills.**

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

**Ready?** Choose your tool above and start! 🚀

---

## Next Steps

After Lab 1, continue to:
- **Lab 2:** Exploratory Data Analysis
- **Lab 3:** Data Visualisation
- **Lab 4:** Basic Automation

**Keep your cleaned dataset—you'll use it in Lab 2!**
