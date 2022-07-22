# The Golden Age of Video Games

Video games are big business expected to be worth more than $300 billion in 2027. With so much money at stake, game creators are incentivized to create
the next big hit. But are games getting better, or has the golden age of video games already passed?

In this project, we'll explore the top 400 best-selling video games created between 1977 and 2000.

1. Some of the top selling video games of all time.
```
%%sql
postgresql:///games

-- Select all information for the top ten best-selling games
-- Order the results from best-selling game down to tenth best-selling
SELECT *
FROM game_sales
oRDER BY games_sold DESC
LIMIT 10;
```

2. Missing score reviews.
```
%%sql 

-- Join games_sales and reviews
-- Select a count of the number of games where both critic_score and user_score are null
SELECT COUNT(g.game)
FROM game_sales g
LEFT JOIN reviews r
ON r.game = g.game
WHERE critic_score IS NULL AND user_score IS NULL
```

3. Years that video game critics loved.
```
%%sql

-- Select release year and average critic score for each year, rounded and aliased
-- Join the game_sales and reviews tables
-- Group by release year
-- Order the data from highest to lowest avg_critic_score and limit to 10 results
SELECT g.year, ROUND(AVG(r.critic_score),2) AS avg_critic_score
FROM reviews r
LEFT JOIN game_sales g
ON g.game = r.game
GROUP BY g.year
ORDER BY avg_critic_score DESC
LIMIT 10;
```
