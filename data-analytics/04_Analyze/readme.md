# 04 Analyze

**DAX measures**
- `Total Rides = DISTINCTCOUNT(Trips_Combined[ride_id])`
- `Average Ride Length = AVERAGE(Trips_Combined[ride_length])`

**Comparisons (member vs casual)**
- Average ride length
- Rides by start hour
- Top 10 start stations
- Rides by day of week

**Findings**
- Casual riders take longer rides than members.
- Members ride mainly in the morning and evening, which fits commuting.
- Casual riders ride more on weekends.
- The most popular start stations differ between the two groups.
