# Analysis & Findings

## Satisfaction by Customer Profile

### **By Customer Type**

```sql
SELECT  `Customer Type`,
        COUNT(*) AS total_passengers,
        ROUND(SUM(CASE WHEN satisfaction = 'Satisfied' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS satisfied_pct
        ROUND(SUM(CASE WHEN satisfaction = 'Dissatisfied' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS dissatisfied_pct
FROM invistico_airline_analysis
GROUP BY `Customer Type`
ORDER BY satisfied_pct DESC;
```

### **By Type of Travel**

```sql
SELECT  `Type of Travel`,
        COUNT(*) AS total_passengers,
        ROUND(SUM(CASE WHEN satisfaction = 'Satisfied' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS satisfied_pct
        ROUND(SUM(CASE WHEN satisfaction = 'Dissatisfied' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS dissatisfied_pct
FROM invistico_airline_analysis
GROUP BY `Type of Travel`
ORDER BY satisfied_pct DESC;
```

### **By Class**

```sql
SELECT  Class,
        COUNT(*) AS total_passengers,
        ROUND(SUM(CASE WHEN satisfaction = 'Satisfied' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS satisfied_pct
        ROUND(SUM(CASE WHEN satisfaction = 'Dissatisfied' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS dissatisfied_pct
FROM invistico_airline_analysis
GROUP BY Class
ORDER BY satisfied_pct DESC;
```

### Service Rating Analysis

```sql
WITH service_rating AS(
        SELECT `Seat Comfort` AS service, ROUND(AVG(`Seat Comfort`), 2) AS avg_rating FROM invistico_airline_analysis
        UNION ALL
        SELECT `Departure/Arrival time convenient`, ROUND(AVG(`Departure/Arrival time convenient`), 2) FROM invistico_airline_analysis
        UNION ALL
        SELECT 'Food and Drink', ROUND(AVG(`Food and drink`), 2) FROM invistico_airline_analysis
        UNION ALL
        SELECT 'Gate Location', ROUND(AVG(`Gate location`), 2) FROM invistico_airline_analysis
        UNION ALL
        SELECT 'Inflight Wifi', ROUND(AVG(`Inflight wifi service`), 2) FROM invistico_airline_analysis
        UNION ALL
        SELECT 'Inflight Entertainment', ROUND(AVG(`Inflight entertainment`), 2) FROM invistico_airline_analysis
        UNION ALL
        SELECT 'Online Support', ROUND(AVG(`Online support`), 2) FROM invistico_airline_analysis
        UNION ALL
        SELECT 'Ease of Online Booking', ROUND(AVG(`Ease of Online booking`), 2) FROM invistico_airline_analysis
        UNION ALL
        SELECT 'Onboard Service', ROUND(AVG(`On-board service`), 2) FROM invistico_airline_analysis
        UNION ALL
        SELECT 'Leg Room', ROUND(AVG(`Leg room service`), 2) FROM invistico_airline_analysis
        UNION ALL
        SELECT 'Baggage Handling', ROUND(AVG(`Baggage handling`), 2) FROM invistico_airline_analysis
        UNION ALL
        SELECT 'Checkin Service', ROUND(AVG(`Checkin service`), 2) FROM invistico_airline_analysis
        UNION ALL
        SELECT 'Cleanliness', ROUND(AVG(`Cleanliness`), 2) FROM invistico_airline_analysis
        UNION ALL
        SELECT 'Online Boarding', ROUND(AVG(`Online boarding`), 2) FROM invistico_airline_analysis
)
SELECT  service, avg_rating,
        RANK() OVER(ORDER BY avg_rating) AS ranking
FROM invistico_airline_analysis
ORDER BY ranking;
```

I used UNION ALL to convert the service rating columns into rows 
so it is easier to compare and rank all services in one result.

### Deep Dive: Disloyal Customer Dissatisfaction

```sql
WITH service_ratings AS (
    SELECT 'Seat Comfort' AS service, ROUND(AVG(`Seat comfort`), 2) AS avg_rating FROM invistico_airline_analysis WHERE `Customer Type` = 'Disloyal Customer' AND satisfaction = 'Dissatisfied'
    UNION ALL
    SELECT 'Time Convenient', ROUND(AVG(`Departure/Arrival time convenient`), 2) FROM invistico_airline_analysis WHERE `Customer Type` = 'Disloyal Customer' AND satisfaction = 'Dissatisfied'
    UNION ALL
    SELECT 'Food and Drink', ROUND(AVG(`Food and drink`), 2) FROM invistico_airline_analysis WHERE `Customer Type` = 'Disloyal Customer' AND satisfaction = 'Dissatisfied'
    UNION ALL
    SELECT 'Gate Location', ROUND(AVG(`Gate location`), 2) FROM invistico_airline_analysis WHERE `Customer Type` = 'Disloyal Customer' AND satisfaction = 'Dissatisfied'
    UNION ALL
    SELECT 'Inflight Wifi', ROUND(AVG(`Inflight wifi service`), 2) FROM invistico_airline_analysis WHERE `Customer Type` = 'Disloyal Customer' AND satisfaction = 'Dissatisfied'
    UNION ALL
    SELECT 'Inflight Entertainment', ROUND(AVG(`Inflight entertainment`), 2) FROM invistico_airline_analysis WHERE `Customer Type` = 'Disloyal Customer' AND satisfaction = 'Dissatisfied'
    UNION ALL
    SELECT 'Online Support', ROUND(AVG(`Online support`), 2) FROM invistico_airline_analysis WHERE `Customer Type` = 'Disloyal Customer' AND satisfaction = 'Dissatisfied'
    UNION ALL
    SELECT 'Ease of Online Booking', ROUND(AVG(`Ease of Online booking`), 2) FROM invistico_airline_analysis WHERE `Customer Type` = 'Disloyal Customer' AND satisfaction = 'Dissatisfied'
    UNION ALL
    SELECT 'Onboard Service', ROUND(AVG(`On-board service`), 2) FROM invistico_airline_analysis WHERE `Customer Type` = 'Disloyal Customer' AND satisfaction = 'Dissatisfied'
    UNION ALL
    SELECT 'Leg Room', ROUND(AVG(`Leg room service`), 2) FROM invistico_airline_analysis WHERE `Customer Type` = 'Disloyal Customer' AND satisfaction = 'Dissatisfied'
    UNION ALL
    SELECT 'Baggage Handling', ROUND(AVG(`Baggage handling`), 2) FROM invistico_airline_analysis WHERE `Customer Type` = 'Disloyal Customer' AND satisfaction = 'Dissatisfied'
    UNION ALL
    SELECT 'Checkin Service', ROUND(AVG(`Checkin service`), 2) FROM invistico_airline_analysis WHERE `Customer Type` = 'Disloyal Customer' AND satisfaction = 'Dissatisfied'
    UNION ALL
    SELECT 'Cleanliness', ROUND(AVG(`Cleanliness`), 2) FROM invistico_airline_analysis WHERE `Customer Type` = 'Disloyal Customer' AND satisfaction = 'Dissatisfied'
    UNION ALL
    SELECT 'Online Boarding', ROUND(AVG(`Online boarding`), 2) FROM invistico_airline_analysis WHERE `Customer Type` = 'Disloyal Customer' AND satisfaction = 'Dissatisfied'
)
SELECT service, avg_rating,
       RANK() OVER(ORDER BY avg_rating DESC) AS ranking
FROM service_ratings
ORDER BY ranking;
```

### Deep Dive: Eco Class Dissatisfaction

```sql
WITH service_ratings AS (
    SELECT 'Seat Comfort' AS service, ROUND(AVG(`Seat comfort`), 2) AS avg_rating FROM invistico_airline_analysis WHERE Class = 'Eco' AND satisfaction = 'Dissatisfied'
    UNION ALL
    SELECT 'Time Convenient', ROUND(AVG(`Departure/Arrival time convenient`), 2) FROM invistico_airline_analysis WHERE Class = 'Eco' AND satisfaction = 'Dissatisfied'
    UNION ALL
    SELECT 'Food and Drink', ROUND(AVG(`Food and drink`), 2) FROM invistico_airline_analysis WHERE Class = 'Eco' AND satisfaction = 'Dissatisfied'
    UNION ALL
    SELECT 'Gate Location', ROUND(AVG(`Gate location`), 2) FROM invistico_airline_analysis WHERE Class = 'Eco' AND satisfaction = 'Dissatisfied'
    UNION ALL
    SELECT 'Inflight Wifi', ROUND(AVG(`Inflight wifi service`), 2) FROM invistico_airline_analysis WHERE Class = 'Eco' AND satisfaction = 'Dissatisfied'
    UNION ALL
    SELECT 'Inflight Entertainment', ROUND(AVG(`Inflight entertainment`), 2) FROM invistico_airline_analysis WHERE Class = 'Eco' AND satisfaction = 'Dissatisfied'
    UNION ALL
    SELECT 'Online Support', ROUND(AVG(`Online support`), 2) FROM invistico_airline_analysis WHERE Class = 'Eco' AND satisfaction = 'Dissatisfied'
    UNION ALL
    SELECT 'Ease of Online Booking', ROUND(AVG(`Ease of Online booking`), 2) FROM invistico_airline_analysis WHERE Class = 'Eco' AND satisfaction = 'Dissatisfied'
    UNION ALL
    SELECT 'Onboard Service', ROUND(AVG(`On-board service`), 2) FROM invistico_airline_analysis WHERE Class = 'Eco' AND satisfaction = 'Dissatisfied'
    UNION ALL
    SELECT 'Leg Room', ROUND(AVG(`Leg room service`), 2) FROM invistico_airline_analysis WHERE Class = 'Eco' AND satisfaction = 'Dissatisfied'
    UNION ALL
    SELECT 'Baggage Handling', ROUND(AVG(`Baggage handling`), 2) FROM invistico_airline_analysis WHERE Class = 'Eco' AND satisfaction = 'Dissatisfied'
    UNION ALL
    SELECT 'Checkin Service', ROUND(AVG(`Checkin service`), 2) FROM invistico_airline_analysis WHERE Class = 'Eco' AND satisfaction = 'Dissatisfied'
    UNION ALL
    SELECT 'Cleanliness', ROUND(AVG(`Cleanliness`), 2) FROM invistico_airline_analysis WHERE Class = 'Eco' AND satisfaction = 'Dissatisfied'
    UNION ALL
    SELECT 'Online Boarding', ROUND(AVG(`Online boarding`), 2) FROM invistico_airline_analysis WHERE Class = 'Eco' AND satisfaction = 'Dissatisfied'
)
SELECT service, avg_rating,
       RANK() OVER(ORDER BY avg_rating DESC) AS ranking
FROM service_ratings
ORDER BY ranking;
```

### Delay Analysis

```sql
SELECT  satisfaction,
		ROUND(AVG(`Departure Delay in Minutes`), 2) AS avg_departure_delay,
        ROUND(AVG(`Arrival Delay in Minutes`), 2) AS avg_arrival_delay
FROM invistico_airline_analysis
GROUP BY satisfaction;
```
---

# Data Cleaning

## Raw Dataset

```sql
SELECT *
FROM invistico_airline;
```

## 1. Removing Duplicates

```sql
WITH duplicates_cte AS
(SELECT *, ROW_NUMBERROW_NUMBER() OVER(PARTITION BY satisfaction, Gender, `Customer Type`, Age, 
           `Type of Travel`, Class, `Flight Distance`, `Seat comfort`,
           `Departure/Arrival time convenient`, `Food and drink`, 
           `Gate location`, `Inflight wifi service`, `Inflight entertainment`,
           `Online support`, `Ease of Online booking`, `On-board service`,
           `Leg room service`, `Baggage handling`, `Checkin service`,
           Cleanliness, `Online boarding`, `Departure Delay in Minutes`,
           `Arrival Delay in Minutes`) AS row_num
)
SELECT *
FROM dupicates_cte
WHERE row_num > 1;
```

## 2. Standardizing Data
```sql
SELECT DISTINCT satisfaction, gender, `Customer Type`, `Type of Travel`, class
FROM invistico_airline_clean;
```

```sql
UPDATE invistico_airline_clean
SET satisfaction = 'Satisfied'
WHERE satisfaction ='satisfied';
```

```sql
UPDATE invistico_airline_clean
SET satisfaction = 'Dissatisfied'
WHERE satisfaction ='dissatisfied';
```

```sql
UPDATE invistico_airline_clean
SET `Customer Type` = 'Disloyal Customer'
WHERE `Customer Type` ='disloyal Customer';
```

```sql
UPDATE invistico_airline_clean
SET `Type of Travel` = 'Business Travel'
WHERE `Type of Travel` ='Business travel';
```

## 3. NULL Values & Blanks

```sql
SELECT *
FROM invistico_airline_clean
WHERE satisfaction IS NULL
OR Gender IS NULL
OR `Customer Type` IS NULL
OR Age IS NULL
OR `Type of Travel` IS NULL
OR Class IS NULL
OR `Flight Distance` IS NULL
OR `Seat comfort` IS NULL
OR `Departure/Arrival time convenient` IS NULL
OR `Food and drink` IS NULL
OR `Gate location` IS NULL
OR `Inflight wifi service` IS NULL
OR `Inflight entertainment` IS NULL
OR `Online support` IS NULL
OR `Ease of Online booking` IS NULL
OR `On-board service` IS NULL
OR `Leg room service` IS NULL
OR `Baggage handling` IS NULL
OR `Checkin service` IS NULL
OR Cleanliness IS NULL
OR `Online boarding` IS NULL
OR `Departure Delay in Minutes` IS NULL
OR `Arrival Delay in Minutes` IS NULL;
```

```sql
SELECT *
FROM invistico_airline_clean
WHERE satisfaction = ''
OR Gender = ''
OR `Customer Type` = ''
OR `Type of Travel` = ''
OR Class = '';
```
