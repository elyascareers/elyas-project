# 03 Process

Cleaning and combining were done in Power BI using Power Query.

1. Imported each CSV as its own query: `Trips_2019_Q1` and `Trips_2020_Q1`.
2. Renamed the 2019 columns to match 2020: `trip_id` to `ride_id`, `start_time` to `started_at`, `usertype` to `member_casual`.
3. Removed columns not available in both years (`tripduration`, `gender`, `birthyear`) and the latitude/longitude columns from 2020.
4. Standardised rider labels: Subscriber became `member`, Customer became `casual`.
5. Added a `year` column to each query.
6. Combined both queries with Append Queries as New, creating `Trips_Combined`.
7. Fixed a column naming mismatch that had caused null date values after the append.
8. Added `ride_length` = `Duration.TotalSeconds([ended_at] - [started_at])`.
9. Removed rides with zero or negative `ride_length`.

**Result:** about 791,746 rows in `Trips_Combined`.
