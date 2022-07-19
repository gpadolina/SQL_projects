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
