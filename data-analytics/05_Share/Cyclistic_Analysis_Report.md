# Cyclistic Bike-Share Analysis

Google Data Analytics Capstone Project · Prepared by Elyas Zulqarnain

**Question:** How do annual members and casual riders use Cyclistic bikes differently?

## Phase 1: Ask
**Business task:** Understand how casual riders and annual members use Cyclistic bikes differently, to design a marketing strategy that converts casual riders into annual members.

**Stakeholders:** Lily Moreno (Director of Marketing), the Cyclistic marketing analytics team, the Cyclistic executive team.

**Guiding questions**
1. How does ride duration differ between casual riders and annual members?
2. How do they differ in the time of day rides start?
3. Which stations are most popular with each group?
4. How does usage by day of the week differ?

**Scope:** First quarter (January to March) of 2019 and 2020.

## Phase 2: Prepare
The data comes from Divvy, the Chicago bike-share system, and is publicly available. It contains no personal details about riders.

- Divvy_Trips_2019_Q1.csv and Divvy_Trips_2020_Q1.csv were used so the same quarter of two years could be compared.
- The 2019 file uses trip_id, start_time, usertype, gender and birthyear. The 2020 file uses ride_id, started_at, member_casual, and adds latitude and longitude.

**ROCCC:** Reliable (collected by the operator), Original (primary data), Comprehensive (ride times, stations, user type), Current (2019 and 2020), Cited (public and referenceable).

**Limitations:** different column names and fields between the files; gender and birthyear only in 2019; latitude and longitude only in 2020.

## Phase 3: Process
Cleaning was done in Power Query.

1. Imported each file as a separate query (Trips_2019_Q1 and Trips_2020_Q1).
2. 2019 data: renamed columns to the 2020 style (trip_id to ride_id, start_time to started_at, usertype to member_casual), removed tripduration, gender and birthyear, changed Subscriber to member and Customer to casual, and added a year column.
3. 2020 data: removed latitude and longitude and added a year column.
4. Combined the tables with Append Queries as New into Trips_Combined. A naming mismatch that caused null dates was fixed first.
5. Created ride_length with `Duration.TotalSeconds([ended_at] - [started_at])` and removed rides with zero or negative duration.

**Result:** Trips_Combined with about 791,746 rows.

## Phase 4: Analyze
**Measures (DAX)**
- `Total Rides = DISTINCTCOUNT(Trips_Combined[ride_id])`
- `Average Ride Length = AVERAGE(Trips_Combined[ride_length])`

**Findings**
1. **Ride duration:** casual riders have a higher average ride length than members.
2. **Time of day:** members are more active in morning and evening hours; casual riders are more spread across the day. This suggests members use the bikes for commuting.
3. **Popular stations:** the two groups use different stations.
4. **Day of the week:** members are more active on weekdays; casual riders are relatively more active on weekends.

## Phase 5: Share
A Power BI report page with: average ride length by rider type, total rides by start hour, top stations, total rides by day of week, and summary cards (about 792K rides, average ride length).

## Phase 6: Act
1. Highlight the value of membership for riders who take longer trips.
2. Target casual riders with weekend-focused promotions.
3. Use popular casual-rider stations for targeted membership information.
4. Consider membership options that feel flexible for weekend or leisure riders.

**Expected outcome:** a clearer way to reach casual riders with messages that match how they already use the service, which can help increase annual memberships.

## Summary
Casual riders take longer rides and are more active on weekends, while members show stronger weekday and commuting patterns. These differences can guide a marketing strategy aimed at converting casual riders into members.
