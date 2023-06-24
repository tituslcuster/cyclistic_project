# Cyclistic Analysis

Total Rides (post clean) - 5,803,962

Member/Casual totals
   1. Member rides: 3,379,237
   2. Casual rides: 2,522,226

```sql
    SELECT COUNT(*)
    FROM tripdata_21_08_22_07
    WHERE member_casual = 'member';

    SELECT COUNT(*)
    FROM tripdata_21_08_22_07
    WHERE member_casual = 'casual';
```

The counts of rides by ride type are as follows:
```sql
        SELECT COUNT(rideable_type)
        FROM tripdata_21_08_22_07
        WHERE rideable_type = 'docked_bike';
```
 1. "docked_bike" - 226,728 rides
 2. "classic_bike" - 3,055,641 rides
 3. "electric_bike" - 2,619,094 rides




Members ride type breakdown
```sql
        SELECT COUNT(rideable_type)
            FROM tripdata_21_08_22_07
            WHERE rideable_type = 'docked_bike'
        AND member_casual = 'member';
```
 1. Member docked rides: 0
 2. Member classic rides: 1,922,749
 3. Member electric rides: 1,456,488

    
Casual ride type breakdown
```sql
        SELECT COUNT(rideable_type)
        FROM tripdata_21_08_22_07
        WHERE rideable_type = 'docked_bike'
            AND member_casual = 'casual';
```
 1. Casual docked rides: 226,728
 2. Casual classic rides: 1,132,892
 3. Casual electric rides: 1,162,606

   

Top 10 shortest rides
```sql
        SELECT 	started_at,
            ended_at,
            member_casual,
            ride_length
        FROM tripdata_21_08_22_07
        ORDER BY ride_length
        LIMIT 10
```

```
    "started_at"	        "ended_at"	        "member_casual"	"ride_length"
    "2021-08-23 19:28:00"	"2021-08-23 19:29:00"	"casual"	"00:01:00"
    "2021-08-18 04:05:00"	"2021-08-18 04:06:00"	"casual"	"00:01:00"
    "2021-08-23 11:48:00"	"2021-08-23 11:49:00"	"casual"	"00:01:00"
    "2021-08-02 12:07:00"	"2021-08-02 12:08:00"	"casual"	"00:01:00"
    "2021-08-22 19:17:00"	"2021-08-22 19:18:00"	"casual"	"00:01:00"
    "2021-08-19 08:01:00"	"2021-08-19 08:02:00"	"casual"	"00:01:00"
    "2021-08-17 15:25:00"	"2021-08-17 15:26:00"	"casual"	"00:01:00"
    "2021-08-21 13:48:00"	"2021-08-21 13:49:00"	"casual"	"00:01:00"
    "2021-08-10 19:07:00"	"2021-08-10 19:08:00"	"casual"	"00:01:00"
    "2021-08-24 16:29:00"	"2021-08-24 16:30:00"	"casual"	"00:01:00"
```

Top 10 longest rides
```sql
        SELECT 	started_at,
            ended_at,
            member_casual,
            ride_length
        FROM tripdata_21_08_22_07
        ORDER BY ride_length DESC
        LIMIT 10

```

```    
    "started_at"	        "ended_at"	        "member_casual"	 "ride_length"
    "2022-03-05 19:08:58"	"2022-03-29 15:43:02"	"casual"	"23 days 20:34:04"
    "2022-07-09 01:02:00"	"2022-08-01 19:11:00"	"casual"	"23 days 18:09:00"
    "2022-07-09 01:03:00"	"2022-08-01 19:11:00"	"casual"	"23 days 18:08:00"
    "2022-07-09 01:02:00"	"2022-08-01 18:51:00"	"casual"	"23 days 17:49:00"
    "2021-09-05 14:45:30"	"2021-09-28 10:24:02"	"casual"	"22 days 19:38:32"
    "2021-09-05 07:05:59"	"2021-09-27 21:42:29"	"casual"	"22 days 14:36:30"
    "2022-06-10 16:13:34"	"2022-07-03 04:16:32"	"casual"	"22 days 12:02:58"
    "2022-07-04 18:37:00"	"2022-07-27 00:32:00"	"casual"	"22 days 05:55:00"
    "2022-06-19 13:52:43"	"2022-07-11 04:23:21"	"casual"	"21 days 14:30:38"
    "2022-07-05 14:25:00"	"2022-07-27 04:31:00"	"casual"	"21 days 14:06:00"
```    