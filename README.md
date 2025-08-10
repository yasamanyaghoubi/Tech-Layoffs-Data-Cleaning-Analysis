# Tech-Layoffs-Data-Cleaning-Analysis
This project demonstrates how to take a messy  dataset and transform it into a clean, analysis-ready form using PostgreSQL. It covers the entire process (removing duplicates, standardizing values, handling missing data, and filtering irrelevant records )followed by in-depth EDA to uncover trends, top impacted companies, etc. 

# Tech Layoffs Data Cleaning & EDA (PostgreSQL)

## 📌 Project Overview
This project focuses on **cleaning** and **analyzing** a global technology layoffs dataset using **PostgreSQL**.  
The dataset contained inconsistencies, missing values, and formatting issues, which were addressed before performing **Exploratory Data Analysis (EDA)** to derive actionable insights.

---

## 🛠️ Technologies Used
- **PostgreSQL** – Data storage, cleaning, and analysis.
- SQL features used:
  - `CTE` (Common Table Expressions)
  - `ROW_NUMBER()` for duplicate detection
  - `TRIM`, `LIKE`, and `ILIKE` for text cleaning
  - `CAST` / `ALTER COLUMN` for data type conversion
  - `EXTRACT` & `TO_CHAR` for date operations
  - Window functions for ranking and rolling totals

---

## 📂 Project Structure
├── SQL data cleaning project.sql # Step-by-step data cleaning queries

├── EDA_layoff.sql # EDA queries and insights

└── README.md # Project documentation



---

## 🧹 Data Cleaning Steps
1. **Removing Duplicates**  
   - Used `ROW_NUMBER()` with `PARTITION BY` to find exact duplicates.
   - Deleted duplicates to ensure data integrity.

2. **Standardizing Data**  
   - Trimmed extra spaces from text fields.
   - Unified variations in industry and country names (e.g., "Crypto Currency" → "Crypto").
   - Fixed inconsistent formatting in country names.

3. **Handling Missing Values**  
   - Converted placeholder "null" strings to SQL `NULL`.
   - Filled missing industry data by referencing other rows for the same company.
   - Removed rows where both `total_laid_off` and `percentage_laid_off` were missing.

4. **Column and Row Filtering**  
   - Removed irrelevant or incomplete records to maintain dataset reliability.

---

## 📊 Exploratory Data Analysis (EDA)
Key insights from the dataset:
- **Largest single layoff event:** 12,000 employees from one company.
- **Industries most affected:** Consumer, Retail, and Transportation sectors.
- **Countries with most layoffs:** United States ranks highest.
- **Yearly trend:** Layoffs spiked sharply in 2022 and 2023.
- **Funding stage analysis:** Late-stage and post-IPO companies saw the biggest layoffs.
- **Top companies per year:** Identified the top 5 companies with the most layoffs annually.
- **Rolling trends:** Tracked cumulative layoffs over time for long-term impact analysis.

---

## 📈 Example Queries
```sql
-- Find top 5 companies with the highest layoffs each year
WITH company_year AS (
    SELECT company,
           EXTRACT(YEAR FROM date) AS year,
           SUM(total_laid_off) AS total_laid_off
    FROM layoffs_staging
    WHERE total_laid_off IS NOT NULL
    GROUP BY company, year
),
company_year_rank AS (
    SELECT *,
           DENSE_RANK() OVER (PARTITION BY year ORDER BY total_laid_off DESC) AS ranking
    FROM company_year
)
SELECT *
FROM company_year_rank
WHERE ranking <= 5;
```
##🚀 How to Run
git clone https://github.com/yourusername/tech-layoffs-sql.git
cd tech-layoffs-sql

