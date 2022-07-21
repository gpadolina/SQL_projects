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

4. Exploring the categories.
```
%%sql

-- Select business name, founding year, and country code from businesses; and category from categories
-- where the founding year was before 1000, arranged from oldest to newest
SELECT b.business, b.year_founded, b.country_code, c.category
FROM businesses b
JOIN categories c
ON b.category_code = c.category_code
WHERE b.year_founded < 1000
ORDER BY b.year_founded ASC
```

5. Counting the categories.
```
%%sql

-- Select the category and count of category (as "n")
-- arranged by descending count, limited to 10 most common categories
SELECT category, COUNT(category) AS n
FROM categories
JOIN businesses
USING(category_code)
GROUP BY category
ORDER BY n DESC
LIMIT 10;
```

6. Oldest business by continent.
```
%%sql

-- Select the oldest founding year (as "oldest") from businesses, 
-- and continent from countries
-- for each continent, ordered from oldest to newest 
SELECT MIN(year_founded) AS oldest, continent
FROM countries c
JOIN businesses b
USING(country_code)
GROUP BY continent
ORDER BY oldest ASC
```

7. Joining all tables for further analysis.
```
%%sql

-- Select the business, founding year, category, country, and continent
SELECT business, year_founded, category, country, continent
FROM businesses
JOIN categories
USING(category_code)
INNER JOIN countries
USING(country_code);
```
