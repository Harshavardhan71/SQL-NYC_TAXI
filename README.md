# NYC Yellow Taxi Case Study

## Overview
This project involves analyzing NYC Yellow Taxi trip data using SQL. The dataset contains details about taxi trips, including pickup and dropoff locations, fare amounts, payment types, and trip durations.

## Database and Table Creation
The project starts by creating a database and a table to store the taxi trip data.

### Create Database
```sql
CREATE DATABASE taxiDB;
USE taxiDB;
```

### Create Table
```sql
CREATE TABLE TAXIDATA (
    VENDOR_ID            VARCHAR(50),
    PICKUP_DATETIME      DATETIME,  
    DROPOFF_DATETIME     DATETIME,  
    PASSENGER_COUNT      INT,
    TRIP_DISTANCE        DECIMAL(9, 6),
    PICKUP_LONGITUDE     DECIMAL(9, 6),
    PICKUP_LATITUDE      DECIMAL(9, 6),
    RATE_CODE            INT,
    STORE_AND_FWD_FLAG   VARCHAR(50),
    DROPOFF_LONGITUDE    DECIMAL(9, 6),
    DROPOFF_LATITUDE     DECIMAL(9, 6),
    PAYMENT_TYPE         VARCHAR(50),
    FARE_AMOUNT          DECIMAL(9, 6),
    EXTRA                DECIMAL(9, 6),
    MTA_TAX             DECIMAL(9, 6),
    TIP_AMOUNT           DECIMAL(9, 6),
    TOLLS_AMOUNT         DECIMAL(9, 6),
    TOTAL_AMOUNT         DECIMAL(9, 6),
    TRIP_TIME_IN_SECS    INT
);
```

## Data Insertion
The dataset is loaded into the `TAXIDATA` table using an `INSERT` statement that selects data from `dbo.taxi_trip_data`.

```sql
INSERT INTO TAXIDATA (
    VENDOR_ID, PICKUP_DATETIME, DROPOFF_DATETIME, PASSENGER_COUNT, TRIP_DISTANCE,
    PICKUP_LONGITUDE, PICKUP_LATITUDE, RATE_CODE, STORE_AND_FWD_FLAG, DROPOFF_LONGITUDE,
    DROPOFF_LATITUDE, PAYMENT_TYPE, FARE_AMOUNT, EXTRA, MTA_TAX, TIP_AMOUNT,
    TOLLS_AMOUNT, TOTAL_AMOUNT, TRIP_TIME_IN_SECS
)
SELECT
    VendorID,
    TRY_CAST(tpep_pickup_datetime AS DATETIME),
    TRY_CAST(tpep_dropoff_datetime AS DATETIME),
    passenger_count,
    trip_distance,
    pickup_longitude,
    pickup_latitude,
    RateCodeID,
    store_and_fwd_flag,
    dropoff_longitude,
    dropoff_latitude,
    payment_type,
    fare_amount,
    extra,
    mta_tax,
    tip_amount,
    tolls_amount,
    total_amount,
    trip_time  
FROM dbo.taxi_trip_data
WHERE TRY_CAST(tpep_pickup_datetime AS DATETIME) IS NOT NULL;
```

## Data Validation
Run a basic query to verify data loading.
```sql
SELECT * FROM TAXIDATA;
```

## Analytical Queries
The following queries answer key business questions about NYC taxi trips.

### 1. Total Number of Trips
```sql
SELECT COUNT(*) AS TOTAL_TRIPS FROM TAXIDATA;
```

### 2. Total Revenue Generated
```sql
SELECT SUM(TOTAL_AMOUNT) AS TOTAL_REVENUE FROM TAXIDATA;
```

### 3. Percentage of Revenue from Tolls
```sql
SELECT (SUM(TOLLS_AMOUNT) / SUM(TOTAL_AMOUNT))*100 AS TOLL_PCT FROM TAXIDATA;
```

### 4. Percentage of Revenue from Tips
```sql
SELECT (SUM(TIP_AMOUNT) / SUM(TOTAL_AMOUNT))*100 AS TIP_PCT FROM TAXIDATA;
```

### 5. Average Trip Amount
```sql
SELECT AVG(TOTAL_AMOUNT) AS AVERAGE_TRIP_AMOUNT FROM TAXIDATA;
```

### 6. Average Fare, Tip, and Tax by Payment Type
```sql
SELECT
    PAYMENT_TYPE,
    AVG(FARE_AMOUNT) AS AVERAGE_FARE,
    AVG(TIP_AMOUNT) AS AVERAGE_TIP,
    AVG(MTA_TAX) AS AVERAGE_TAX
FROM TAXIDATA
GROUP BY PAYMENT_TYPE;
```

### 7. Peak Revenue Hour
```sql
SELECT
    DATEPART(HOUR, PICKUP_DATETIME) AS HOUR,
    AVG(TOTAL_AMOUNT) AS AVERAGE_REVENUE
FROM
    TAXIDATA
GROUP BY
    DATEPART(HOUR, PICKUP_DATETIME)
ORDER BY
    AVERAGE_REVENUE DESC;
```

### 8. Average Trip Distance
```sql
SELECT AVG(TRIP_DISTANCE) AS AVERAGE_DISTANCE FROM TAXIDATA;
```

### 9. Unique Payment Types
```sql
SELECT DISTINCT PAYMENT_TYPE FROM TAXIDATA;
```





## Images
## NYC Taxi Trip Data Analysis
1. **[Taxi Trip Data](https://github.com/Harshavardhan71/SQL-NYC_TAXI/blob/main/results%20sql%20pic/1%20taxi%20trip%20data.png)**
2. **[Taxi Data Schema](https://github.com/Harshavardhan71/SQL-NYC_TAXI/blob/main/results%20sql%20pic/2%20taxi%20data.png)**
3. **[Query 1: Total Trips](https://github.com/Harshavardhan71/SQL-NYC_TAXI/blob/main/results%20sql%20pic/3%20query1.png)**
4. **[Query 2: Total Revenue](https://github.com/Harshavardhan71/SQL-NYC_TAXI/blob/main/results%20sql%20pic/4%20query2.png)**
5. **[Query 3: Average Trip Amount](https://github.com/Harshavardhan71/SQL-NYC_TAXI/blob/main/results%20sql%20pic/5%20query3.png)**

## Conclusion
This SQL case study provides valuable insights into NYC taxi trips, including revenue, trip distances, payment types, and peak revenue hours. By analyzing this data, we can better understand taxi usage trends and fare structures.

