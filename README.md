#Data Cleaning SQL Script

This repository contains an SQL script (data_cleaning.sql) designed to clean and preprocess layoff data. The script focuses on removing duplicates, standardizing values, handling nulls, and restructuring the dataset for analysis.

📂 File Description

data_cleaning.sql
A step-by-step SQL script that:

Removes duplicates from the raw dataset.

Standardizes text fields (company names, industries, countries).

Handles null and blank values by either imputing or removing them.

Cleans date formats and converts them into proper SQL DATE type.

Drops unnecessary helper columns after processing.

🗂️ Database Tables Used

layoffs – Original dataset table containing raw layoff records.

layoffs_staging – Temporary staging table created as a copy of layoffs for duplicate detection.

layoffs_staging2 – Final cleaned dataset table with standardized and deduplicated records.

⚙️ Key Steps in the Script

Duplicate Removal

Uses ROW_NUMBER() to identify duplicates based on multiple columns.

Inserts unique rows into layoffs_staging2.

Deletes duplicate rows.

Standardization

Trims extra spaces from company names.

Normalizes industry labels (e.g., merging all variations of "Crypto").

Fixes inconsistent country names (United States% → United States).

Converts date field from text to SQL DATE format.

Null Value Handling

Sets empty strings in industry to NULL.

Uses self-join to fill missing industry values from other company records.

Removes rows where both total_laid_off and percentage_laid_off are NULL.

Final Cleanup

Drops the row_num helper column.

Produces a cleaned dataset in layoffs_staging2.

📊 Data Flow Diagram
erDiagram
    LAYOFFS {
        text company
        text location
        text industry
        int total_laid_off
        text percentage_laid_off
        text date
        text stage
        text country
        int funds_raised_millions
    }

    LAYOFFS_STAGING {
        text company
        text location
        text industry
        int total_laid_off
        text percentage_laid_off
        text date
        text stage
        text country
        int funds_raised_millions
        int row_num
    }

    LAYOFFS_STAGING2 {
        text company
        text location
        text industry
        int total_laid_off
        text percentage_laid_off
        date date
        text stage
        text country
        int funds_raised_millions
    }

    LAYOFFS ||--o{ LAYOFFS_STAGING : "copy data"
    LAYOFFS_STAGING ||--o{ LAYOFFS_STAGING2 : "deduplicate + clean"

✅ Output

A cleaned and standardized layoffs dataset stored in layoffs_staging2.

Ready for exploratory data analysis (EDA), visualization, or reporting.

🔧 Usage

Import your raw layoffs dataset into a table called layoffs.

Run the data_cleaning.sql script step by step in your SQL environment (MySQL recommended).

After execution, the cleaned dataset will be available in layoffs_staging2.

📌 Notes

The script assumes the raw dataset is loaded into a table named layoffs.

The database schema name (world_layoffs) may need adjustment depending on your environment.

Designed for MySQL 8+ (due to use of ROW_NUMBER() and CTEs).
