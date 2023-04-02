# Cyclistic SQL Documentation


Total rides in this dataset - 5,901,463
Rides after cleaning - 5,803,962

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

   

The top 10 longest rides
 ```sql
        SELECT 	started_at,
            ended_at,
            member_casual,
            ended_at - started_at AS ride_length
        FROM tripdata_21_08_22_07
        ORDER BY ride_length DESC
        LIMIT 10
```
```
    "started_at"	        "ended_at"	        "member_casual"	"ride_length"
    "2021-08-01 18:53:00"	"2021-08-30 16:42:00"	"casual"	"28 days 21:49:00"
    "2021-10-02 18:35:36"	"2021-10-31 01:00:37"	"casual"	"28 days 06:25:01"
    "2022-05-08 00:28:53"	"2022-06-02 04:46:41"	"casual"	"25 days 04:17:48"
    "2022-06-15 07:56:59"	"2022-07-10 04:57:37"	"casual"	"24 days 21:00:38"
    "2021-11-06 16:53:11"	"2021-12-01 00:10:54"	"casual"	"24 days 07:17:43"
    "2022-03-05 19:08:58"	"2022-03-29 15:43:02"	"casual"	"23 days 20:34:04"
    "2022-07-09 01:02:00"	"2022-08-01 19:11:00"	"casual"	"23 days 18:09:00"
    "2022-07-09 01:03:00"	"2022-08-01 19:11:00"	"casual"	"23 days 18:08:00"
    "2022-07-09 01:02:00"	"2022-08-01 18:51:00"	"casual"	"23 days 17:49:00"
    "2021-09-05 14:45:30"	"2021-09-28 10:24:02"	"casual"	"22 days 19:38:32"
```
   

The 10 shortest rides
```sql
        SELECT 	started_at,
            ended_at,
            member_casual,
            ended_at - started_at AS ride_length
        FROM tripdata_21_08_22_07
        ORDER BY ride_length
        LIMIT 10
```
```
    "started_at"	        "ended_at"	        "member_casual"	"ride_length"
    "2022-06-07 19:23:03"	"2022-06-07 17:05:38"	"casual"	"-02:17:25"
    "2022-06-07 19:15:39"	"2022-06-07 17:05:37"	"casual"	"-02:10:02"
    "2022-06-07 19:14:47"	"2022-06-07 17:05:42"	"member"	"-02:09:05"
    "2022-06-07 19:14:46"	"2022-06-07 17:07:45"	"casual"	"-02:07:01"
    "2022-06-07 19:11:33"	"2022-06-07 17:05:24"	"casual"	"-02:06:09"
    "2022-06-07 19:13:27"	"2022-06-07 17:07:57"	"casual"	"-02:05:30"
    "2022-06-07 19:06:49"	"2022-06-07 17:09:43"	"member"	"-01:57:06"
    "2022-06-07 18:47:01"	"2022-06-07 17:05:41"	"casual"	"-01:41:20"
    "2021-11-07 01:58:08"	"2021-11-07 01:00:06"	"casual"	"-00:58:02"
    "2021-11-07 01:56:51"	"2021-11-07 01:00:57"	"casual"	"-00:55:54"
```

There are 129 rides in which the end time is smaller than the start time
```sql
    SELECT 	COUNT(*)
    FROM tripdata_21_08_22_07
	WHERE ended_at < started_at
```
I deleted these occurances and assumed this was a bug within the app given that of the 5,901,463 rows only 129 of them met this condition
```sql
        DELETE
        FROM tripdata_21_08_22_07
	    WHERE ended_at < started_at
```
I also deleted rides that were shorter that 00:00:01. There were 17,527 rides that were less than a second in length
```sql
        DELETE
        FROM tripdata_21_08_22_07
	    WHERE ride_length < '00:00:01'
```
From the company's (Divey) [website](https://divvybikes.com/system-data): "The data has been processed to remove trips that are taken by staff as they service and inspect the system; and any trips that were below 60 seconds in length (potentially false starts or users trying to re-dock a bike to ensure it was secure)." 

With this in mind I deleted all rides shorter than 00:00:60 (74,879 in total)
```sql        
        DELETE
        FROM tripdata_21_08_22_07
        WHERE ride_length < '00:00:60'
```
From the company's (Divey) [website](https://help.divvybikes.com/hc/en-us/articles/360033484791-What-if-I-keep-a-bike-out-too-long-): "If you do not return a bike within a 24-hour period, you may be charged a lost or stolen bike fee of $250 (plus tax)" 

With this in mind I deleted all rides longer than 24 hours (4,966 in total)
```sql        
        DELETE
        FROM tripdata_21_08_22_07
        WHERE ride_length > '24:00:00'
```
Updated 10 shortest rides
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

Updated 10 longest rides
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
Creating a 'ride_length' column for constraining against
```sql   
    ALTER TABLE tripdata_21_08_22_07
    ADD COLUMN ride_length interval

    UPDATE tripdata_21_08_22_07
    SET ride_length = ended_at - started_at
```


