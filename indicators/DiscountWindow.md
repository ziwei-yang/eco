# Discount Window

## What is it?
The discount window is the Federal Reserve’s primary direct lending facility for depository institutions. Banks can borrow short-term funds directly from the Fed, usually overnight, by pledging collateral.

## What does it do?
It provides liquidity to banks to meet short-term funding needs, acting as a safety valve for the banking system. It helps relieve pressure in times of stress or unexpected outflows, supporting the stability of the payments system.

## How does it work?
Banks request a loan through their regional Federal Reserve Bank, pledging eligible collateral. The loan rate is the discount rate set by the Fed (three discount window programs: primary credit, secondary credit, seasonal credit). Loans are typically overnight but can be extended. Historically there has been a stigma to using the window, but reforms after 2023 aim to reduce stigma.

## Where to obtain data (real-time or incremental)?
Data on discount window usage is published in aggregate by the Fed:
- **H.4.1 weekly statistical release:** includes “primary credit”, “secondary credit”, and “seasonal credit” outstanding totals. These series are available on FRED (e.g., `DISCREDIT`, `DISCCRED`, `SEASNCREDIT`).
- **Federal Reserve annual reports** and special disclosures provide detailed information on discount window lending by type and collateral.
- There is no real-time feed; the data is updated weekly.

### Technical details
To obtain historical and current series programmatically, use FRED’s API. For example, the St. Louis Fed FRED API can return weekly data for primary credit outstanding:
```
https://api.stlouisfed.org/fred/series/observations?series_id=DISCOUNTWND&api_key=YOUR_API_KEY&file_type=json
```
Replace `series_id` with the relevant series and supply your FRED API key. Alternatively, download the data manually from the FRED page.

## Do you need local storage?
FRED provides full historical data for each series, so if you only need aggregated weekly totals, you can query FRED when needed. If you plan to analyze the data frequently or maintain enriched datasets (e.g., combined with other series), local storage may be beneficial but is not strictly necessary.
