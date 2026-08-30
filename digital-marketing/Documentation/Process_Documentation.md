# Customer Segmentation (RFM) — Process Documentation

*Standard project documentation: a record of what was done, and why, to produce the
analysis. The methodology behind the results in `Final_Output`.*

## Project overview
The goal was to help a marketing team spend its campaign budget more effectively by
segmenting customers with **RFM** (Recency, Frequency, Monetary) and checking which
segments actually respond to campaigns. Built end-to-end in **Power BI**.

## Tools
- **Power BI Desktop** — cleaning (Power Query), RFM scoring and segments (DAX), and all
  visuals. Results were independently cross-checked before publishing.

## Data source
`marketing_campaign.csv` — a public marketing dataset of **2,240 customers** (originally an
iFood/superstore customer-analysis case). Each row is one customer: demographics, two years
of spending by product category, purchases by channel, and whether they accepted each of
five past campaigns plus the latest one. The file is **tab-separated**.

## Data preparation and cleaning (what was done)
1. **Dropped constant columns** `Z_CostContact` and `Z_Revenue` (same value for everyone).
2. **Income** — set to numeric; removed one broken record (income of 666,666, a data-entry
   error) and filled ~24 missing values with the median (~50,000).
3. **Age** — derived as `2014 - Year_Birth`; removed three records with birth years in the
   1800s (impossible ages).
4. **Standardised marital status** — collapsed the many labels (and junk values like
   "YOLO"/"Absurd") into `Partner` / `Single`.
5. **Engineered features:** `Monetary` (sum of all six spend columns), `Frequency`
   (web + catalog + store purchases), `Children`, and `TotalAcceptedCmp`.
6. **Removed non-buyers** (Frequency = 0). Final clean dataset: **2,207 customers**.

## RFM scoring
- Split **Recency**, **Frequency**, and **Monetary** into quartiles and scored each 1&ndash;4
  (Recency reversed, so recent buyers score high). Quartile cut-points: Recency 24/49/74,
  Frequency 6/12/19, Monetary 69/398/1048.
- Summed the three scores (3&ndash;12) and mapped to named segments: **Champions (10&ndash;12),
  Loyal Customers (8&ndash;9), Potential Loyalists (6&ndash;7), At Risk (4&ndash;5), Hibernating (3)**.

## Analysis performed
Compared segments on size, average recency/frequency/spend, and **campaign response rate**;
also looked at acceptance rate per campaign, total spend by product category, and purchases
by channel.

## Assumptions and data-quality notes
- Missing incomes filled with the median — fine for segmentation, but income-based
  conclusions should be treated as approximate.
- "Age" assumes a 2014 snapshot (the data's last enrolment year).
- Quartile thresholds were computed once from this dataset; on new data they'd be
  recalculated.

## Validation
Row counts were tracked through cleaning (2,240 &rarr; 2,207) and every summary figure was
reproduced independently to confirm the Power BI results.

## Outputs produced
- Cleaned dataset and summary tables (`Data_Processed`).
- Dashboard visuals (`Final_Output/Charts`).
- Final report with findings and recommendations (`Final_Output`).
