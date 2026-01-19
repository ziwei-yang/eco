# Standing Repo Facility (SRF) — Operational Guide  
## What is it?  
The Standing Repo Facility (SRF) is a permanent lending facility created by the Federal Reserve to provide overnight repurchase agreements to primary dealers and eligible depository institutions. It offers liquidity by allowing participants to borrow cash from the Federal Reserve by pledging Treasury securities, agency debt, and agency mortgage‑backed securities as collateral.  

## What does it do?  
- Serves as a backstop to the overnight funding market, preventing spikes in repo rates.  
- Provides an elastic source of reserves by enabling institutions to convert high‑quality securities into cash.  
- Enhances control over short‑term interest rates by capping repo rates at the SRF offering rate.  

## How does it work?  
- Eligible counterparties can request overnight cash loans from the Fed at a fixed offering rate, typically set above the target range for the federal funds rate.  
- Transactions are secured by pledging Treasury securities, agency debt, or agency MBS as collateral, subject to haircuts.  
- The facility is standing, meaning it is available every business day without a pre‑announced schedule; demand‑driven.  
- Loans are settled same‑day and mature the next business day. Participants can roll over as needed.  

## Where to obtain real‑time or incremental data?  
There is no true real‑time transactional feed for SRF usage. Data availability is incremental:  
- **Federal Reserve H.4.1 release**: The Fed’s weekly balance‑sheet statement includes aggregate amounts outstanding under “repurchase agreements” and can show SRF usage.  
- **Federal Reserve Economic Data (FRED)**: Series such as `WORAL` (Repurchase Agreements: Federal Reserve) or similar provide weekly totals.  
- **New York Fed**: Occasional usage summaries may appear in statements or research posts but are not a continuous feed.  

### Technical details  
- To retrieve data from FRED, use the FRED API endpoint:  
  `https://api.stlouisfed.org/fred/series/observations?series_id=WORAL&api_key=YOUR_API_KEY&file_type=json`  
- Replace `YOUR_API_KEY` with a valid FRED API key. The response will include dates and values.  
- Since the data is weekly, poll once per week (e.g., after the Thursday H.4.1 release) to ingest new observations.  

## Is local storage needed for historical data?  
Yes. Because the data from FRED or H.4.1 provides incremental updates (weekly observations), maintaining a local database or time‑series file is recommended to build a continuous historical series. Store each observation with its date and value. This allows you to perform trend analysis and ensures you have a complete history even if earlier data is revised or removed from the API. 
