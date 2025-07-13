# Crawler Scripts

This folder contains the Jupyter notebooks used for crawling YES24 IT Mobile category data.

## Script History

### autocr1.ipynb
- **Period**: 2025.05.10 ~ 2025.05.14
- **Description**: Initial crawler implementation
- **File Format**: `ctgr_MMDD_HH.csv`, `subctgr_MMDD_HH.csv`
- **Notes**: Files later renamed to YYMMDD format for consistency
- **Status**: Deprecated (accidentally terminated kernel)

### autocr2.ipynb
- **Period**: 2025.05.14 ~ 2025.07.12
- **Description**: Same codebase
- **File Format**: `ctgr_MMDD_HH.csv`, `subctgr_MMDD_HH.csv`
- **Notes**: Includes period affected by YES24 service disruption (June 9-13). Files later renamed to YYMMDD format for consistency
- **Status**: Deprecated (server computer issues)

### autocr3.ipynb
- **Period**: 2025.07.13 ~ 2025.07.13
- **Description**: Same codebase
- **File Format**: `ctgr_MMDD_HH.csv`, `subctgr_MMDD_HH.csv`
- **Notes**: Functions not executed at 17:00, 17:15 due to server computer issues. Files later renamed to YYMMDD format for consistency
- **Status**: Deprecated (server computer issues)

### autocr4.ipynb
- **Period**: 2025.07.14 ~ Present
- **Description**: Improved stability and retry logic. Updated file naming with year prefix
- **File Format**: `ctgr_YYMMDD_HH.csv`, `subctgr_YYMMDD_HH.csv`
- **Status**: Active

## Functions

Each notebook contains identical crawler functions with built-in reliability features:

### `run_ctgr()`
- Crawls main category data from YES24 IT Mobile section
- Extracts category names, URLs, and product counts
- Uses Selenium WebDriver with headless Chrome
- Implements random delays (1-2 seconds) to avoid server overload
- **Retry Logic**: Automatically retries up to 3 times on failure with 60-second intervals (from autocr4.ipynb)
- **Chrome Stability**: Enhanced browser options to prevent crashes (from autocr4.ipynb)

### `run_subctgr()`
- Crawls hierarchical subcategory data
- Captures parent-child category relationships
- Longer delay intervals (1-3 seconds) for complex navigation
- Handles dynamic page elements with staleness detection
- **Retry Logic**: Automatically retries up to 3 times on failure with 60-second intervals (from autocr4.ipynb)
- **Chrome Stability**: Enhanced browser options to prevent crashes (from autocr4.ipynb)

## Scheduling

All scripts use the same scheduling pattern:
- **05:00**: Main categories (`run_ctgr`)
- **05:15**: Subcategories (`run_subctgr`)
- **17:00**: Main categories (`run_ctgr`)
- **17:15**: Subcategories (`run_subctgr`)

## Reliability Features

### Automatic Retry Logic
- **Max Retries**: 3 attempts per scheduled execution
- **Retry Interval**: 60 seconds between attempts
- **Smart Logging**: Distinguishes between retry attempts and final failures
- **Graceful Recovery**: Continues operation after temporary failures

### Chrome Browser Stability
Enhanced Chrome options to reduce crashes and improve reliability:
```
--disable-dev-shm-usage          # Prevents shared memory issues
--disable-extensions             # Reduces memory usage
--no-first-run                   # Skips first-run setup
--disable-default-apps           # Disables default applications
--disable-background-timer-throttling  # Improves performance
```

### Error Handling
- **Comprehensive Logging**: All errors logged to `scraper_log.txt` with timestamps
- **Error Classification**: Setup errors, retry attempts, and final failures clearly marked
- **Partial Success**: Individual category failures don't stop entire collection process

## Technical Notes

- **Browser**: Headless Chromium with optimized stability flags
- **Encoding**: UTF-8 with BOM for Korean text compatibility
- **Data Format**: CSV files with pandas DataFrame export
- **File Naming**: autocr1-3 files were renamed from MMDD to YYMMDD format for consistency
- **Service Disruptions**: Automatic retry logic handles most temporary issues

## Log Messages

- `✅ Save completed`: Successful data collection
- `⚠️ Error occurred (attempt X/3)`: Temporary failure, retry scheduled
- `❌ Error occurred (final attempt)`: Permanent failure after all retries
- `❌ Setup error`: Browser initialization failure

## Expected Outcomes

With the enhanced reliability features, data collection success rate should significantly improve:
- **Network Issues**: Automatically resolved through retry logic
- **Chrome Crashes**: Reduced frequency with stability options
- **Timeout Errors**: Better handled with improved browser configuration
- **Temporary Site Issues**: Overcome through patience and retries