# Analysis & Findings

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
