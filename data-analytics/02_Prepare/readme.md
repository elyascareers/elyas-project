# 02 Prepare

**Data source:** Divvy, the Chicago bike-share system. The data is public and contains no personal details such as rider names or addresses.

**Datasets** (see `Data_Raw`)
- Divvy_Trips_2019_Q1.csv (January to March 2019)
- Divvy_Trips_2020_Q1.csv (January to March 2020)

Two files were chosen so rider behaviour in the same quarter of two different years could be compared.

**Data structure:** The 2019 file has trip_id, start_time, end_time, station names and IDs, usertype, gender and birthyear. The 2020 file uses different names (ride_id, started_at, ended_at, rideable_type, member_casual) and adds latitude and longitude.

**ROCCC check**
- Reliable: collected directly by the bike-share operator
- Original: primary data, not taken from another summary
- Comprehensive: contains the main fields needed (ride times, stations, user type)
- Current: 2019 and 2020 data is recent enough for this analysis
- Cited: publicly available and can be properly referenced

**Limitations and privacy**
- The two files have different column names and some different fields.
- Gender and birthyear appear only in 2019; latitude and longitude only in 2020.
- No personal information about individual riders is present, so privacy risk is low.
