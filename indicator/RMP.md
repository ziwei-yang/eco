# Reserve Management Purchases (RMP) — Technical Overview & Data Engineering Guide

---

## 1. What is Reserve Management Purchases (RMP)?

**Reserve Management Purchases (RMP)** are **U.S. Federal Reserve open market operations** conducted by the **New York Fed Open Market Trading Desk**.  
They consist primarily of **secondary-market purchases of U.S. Treasury bills** and are authorized by the **Federal Open Market Committee (FOMC)**.

RMP formally **started on December 12, 2025**, following the December 10, 2025 operating policy statement.

Operationally, RMP appears as:
- **Treasury Bill Purchase operations**
- Executed on specific operation days
- Conducted in scheduled monthly “operation periods”

RMP is **not QE**. It is an operational balance-sheet tool.

---

## 2. What does RMP do?

RMP is designed to:

- **Maintain an ample level of reserves** in the banking system
- Offset reserve drains from:
  - Treasury issuance
  - Balance-sheet runoff
  - Currency growth
- Ensure **short-term money market rates** remain aligned with policy targets

Key characteristics:
- Predictable and scheduled
- Concentrated in **Treasury bills**
- Operational rather than signaling-based

---

## 3. Is RMP data real-time or incremental?

### Short answer
- ❌ **No true real-time stream**
- ✅ **Incremental, operation-level updates**

### How data is published
The New York Fed publishes RMP data in two stages:

1. **Operation announcement**
   - Posted at the start of each operation window
   - Includes maximum size, maturity range, and timing

2. **Operation results**
   - Posted **after the operation closes**
   - Includes accepted amounts and security details

There is **no WebSocket or push feed**. All consumption is **poll-based**.

---

## 4. Where to obtain RMP data (official sources)

### A. Human-readable reference page
**Treasury Securities Operational Details (NY Fed)**  
Contains:
- Monthly operation periods
- Planned RMP totals
- Links to schedules (PDF)
- Links to results (Excel)

URL:
https://www.newyorkfed.org/markets/domestic-market-operations/monetary-policy-implementation/treasury-securities/treasury-securities-operational-details

---

### B. Machine-readable operation-level results (primary source)

The NY Fed Markets site provides **operation-level results via an XLSX endpoint**.

#### Endpoint pattern
https://markets.newyorkfed.org/api/tsy/all/results/details/search.xlsx

#### Required query parameters
- startDate=MM/DD/YYYY
- endDate=MM/DD/YYYY
- securityType=treasury

#### Example
https://markets.newyorkfed.org/api/tsy/all/results/details/search.xlsx?startDate=12/12/2025&endDate=02/12/2026&securityType=treasury

This file contains **one row per operation**, including:
- Operation date
- Operation type
- Security type
- Maturity bucket
- Accepted amount

This is the **authoritative dataset** for building daily RMP charts.

---

## 5. Technical ingestion (incremental data strategy)

### Why incremental ingestion is required
- Data is published **after operations**
- Historical periods remain downloadable, but:
  - There is no single “RMP time series”
  - The Fed does not guarantee immutable historical snapshots

Therefore, treat the dataset as:
**Append-only with possible short-term revisions**

---

### Recommended polling strategy

| Parameter | Recommendation |
|--------|----------------|
| Frequency | Daily (or every operation day) |
| Lookback window | 7–14 days |
| Format | XLSX |
| Method | HTTP GET |
| Storage | Local persistent storage |

---

### Example: automated fetch (Python)

```
import requests
import pandas as pd
from io import BytesIO

url = (
  "https://markets.newyorkfed.org/api/tsy/all/results/details/search.xlsx"
  "?startDate=12/12/2025&endDate=02/12/2026&securityType=treasury"
)

response = requests.get(url, timeout=60)
response.raise_for_status()

df = pd.read_excel(BytesIO(response.content))
```

---

## 6. Identifying RMP operations in the dataset

RMP does **not** appear as a labeled column.  
You identify RMP operations using **operational characteristics**.

### Practical RMP filter
Filter rows where:
- Operation Type = **Bill Purchases**
- Security Type = **Treasury Bills**
- Operation Date ≥ **2025-12-12**

This matches the NY Fed’s RMP execution method.

---

## 7. Does incremental RMP data require local storage?

### Yes — local storage is strongly recommended.

Reasons:
- No official historical RMP time series
- Incremental publication model
- Needed for:
  - Daily charts
  - Cumulative totals
  - Backtesting and analytics
  - Reproducibility

---

## 8. Recommended local storage design

### Minimum fields to store
- operation_date
- operation_type
- security_type
- maturity_bucket
- settlement_date
- accepted_amount
- cusip (if present)
- source_url
- ingested_at

---

### Example: SQLite schema

```
CREATE TABLE IF NOT EXISTS rmp_operations (
  operation_date TEXT,
  operation_type TEXT,
  security_type TEXT,
  maturity_bucket TEXT,
  settlement_date TEXT,
  cusip TEXT,
  accepted_amount REAL,
  source_url TEXT,
  ingested_at TEXT,
  PRIMARY KEY (
    operation_date,
    operation_type,
    maturity_bucket,
    settlement_date,
    cusip
  )
);
```

---

## 9. Incremental ingestion algorithm (recommended)

1. Determine today
2. Set startDate = today - 14 days
3. Download XLSX results
4. Parse rows
5. Filter to bill purchases (RMP proxy)
6. Upsert into local storage
7. Build charts from stored data

This protects against:
- Late postings
- Minor revisions
- Business-day timing issues

---

## 10. Building a daily RMP chart (from stored data)

### Daily aggregation query
```
SELECT
  operation_date,
  SUM(accepted_amount) AS daily_rmp_usd
FROM rmp_operations
WHERE operation_type = 'Bill Purchases'
GROUP BY operation_date
ORDER BY operation_date;
```

### Visualization notes
- RMP is **not daily** — gaps are expected
- Spikes correspond to Fed operation days
- Typical operation size: **~$6–9B**

---

## 11. Summary

- **RMP** = Treasury bill purchases to maintain ample reserves
- **Start date** = December 12, 2025
- **Data model** = Incremental, operation-level
- **Access method** = NY Fed Markets XLSX endpoint
- **Storage needed** = Yes, for historical continuity
- **Best practice** = Poll + upsert + chart from local DB
