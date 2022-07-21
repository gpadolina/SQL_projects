# What and Where are the World's Oldest Businesses

In this notebook, we'll explore business that has been around for hundreds of years and have survived changing market conditions.

1. Range of the founding years.
```
%%sql 
postgresql:///oldestbusinesses
 
-- Select the oldest and newest founding years from the businesses table
SELECT MIN(year_founded), MAX(year_founded)
FROM businesses
```

2. Number of businesses with founding year before 1000.
```
%%sql

-- Get the count of rows in businesses where the founding year was before 1000
SELECT COUNT(*)
FROM businesses
WHERE year_founded < 1000
```

3. Businesses founded before year 1000.
```
%%sql

-- Select all columns from businesses where the founding year was before 1000
-- Arrange the results from oldest to newest
SELECT *
FROM businesses
WHERE year_founded < 1000
ORDER BY year_founded ASC
```
