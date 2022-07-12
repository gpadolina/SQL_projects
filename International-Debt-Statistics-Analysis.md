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

4. Calculate the amount of debt owed by the countries.
```
%%sql
SELECT ROUND(SUM(debt)/1000000,2) AS total_debt
FROM international_debt;
```

5. Find the country with the highest debt.
```
%%sql
SELECT
  country_name,
  SUM(debt) AS total_debt
FROM international_debt
GROUP BY country_name
ORDER BY total_debt DESC
LIMIT 1;
```

6. Find the average amount of debt by indicators.
```
%%sql
SELECT
  indicator_code AS debt_indicator,
  indicator_name,
  AVG(debt) AS average_debt
FROM international_debt
GROUP BY debt_indicator, indicator_name
ORDER BY average_debt DESC
LIMIT 10;
```

7. Find the highest amount of principal payments.
```
%%sql
SELECT
  country_name,
  indicator_name
FROM international_debt
WHERE debt = (SELECT
    max(debt)
  FROM international_debt
  WHERE indicator_code = 'DT.AMT.DLXF.CD');
```

8. Find the most common debt indicator.
```
%%sql
SELECT
  indicator_code,
  COUNT(indicator_code) AS indicator_count
FROM international_debt
GROUP BY indicator_code
ORDER BY indicator_count DESC, indicator_code DESC
LIMIT 20;
```
