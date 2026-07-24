# Layoffs Dataset — Data Cleaning (SQL)

A SQL project that takes a raw, messy dataset of global company layoffs and transforms it into a clean, standardized, analysis-ready table and using MySQL.

![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat&logo=mysql&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-CC2927?style=flat)
![Data Cleaning](https://img.shields.io/badge/Data%20Cleaning-informational)

---

## 📊 Project Overview

Raw datasets almost never arrive analysis-ready, they contain duplicate records, inconsistent text formatting, blank/null values, and incorrect data types. This project works through a real-world layoffs dataset step by step, applying standard data cleaning practices so the table can be trusted for downstream analysis or reporting (e.g. in a BI tool like Power BI or Tableau).

## 🎯 Business Problem

A layoffs dataset pulled from public reporting is only useful if it's reliable. Duplicate rows inflate counts, inconsistent category names (e.g. `Crypto` vs `CryptoCurrency`) fragment what should be a single group, and free-text dates block any time-based analysis. Before anyone can answer real questions, "Which industries were hit hardest?", "How have layoffs trended over time?", the data has to be cleaned and standardized first. That's the problem this project solves.

## 🧹 Cleaning Process

| Step | What It Does |
|---|---|
| **Staging setup** | Creates staging tables (`layoffs_staging`, `layoffs_staging2`) so the raw source data is never modified directly and all cleaning happens on a working copy |
| **Duplicate removal** | Uses `ROW_NUMBER()` with `PARTITION BY` across key columns to identify true duplicate rows, then deletes them |
| **Standardizing text** | Trims whitespace from company names; consolidates inconsistent category labels (e.g. collapsing `Crypto`, `CryptoCurrency`, etc. into one `Crypto` value); strips trailing punctuation from country names (e.g. `United States.` → `United States`) |
| **Fixing data types** | Converts the `date` column from text to a proper `DATE` type using `STR_TO_DATE`, enabling time-based analysis and sorting |
| **Handling missing data** | Converts blank strings to proper `NULL` values, then fills missing `industry` values by self-joining on `company` to borrow values from other rows for the same company |
| **Removing unusable rows** | Deletes rows where both `total_laid_off` and `percentage_laid_off` are null, since they carry no analyzable information |
| **Final cleanup** | Drops helper columns (like `row_num`) once they're no longer needed, leaving a clean, deduplicated, well-typed table ready for use |

## 🛠️ Tools & Techniques

- **MySQL** — all cleaning performed with standard SQL
- **Window functions** — `ROW_NUMBER()` for duplicate detection
- **CTEs (Common Table Expressions)** — structuring multi-step logic clearly
- **Self-joins** — filling missing values by referencing other rows in the same table
- **`UPDATE`/`DELETE` with joins** — correcting and removing data based on related rows
- **String functions** — `TRIM()`, `LIKE` for text standardization
- **Data type conversion** — `STR_TO_DATE`, `ALTER TABLE ... MODIFY COLUMN`

## 💡 Key Skills Demonstrated

- Data cleaning and standardization on a real-world, messy dataset
- Identifying and removing duplicates without a unique ID column, using window functions
- Handling missing data thoughtfully (imputation via self-join, not just deletion)
- Writing multi-step SQL logic using CTEs rather than one-off queries
- Preserving raw source data via a staging-table workflow — a practice used in production data pipelines

## 📁 Repository Contents

```
├── SQL_project.sql   # Full data cleaning script, step by step
└── README.md         # Project documentation
```

## ▶️ How to Run This Project

1. Load the raw `layoffs` dataset into a MySQL database
2. Run `SQL_project.sql` from top to bottom, each section builds on the last (staging → deduplication → standardization → missing data → final cleanup)
3. The result is a clean table (`layoffs_staging2`) ready for analysis or to feed into a BI tool

## 🔭 Possible Next Steps

- Add exploratory data analysis (EDA) queries on top of the cleaned table (e.g. layoffs by industry/year, top companies)
- Feed the cleaned table into a Power BI or Tableau dashboard for visualization
- Add data validation checks (e.g. row counts before/after cleaning) to document the impact of each step

---

*Part of a personal data analytics portfolio, see also: [Data Professional Survey Dashboard (Power BI)](../PowerBIProject) for the dashboarding side of this skill set.*
- MySQL
