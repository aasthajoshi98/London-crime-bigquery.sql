# C&BD Assignment 1
# London-crime-bigquery.sql
SQL Queries exploring London crime dataset on Google BigQuery public datase

## Q1. Number of distinct boroughs.

``` sql
SELECT
distinct borough
FROM
  `bigquery-public-data.london_crime.crime_by_lsoa`
```
![image](https://user-images.githubusercontent.com/87647832/131226958-ea986e17-4070-4cb8-820d-e0b395ae0227.png)

## Q2. What are the number of codes per borough?

```sql
SELECT
  borough,
  COUNT(DISTINCT lsoa_code) AS n_codes
FROM
  `bigquery-public-data.london_crime.crime_by_lsoa`
GROUP BY
  borough
ORDER BY
  COUNT(DISTINCT lsoa_code) DESC;
```
![image](https://user-images.githubusercontent.com/87647832/131226979-ab841b89-ecd5-41fa-ac07-230c6f34276a.png)

## Q3. What is the total crime in london?

```sql
SELECT
  year,
  month,
  SUM(value) AS `total_crime`
FROM
  `bigquery-public-data.london_crime.crime_by_lsoa`
WHERE
  borough = 'City of London'
GROUP BY
  year,
  month;
```
![image](https://user-images.githubusercontent.com/87647832/131226993-ee1d31a5-55b7-4cb6-a73d-3967216817e0.png)

## Q4. What are the total number of crimes under 'Theft and Handling' and 'Violence against the person' per borough in 2016?
```sql
SELECT
  b1 AS Borough,
  Violence_Against_the_Person,
  Theft_and_Handling
FROM ((
    SELECT
      borough AS b1,
      SUM(value) Theft_and_Handling
    FROM
      `bigquery-public-data.london_crime.crime_by_lsoa`
    WHERE
      major_category = "Violence Against the Person"
      AND YEAR = 2016
    GROUP BY
      borough)
  JOIN (
    SELECT
      borough AS b2,
      SUM(value) Violence_Against_the_Person
    FROM
      `bigquery-public-data.london_crime.crime_by_lsoa`
    WHERE
      major_category = "Theft and Handling"
      AND YEAR = 2016
    GROUP BY
      borough)
      ON 
      b1=b2)
  ```
![image](https://user-images.githubusercontent.com/87647832/131227000-e29cd553-a88e-4e58-91b9-2972954091d6.png)

## Q5. What are the total number of crimes under 'Drugs' and 'Robbery' per borough in 2016?
```sql 
SELECT
  b1 AS Borough,
  Drugs, Robbery
FROM ((
    SELECT
      borough AS b1,
      SUM(value) Drugs
    FROM
      `bigquery-public-data.london_crime.crime_by_lsoa`
    WHERE
      major_category = "Drugs"
      AND YEAR = 2016
    GROUP BY
      borough)
  JOIN (
    SELECT
      borough AS b2,
      SUM(value) Robbery
    FROM
      `bigquery-public-data.london_crime.crime_by_lsoa`
    WHERE
      major_category = "Robbery"
      AND YEAR = 2016
    GROUP BY
      borough)
      ON 
      b1=b2)
```
![image](https://user-images.githubusercontent.com/87647832/131227007-90437d8a-ec6a-4d17-815d-02292bb0356d.png)


