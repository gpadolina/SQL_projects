# The World Bank's International Debt Data

In this notebook, we are going to analyze international debt data collected by The World Bank. The dataset
contains information about the amount of debt (in USD) owed by developing countries across several
categories. We are going to find the answers to questions like:
- What is the total amount of debt that is owed by the countries listed in the dataset?
- Which country owns the maximum amount of debt and what does that amount look like?
- What is the average amount of debt owed by countries across different debt indicators?

1. Connect to international_debt database, select all columns from international_debt table, and limit the output.
```
%%sql
postgresql:///international_debt
SELECT *
FROM international_debt
LIMIT 10;
```

2. Find the number of distinct countries.
```
%%sql
SELECT COUNT(DISTINCT country_name) AS total_distinct_countries
FROM international_debt;
```

3. Find the distinct debt indicators.
```
%%sql
SELECT DISTINCT indicator_code AS distinct_debt_indicators
FROM international_debt
ORDER BY distinct_debt_indicators
```
