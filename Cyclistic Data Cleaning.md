# Cyclystic Data Cleaning

Total rides in this dataset - 5,901,463
Rides after cleaning - 5,803,962


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

Creating a 'ride_length' column for constraining against
```sql   
    ALTER TABLE tripdata_21_08_22_07
    ADD COLUMN ride_length interval

    UPDATE tripdata_21_08_22_07
    SET ride_length = ended_at - started_at
```
