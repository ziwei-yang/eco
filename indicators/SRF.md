# Standing Repo Facility (SRF)

## What is it?
The Federal Reserve's Standing Repo Facility (SRF) is a standing (always available) open market facility that offers overnight repurchase agreements (repos) to primary dealers and eligible banks. It was launched in July 2021 to provide liquidity backstop in the U.S. Treasury repo market.

## What does it do?
The SRF serves as a safety valve for money markets. By offering overnight cash loans secured by U.S. Treasury, agency debt, and agency mortgage‑backed securities, it helps cap repo rates and ensure that short‑term interest rates stay within the target range set by the FOMC. It provides liquidity on demand when markets are stressed.

## How does it work?
The facility operates daily at a fixed rate (the SRF rate) with a set maximum amount per counterparty. Participants post eligible securities as collateral and receive cash overnight. If they do not repay, the Fed keeps the collateral. Because it is a standing facility, banks and dealers do not need to wait for scheduled operations; they can request funds any day through the facility.

## Where to obtain data (realtime or incremental)?
SRF usage data is published by the New York Fed and the Federal Reserve Board:
- The Fed's weekly **H.4.1** statistical release includes aggregate outstanding repo operations under 'repurchase agreements'. This series is available on FRED (`WORAL`) and is updated weekly.
- The New York Fed's **Repo Operations** page publishes details of each repo operation, including the offered and accepted amounts. You can retrieve historical operations using the New York Fed Markets API (similar to the Treasury operations API) by specifying `operationType=repo`.
- There is no push feed; you must poll the data periodically to get new operations. The New York Fed may also provide XLSX downloads of repo operation results.

### Technical details
To download repo operation results programmatically, call the NY Fed Markets API endpoint with appropriate date parameters. For example:

```
https://markets.newyorkfed.org/api/repo/all/results/details/search.xlsx?startDate=01/01/2025&endDate=01/20/2026
```

Replace the dates with the desired range. Load the XLSX file using a library such as pandas. Filter rows where `operationType` equals "Standing Repo Facility" if available. Because the API may not distinguish SRF from other repo operations explicitly, you may need to identify SRF operations by date (after July 2021) and collateral type.

## Do you need local storage?
Since the NY Fed only provides results for a chosen date range, maintaining a local database or CSV is recommended if you need a full historical series. Poll the API daily or weekly, appending new rows to your store. For weekly aggregated totals, FRED's `WORAL` series can be queried directly and does not require local storage.
