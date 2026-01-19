# Reserve Management Purchases (RMP) — Technical Overview & Data Engineering Guide

## 1. What is Reserve Management Purchases (RMP)?

Reserve Management Purchases (RMP) are U.S. Federal Reserve open market operations carried out by the New York Fed’s Open Market Trading Desk. They involve regular secondary-market purchases of short-maturity U.S. Treasury bills and began on December 12, 2025.

## 2. What does RMP do?

- Maintains an ample level of bank reserves by adding reserves through predictable bill purchases.
- Offsets reserve drains from Treasury issuance, balance sheet runoff, and currency growth.
- Helps keep short-term money market rates aligned with the Fed’s policy target.

## 3. How does it work?

RMP is implemented as a series of scheduled Treasury bill purchase operations during defined monthly periods. The Desk announces the operation schedule (dates, maturities and maximum sizes) on the Treasury Securities Operational Details page and then conducts the operations on those dates. After each operation closes, results detailing the amount purchased are posted. RMP differs from quantitative easing (QE) because it is limited to T-bills and framed as a reserve-maintenance tool rather than macro stimulus.

## 4. Where to obtain real‑time or incremental data?

- **Human-readable page:** The New York Fed’s Treasury Securities Operational Details page lists each monthly operation period with links to schedules and results.
- **Machine-readable data:** The Markets site provides an Excel download of operation-level results via the endpoint `https://markets.newyorkfed.org/api/tsy/all/results/details/search.xlsx` with `startDate`, `endDate`, and `securityType=treasury` query parameters. Set `startDate=12/12/2025` and `endDate` to today to capture all RMP operations since inception.

## 5. Technical details to obtain incremental data

1. **Choose date range:** For incremental updates, poll the last 7‑14 days to capture new results and possible revisions.
2. **Download XLSX:** Use HTTP GET to the above endpoint with your date range. For example:
   `https://markets.newyorkfed.org/api/tsy/all/results/details/search.xlsx?startDate=12/12/2025&endDate=01/20/2026&securityType=treasury`
3. **Parse data:** Load the Excel file using a tool like pandas in Python. Each row corresponds to an operation.
4. **Filter for RMP:** Select rows where the operation type indicates bill purchases and the security type is Treasury bills.

## 6. Does incremental data require local storage?

Yes. The New York Fed provides results as snapshots for a requested date range; there is no unified historical time series. To build a continuous history (for charts and analysis), you should store each operation row locally (e.g., in a CSV or database) and update it with new operations. Use a primary key based on operation date, type, maturity bucket and settlement date to deduplicate rows.

## 7. Summary

RMP is an operational tool for maintaining ample reserves through regular bill purchases. Data is published incrementally after operations via the New York Fed’s operations results download. Analysts should retrieve new data periodically, filter for bill purchases, and store it locally to maintain a historical record.
