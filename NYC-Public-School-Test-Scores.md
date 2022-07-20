# NYC Public School Test Result Scores

In this notebook, we will take a look at SATs across public schools in New York City. SATs are standardized tests intended to measure literacy, numeracy,
and writing skills. The test has three sections - math, reading, and writing, each with a maximum score of 800 points.

Analyzing the performance of schools is important for a variety of stakeholders, including policy and education professionals, researchers, government, 
and even parents considering which school their children should attend.

1. Connect to schools database and take a look at first few schools.
```
%%sql
postgresql:///schools
    
-- Select all columns from the database
SELECT *
FROM schools
LIMIT 10;
```

2. Find missing values.
```
%%sql

-- Count rows with percent_tested missing and total number of schools
SELECT COUNT(*) - COUNT(percent_tested) AS num_tested_missing,
COUNT(school_name) AS num_schools
FROM schools
```

3. Find number of unique schools by building code.
```
-- Count the number of unique building_code values
SELECT COUNT(DISTINCT building_code) AS num_school_buildings
FROM schools
```

4. Best schools for math.
```
%%sql

-- Select school and average_math
-- Filter for average_math 640 or higher
-- Display from largest to smallest average_math
SELECT school_name, average_math
FROM schools
WHERE average_math >= 640
ORDER BY average_math DESC
```

5. Lowest reading score.
```
%%sql

-- Find lowest average_reading
SELECT MIN(average_reading) AS lowest_reading
FROM schools
```

6. Best writing school.
```
%%sql

-- Find the top score for average_writing
-- Group the results by school
-- Sort by max_writing in descending order
-- Reduce output to one school
SELECT school_name, MAX(average_writing) AS max_writing
FROM schools
GROUP BY school_name
ORDER BY max_writing DESC
LIMIT 1;
```

7. Top 10 schools.
```
%%sql

-- Calculate average_sat
-- Group by school_name
-- Sort by average_sat in descending order
-- Display the top ten results
SELECT school_name, SUM(average_math + average_reading + average_writing) AS average_sat
FROM schools
GROUP BY school_name
ORDER BY average_sat DESC
LIMIT 10;
```
