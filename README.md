# M1FinanceHoldingsParser
Simple javascript to parse holdings on M1Finance into a CSV


All this does is grab the first table element on the page, read through the rows, and output a big string - but obviously use at your own risk. My purpose in sharing this is just for individual use within a single account and expressly not to support any type of automated scraping.

### Usage:

1. Go to desktop dashboard https://dashboard.m1finance.com/d/invest/holdings
2. Open Chrome developer tools > console
3. Paste into console and run
4. Copy and paste output CSV to Google Sheets
5. Click paste icon and select Split text to columns
