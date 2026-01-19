# Discount Window — Operational Guide  
## What is it?  
The Discount Window is the Federal Reserve’s traditional lending facility that provides short-term credit to eligible depository institutions. It offers liquidity when market funding is unavailable or expensive. The facility includes the primary, secondary, and seasonal credit programs.  

## What does it do?  
- Supplies liquidity to banks in times of stress, allowing them to meet short-term funding needs.  
- Helps to alleviate strains in the interbank market and support the smooth functioning of the payments system.  
- Acts as a safety valve, reducing the stigma of borrowing from the Fed through reforms and outreach.  

## How does it work?  
- Eligible depository institutions pledge acceptable collateral (such as Treasury securities, agency debt, mortgage-backed securities, or high-quality loans) to the Federal Reserve.  
- They may request overnight or term loans.  
- The interest rate charged is the discount rate (primary credit rate) set by the Federal Reserve Board, usually above the target federal funds rate.  
- Upon repayment, collateral is released; if not, the Fed may seize collateral.  

## Where to obtain real‑time or incremental data?  
Data on Discount Window borrowing is not available in real time. The Federal Reserve publishes aggregate usage data in two main sources:  
- **Federal Reserve H.4.1**: The weekly balance‑sheet release includes the outstanding amount of primary, secondary, and seasonal credit.  
- **Federal Reserve Statistical Release**: Some historical details are published with a two-year lag, showing daily borrowing amounts by borrower type.  
- **FRED**: Series such as `DISCBORR` (Borrowings from Federal Reserve Banks, All Commercial Banks) provide weekly totals.  

### Technical details  
- To fetch aggregated weekly borrowing data, use the FRED API:  
  `https://api.stlouisfed.org/fred/series/observations?series_id=DISCBORR&api_key=YOUR_API_KEY&file_type=json`  
- Replace `YOUR_API_KEY` with your FRED key.  
- Since data is weekly, polling once per week is sufficient. More granular historical data (with borrower-level detail) is released with a lag and must be downloaded manually from the Fed’s website.  

## Is local storage needed for historical data?  
Yes. Since the published data is incremental and historical releases may be revised or archived, storing observations locally allows you to maintain a continuous time series and conduct long-term analysis of Discount Window usage. 
