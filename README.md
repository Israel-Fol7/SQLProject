# Layoffs Dataset – Data Cleaning (SQL)

This project takes a raw, messy dataset of company layoffs and transforms it into a clean, standardized table ready for analysis, using MySQL.

## What the Project Does

- **Staging setup** – Creates staging tables (`layoffs_staging`, `layoffs_staging2`) so the raw source data stays untouched while cleaning happens on a working copy.
- **Duplicate removal** – Uses `ROW_NUMBER()` with `PARTITION BY` across key columns to identify true duplicate rows, then deletes them.
- **Standardizing values** – Trims whitespace from company names, consolidates inconsistent category labels (e.g. collapsing `Crypto`, `CryptoCurrency`, etc. into one `Crypto` value), and strips trailing punctuation from country names (e.g. `United States.` → `United States`).
- **Fixing data types** – Converts the `date` column from text to a proper `DATE` type using `STR_TO_DATE`, enabling time-based analysis.
- **Handling missing data** – Converts blank strings to proper `NULL` values, then fills missing `industry` values by matching other rows for the same company. Removes rows where both `total_laid_off` and `percentage_laid_off` are null, since they carry no usable information.
- **Final cleanup** – Drops helper columns (like `row_num`) once they're no longer needed, leaving a clean, deduplicated, well-typed table.

## Skills Demonstrated

- Window functions (`ROW_NUMBER`)
- Common Table Expressions (CTEs)
- Self-joins for data imputation
- `UPDATE` / `DELETE` with joins
- String manipulation (`TRIM`, `LIKE`)
- Data type conversion
- General data quality / ETL thinking

## Tech Stack

- MySQL
