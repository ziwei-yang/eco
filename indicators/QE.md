# Quantitative Easing (QE)

## What is it, what does it do

Quantitative Easing (QE) is a monetary policy tool in which the Federal Reserve purchases large quantities of longer-term securities, such as Treasury bonds and agency mortgage‑backed securities, to inject liquidity into the financial system. QE aims to lower long-term interest rates, support financial market functioning, stimulate economic activity and achieve the Fed’s macroeconomic objectives, especially when the federal funds rate is at or near the zero lower bound.

## How does it work

During a QE program, the FOMC authorizes the New York Fed’s Open Market Trading Desk to buy specified amounts of securities over a certain period. Purchases are typically spread across different maturities and are announced in advance via operating policy statements and FOMC communications. These purchases increase the Fed’s balance sheet and deposit reserves into the banking system, raising reserve balances and lowering yields. QE also signals an accommodative policy stance.

## Where to obtain realtime data or incremental data only? Technical details

There is no real-time stream for QE operations; data becomes available as operations are executed and via regular balance sheet reports. Key data sources include:

- **Federal Reserve H.4.1 weekly release** – details total assets, Treasury securities and MBS holdings, which reflect cumulative QE purchases.
- **FRED** – provides time series such as “WALCL” (total assets) and separate series for Treasury and MBS holdings. The FRED API allows queries by date range for these series.
- **New York Fed’s purchase schedules and results** – when QE is active, the New York Fed publishes purchase schedules, including operation dates, maturities and sizes. After each operation, results are posted with amounts accepted. These are available on the New York Fed’s website under domestic market operations.
- **SOMA holdings files** – provide granular holdings by security CUSIP and maturity and are updated regularly.

To obtain incremental data programmatically:

1. Use the FRED API to query series like WALCL, TREAST, and MBS holdings with `start_date` set to your last stored date to append new observations.
2. Download the H.4.1 release weekly and parse the factors affecting reserve balances to extract securities holdings.
3. When QE purchase operations are scheduled, scrape or download the New York Fed’s operation details and results to capture operation-level data.

## If it is incremental data only, does it need a local data storage for historical data?

Yes. Maintaining a local database of QE-related series and operation details is important for analyzing the effects of QE over time. While FRED retains historical series, storing your own copy allows you to merge operation schedules, results and balance sheet data, perform custom analytics and avoid repetitive downloads of the entire history. Use a database or CSV files to append new observations as they are released.
