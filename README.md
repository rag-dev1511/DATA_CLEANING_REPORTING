# Data Cleaning & Reporting Automation

Automated Python (pandas) workflow that cleans a messy e-commerce dataset and generates a report with charts, built in Google Colab.

## Problem Statement
Raw data is rarely ready for analysis. This project automates data cleaning (missing values, duplicates, inconsistent formats) and produces reports and visual summaries from the cleaned data.

## Dataset
- **Name:** SHEIN Jewelry & Accessories product listings (US store)
- **Source:** Kaggle, "Dirty E-Commerce Data [80,000+ Products]" (one category file)
- **Type:** Product listing data (not individual customer transactions)
- **Size:** 3,547 rows × 8 columns
- **File:** `data/data_cleaning_reporting.csv` (original, unmodified)

## Data Quality Issues Found
| Issue | Details |
|---|---|
| Duplicate rows | 180 exact duplicates |
| Missing values | 15,149 blank cells (mostly rank, "sold recently" and discount columns) |
| Split columns | Product title spread across two columns; URL present for only 39 rows |
| Wrong data types | Price (`$6.40`), discount (`-10%`) and "sold recently" (`7.2k+ sold recently`) stored as text |
| Inconsistent text | Rank labels mixed `#5 Best Seller` and `#5 Best Sellers`; categories prefixed with `in ` |

## Cleaning Steps
1. Renamed columns to clear, consistent names.
2. Merged the two title columns into one `title` column.
3. Trimmed whitespace and converted blank strings to missing values.
4. Converted price and discount to numbers (`price_usd`, `discount_pct`).
5. Extracted the numeric rank from the inconsistent rank text (`best_seller_rank`).
6. Converted "7.2k+ sold recently" to a number (`units_sold_recently`; a lower-bound estimate).
7. Removed duplicates after standardizing.
8. Filled missing discounts with 0 (a blank discount means no discount); added `has_discount` and `is_best_seller` flags.
9. Left missing rank and "sold recently" values as missing, because a blank label doesn't mean zero sales.
10. Dropped rows missing the title or price.

## Results (Before vs After)
| Metric | Before | After |
|---|---|---|
| Rows | 3,547 | 3,367 |
| Duplicates removed | – | 180 |
| Missing cells | 15,149 | 6,783* |

*Remaining blanks are the product URL, rank and "sold recently" columns, left missing on purpose.

## Key Metrics
| Metric | Value |
|---|---|
| Average price | $2.78 |
| Median price | $2.00 |
| Items discounted | 54.6% |
| Average discount (discounted items) | 18.5% |
| Items marked best seller | 20.7% |

## Charts
Add your screenshots here:
- `output/hist_price.png`: price distribution
- `output/hist_discount.png`: discount distribution
- `output/bar_top_categories.png`: top best-seller categories
- `output/bar_discount_share.png`: discounted vs full-price items

## Outputs
- `output/cleaned_data.csv`: cleaned dataset
- `output/cleaned_report.xlsx`: Excel report with sheets Cleaned_Data, Summary_Stats, Key_Metrics, Cleaning_Log
- `output/*.png`: charts

## How to Run
1. Open `data_cleaning_reporting.ipynb` in Google Colab.
2. Run all cells (Runtime → Run all).
3. Upload the CSV when prompted in Step 1.
4. The report and charts are saved to `output/` and downloaded as `output.zip`.

To use another dataset, upload a different CSV, but update the column names in the cleaning function first.

## Tools
Python, pandas, NumPy, Matplotlib, Google Colab

## Limitations
- Cleaning code is specific to this dataset's columns.
- "Sold recently" values are lower-bound labels (e.g. "100+"), so they're estimates.
- No date column, so no time-trend analysis.
BY - RAG
