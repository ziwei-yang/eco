# Reinvestment Policy

## What is it?
Reinvestment refers to the Federal Reserve’s policy of reinvesting principal payments from its holdings of Treasury securities and agency mortgage‑backed securities (MBS) back into new securities instead of letting the balance sheet shrink. Reinvestment was used after the Global Financial Crisis to maintain the size of the Fed’s balance sheet.

## What does it do?
When the Fed reinvests maturing securities, it keeps its holdings—and therefore bank reserves—roughly constant. This prevents reserves from declining and helps maintain accommodative financial conditions. During periods of quantitative easing, reinvestment helps sustain the level of assets after active purchases stop.

## How does it work?
As Treasury or MBS holdings mature or prepay, the Federal Reserve Bank of New York’s Desk reinvests the proceeds back into new securities of similar type and maturity distribution according to FOMC directives. Schedules for Treasury reinvestments are published monthly in the Treasury Securities Operational Details. For agency MBS, reinvestment purchases follow a monthly cap and are conducted via TBA transactions.

## Where to obtain data?
Data on reinvestment operations is published by the New York Fed and the Federal Reserve Board:
- The **Treasury Securities Operational Details** page includes amounts for reinvestment purchases alongside reserve management purchases. It shows planned reinvestment amounts each month.
- The **System Open Market Account (SOMA) holdings** table (released weekly in the H.4.1 and in the New York Fed’s balance sheet statements) shows the levels of Treasury and MBS holdings, which reflect reinvestments.
- For agency MBS reinvestment operations, the New York Fed publishes operation schedules and results on its website.

### Technical details
Reinvestment data is not streamed in real time. To monitor reinvestment amounts:
1. Check the monthly Treasury securities schedule for the planned reinvestment amount.
2. For historical operations, download the results from the NY Fed’s operations details page or via the markets API as XLSX files (similar to the RMP operations).
3. For MBS, review the monthly agency MBS reinvestment purchase announcements and results.

## Do you need local storage?
If you need to track historical reinvestment operations beyond what is provided in summary tables, maintain a local dataset by downloading the results for each operation period and appending them. For general analysis of aggregate holdings, the H.4.1/FRED series provide complete historical data, so local storage is optional.
