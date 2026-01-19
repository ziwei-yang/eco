# Quantitative Tightening (QT)

## What is it, what does it do

Quantitative Tightening (QT) refers to the Federal Reserve’s process of reducing the size of its balance sheet by allowing securities to mature without reinvesting the proceeds or by actively selling assets. The policy is the opposite of quantitative easing (QE) and is intended to decrease the amount of reserves in the banking system, tighten financial conditions and remove monetary accommodation.

## How does it work

Under QT, the Fed sets monthly caps for the amounts of Treasury securities and agency mortgage‑backed securities (MBS) that are allowed to run off. When securities mature, the Fed receives principal but does not reinvest those funds above the cap; the liabilities side of the balance sheet shrinks as reserve balances are reduced. QT can also involve outright sales of securities, though the Fed has not done this in recent rounds. The runoff schedule and caps are announced in advance in policy statements and FOMC minutes, allowing markets to anticipate the pace of balance sheet reduction.

## Where to obtain realtime data or incremental data only? Technical details

There is no real‑time feed for QT. The main data sources are:

- **Federal Reserve H.4.1 weekly release** – provides the consolidated balance sheet of the Federal Reserve System, including total assets, Treasury securities and MBS holdings. The release is published every Thursday.
- **FRED (Federal Reserve Economic Data)** – hosts time series for the Fed’s balance sheet (e.g. “WALCL” for total assets) and for System Open Market Account (SOMA) holdings by security type. You can query these series via the FRED API using your API key and dates.
- **New York Fed SOMA holdings data** – includes historical and current holdings by CUSIP and maturity and shows the reduction over time. This can be downloaded as CSV from the New York Fed’s website.

To programmatically retrieve incremental data, you can:

1. Use the FRED API (`https://api.stlouisfed.org/fred/series/observations`) with the appropriate series ID, `api_key`, `start_date` and `end_date` parameters to pull weekly observations of the Fed’s balance sheet. For example, `series_id=WALCL` returns total assets. Request data after your last stored date to append new observations.
2. Download the H.4.1 release each week and parse the “Factors Affecting Reserve Balances” table to extract securities holdings.
3. For detailed SOMA holdings, periodically download the CSV files from the New York Fed site and compare to previous snapshots.

## If it is incremental data only, does it need a local data storage for historical data?

Yes. To analyze the progression of QT over time you should store historical data locally. Since the H.4.1 and FRED series provide a snapshot at each point in time rather than a continuous feed, maintaining a local database (e.g. CSV files or an SQL database) allows you to append new data as it becomes available and to perform trend analysis, charting and back‑testing. Without local storage you would need to repeatedly query the entire historical series, which is less efficient.
