# Cyclistic Bike-Share — Process Documentation

*Standard project documentation: a record of what was done, and why, to produce the
analysis. This is the methodology behind the results in `Final_Output/`.*

## Project overview
The goal was to answer a single question for Cyclistic's marketing team: **how do annual
members and casual riders use the bikes differently?** The analysis used one quarter of
trip data from two years and was carried out end-to-end in **Power BI**.

## Tools
- **Power BI Desktop** — data cleaning and transformation (Power Query), the data model
  and calculations (DAX), and all visualizations.
- Results were independently cross-checked for accuracy before publishing.

## Data sources
| File | Period | Rows | Notes |
|---|---|---|---|
| `Divvy_Trips_2019_Q1.csv` | Jan–Mar 2019 | 365,069 | Older Divvy schema |
| `Divvy_Trips_2020_Q1.csv` | Jan–Mar 2020 | 426,887 | Newer Divvy schema |

The data is public, first-party bike-system data (Motivate International Inc.) and
contains no personally identifiable information.

## Data preparation and cleaning (what was done)
The two source files use different column layouts, so the bulk of the preparation was
reconciling them into one consistent table:

1. **Schema alignment** — the 2019 columns were renamed to match the 2020 naming
   (e.g. `trip_id` → `ride_id`, `start_time` → `started_at`,
   `from_station_name` → `start_station_name`).
2. **Label standardisation** — the 2019 rider values `Subscriber` and `Customer` were
   recoded to `member` and `casual` to match the 2020 convention.
3. **Column reduction** — fields that existed in only one file (bike ID, trip duration,
   gender, birth year, latitude/longitude, bike type) were removed, keeping a common set
   of eight columns.
4. **Append** — the two cleaned tables were stacked into one dataset of **791,956** rows.
5. **Calculated fields** — three fields were derived: `ride_length_min`
   (ride end minus start, in minutes), `day_of_week`, and `month`.
6. **Row filtering** — trips with a ride length of zero or negative, and trips starting
   at the internal test station **"HQ QR"**, were removed. This dropped **3,767** rows,
   leaving a final clean dataset of **788,189** trips.

## Analysis performed
Descriptive analysis was run on the clean dataset, comparing members and casual riders on:
- total number of rides and the split between the two groups;
- average and median ride length;
- number of rides by day of week;
- average ride length by day of week;
- number of rides by month;
- most-used start stations for casual riders.

## Assumptions and data-quality notes
- **Outliers:** a small number of trips have extreme ride lengths (one exceeds 100,000
  minutes), which inflate the casual average. Both the mean and the more robust **median**
  are reported so the "typical" trip is not misrepresented. Values were left in place so
  totals reconcile exactly with the source data.
- **Different years:** the two quarters are from different years (2019 vs 2020), so the
  comparison is directional rather than a like-for-like year-over-year study.
- **Season:** both files cover Q1 (winter in Chicago), when ridership is lower than in
  summer; a full-year dataset would give a more complete seasonal picture.
- **Privacy:** with no rider identifiers, it is not possible to tell whether casual
  riders are local or how many passes an individual bought.

## Validation
Row counts were checked at each stage (365,069 + 426,887 = 791,956 before cleaning;
788,189 after), and the summary figures were reproduced independently to confirm the
Power BI results were correct.

## Outputs produced
- Cleaned, merged dataset and summary tables (`Data_Processed/`).
- Dashboard visuals (`Final_Output/Charts/`).
- Final report with findings and recommendations (`Final_Output/Cyclistic_Report.md`).
