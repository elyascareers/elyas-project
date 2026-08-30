# Customer Segmentation (RFM) — Power BI Step-by-Step Tutorial

*How I built Project 2 (Digital Marketing) so I can rebuild it from scratch later.*

Everything — cleaning, the RFM scoring, the segments, and the charts — is done inside
**Power BI Desktop**. Plain steps, so future-me doesn't have to remember anything.

---

## 0. What we're doing

A retail/e-commerce company runs marketing campaigns but most people ignore them. The
marketing team wants to **spend the campaign budget on the customers most likely to buy**.

> **Business question:** How do we segment customers so marketing can target the right
> people and lift the campaign response rate?

The classic technique is **RFM**:
- **Recency** — how recently did they buy? (recent = better)
- **Frequency** — how often do they buy?
- **Monetary** — how much do they spend?

We score each customer 1&ndash;4 on R, F and M, add the scores, and bucket them into named
segments (Champions, Loyal, etc.). Then we check which segments actually respond to
campaigns.

**Data:** `marketing_campaign.csv` — 2,240 customers with their spending, purchases, and
whether they accepted past campaigns. **Tool:** Power BI Desktop.

---

## 1. Load the data

1. Open **Power BI Desktop** &rarr; `File > Save As` &rarr; `Customer_Segmentation.pbix` in
   `Final_Output`.
2. Home &rarr; **Get data** &rarr; **Text/CSV** &rarr; pick
   `Data_Raw\marketing_campaign.csv`.
3. **Important:** this file is **tab-separated**. In the import preview, set
   **Delimiter = Tab**. You should see ~29 clean columns. Click **Transform Data**.

---

## 2. Clean the data (Power Query)

**2a. Drop useless columns.** Select `Z_CostContact` and `Z_Revenue` (they're the same
value for everyone) &rarr; right-click &rarr; **Remove Columns**.

**2b. Fix Income.**
- Set `Income` type to **Whole Number** (blanks become null).
- Remove the one broken row: filter `Income` &rarr; **Number Filters > Less Than** &rarr;
  `200000` (there's a 666,666 typo that wrecks every average).
- Fill the ~24 blanks: select `Income` &rarr; **Transform > Replace Values** isn't ideal for
  a median, so instead go **Transform > Statistics** later, or simpler: right-click
  `Income` &rarr; **Replace Values**, replacing `null` with the median **50,000** (that's
  the dataset median; close enough for this project — note it in your write-up).

**2c. Remove impossible ages.** Add a custom column `Age`:
```m
= 2014 - [Year_Birth]
```
Then filter `Age` to **less than 120** (three rows have birth years in the 1800s).

**2d. Parse the date.** Set `Dt_Customer` type to **Date** (format is DD-MM-YYYY — pick the
locale or use `Date` with day-first).

---

## 3. Build the RFM building blocks (Power Query)

Add these **Custom Columns** (Add Column &rarr; Custom Column):

**Monetary** (total spent across all product types):
```m
= [MntWines]+[MntFruits]+[MntMeatProducts]+[MntFishProducts]+[MntSweetProducts]+[MntGoldProds]
```
**Frequency** (total real purchases — web + catalog + store):
```m
= [NumWebPurchases]+[NumCatalogPurchases]+[NumStorePurchases]
```
`Recency` already exists in the data (days since last purchase).

Filter `Frequency` &rarr; **greater than 0** (drop anyone who never bought).

Then **Home &rarr; Close & Apply**. You should have about **2,207 rows**.

---

## 4. Score R, F, M (1&ndash;4)

We split each measure into 4 groups using the quartile cut-points I calculated from the
data. Using fixed thresholds keeps this simple in Power BI (no percentile functions
needed). Add three **conditional columns** (Add Column &rarr; Conditional Column), or use the
DAX below.

**Cut-points (from the data):**
| Measure | 25% | 50% | 75% |
|---|---|---|---|
| Recency (days) | 24 | 49 | 74 |
| Frequency (purchases) | 6 | 12 | 19 |
| Monetary (spend) | 69 | 398 | 1048 |

**R_score** — recent buyers score high, so it's reversed:
```DAX
R_score =
VAR r = Customers[Recency]
RETURN SWITCH(TRUE(), r<=24,4, r<=49,3, r<=74,2, 1)
```
**F_score:**
```DAX
F_score =
VAR f = Customers[Frequency]
RETURN SWITCH(TRUE(), f<=6,1, f<=12,2, f<=19,3, 4)
```
**M_score:**
```DAX
M_score =
VAR m = Customers[Monetary]
RETURN SWITCH(TRUE(), m<=69,1, m<=398,2, m<=1048,3, 4)
```
*(Create these as **calculated columns**: Table tools &rarr; New column.)*

**RFM total:**
```DAX
RFM = Customers[R_score] + Customers[F_score] + Customers[M_score]
```

---

## 5. Turn RFM into named segments

Add one more calculated column:
```DAX
Segment =
VAR s = Customers[RFM]
RETURN SWITCH(TRUE(),
    s>=10,"Champions",
    s>=8,"Loyal Customers",
    s>=6,"Potential Loyalists",
    s>=4,"At Risk",
    "Hibernating")
```

To sort the segments logically (not alphabetically), add a helper column and set it as
`Segment`'s **Sort by column**:
```DAX
SegOrder = SWITCH(Customers[Segment],
    "Champions",1,"Loyal Customers",2,"Potential Loyalists",3,"At Risk",4,"Hibernating",5)
```

---

## 6. Measures (DAX)

```DAX
Customers = COUNTROWS(Customers)
```
```DAX
Response Rate = AVERAGE(Customers[Response])
```
*(format as Percentage)*
```DAX
Avg Spend = AVERAGE(Customers[Monetary])
```
```DAX
Total Spend = SUM(Customers[Monetary])
```

---

## 7. Build the visuals

**KPI cards (top):** Customers, Response Rate, Avg Spend.

**A. Customers per segment** — Clustered column
- Axis: `Segment` &middot; Values: `Customers`
- *(Expect: Loyal 632, Champions 533, Potential 498, At Risk 420, Hibernating 124)*

**B. Response rate by segment** — Clustered column (the money chart)
- Axis: `Segment` &middot; Values: `Response Rate`
- *(Expect: Champions ~30%, Loyal ~16%, Potential ~11%, At Risk ~4%, Hibernating ~1%)*

**C. Average spend by segment** — Clustered column
- Axis: `Segment` &middot; Values: `Avg Spend`
- *(Champions ~1,236 vs Hibernating ~40)*

**D. Acceptance rate by campaign** — build a small measure per campaign, or unpivot the
`AcceptedCmp1..5` columns in Power Query into `Campaign` / `Accepted`, then chart the
average. *(Cmp2 flops at ~1.4%; the rest ~6&ndash;7%; the latest campaign ~15%.)*

**E. Total spend by product category** — unpivot the six `Mnt...` columns into
`Category` / `Spend`, then a bar chart. *(Wine dominates.)*

**F. Purchases by channel** — a bar of the summed `NumWebPurchases`,
`NumCatalogPurchases`, `NumStorePurchases`. *(Store leads.)*

Add a **Segment slicer** so the whole page filters.

---

## 8. Self-check numbers

| Check | Value |
|---|---|
| Clean customers | ~2,207 |
| Overall response rate | ~15.1% |
| Champions | 533 (24.2%), 30% response, avg spend ~1,236 |
| Loyal Customers | 632 (28.6%), 16% response |
| At Risk | 420 (19%), 4% response |
| Hibernating | 124 (5.6%), 1% response |
| Top category | Wine (~675k) |
| Top channel | Store (~12,846 purchases) |

If your Power BI matches these, the build is correct.

> Note: I filled missing Income with the median and dropped a 666,666 typo and three
> impossible ages. In a stricter version I'd model Income more carefully, but for
> segmentation this is fine.

*The written-up findings and recommendations are in `Final_Output`.*
