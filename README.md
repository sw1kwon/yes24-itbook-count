# YES24 IT Mobile Category Data
- This repository contains daily snapshots of book category data crawled from [YES24 IT Mobile](https://www.yes24.com/Product/Category/Display/001001003).  
- The data includes product counts by category, collected every day at 05:00 and 17:00 (KST).
- The data includes product counts by category and subcategory, collected every day at 05:15 and 17:15 (KST).

## Repository Structure
- `YYMM/`: Monthly data folders organized by year and month
- `crawling_notebooks/`: Jupyter notebooks for data collection
- `scraper_log.txt`: Execution logs and error tracking

## Files
- `ctgr_YYMMDD_HH.csv`: Main categories with product counts
- `subctgr_YYMMDD_HH.csv`: Subcategories with hierarchical category structure and product counts

## Schedule
- Crawled twice daily using an automated scheduler
- All data is saved in UTF-8 with BOM format for Korean text compatibility