# Optimizing Online Sports Retail Revenue

In this project, we will a product analyst for an online sports clothing company. The company is specifically interested in how it can improve revenue. We will dive into product data such as pricing, reviews, descriptions, and ratings, as well as revenue and website traffic, to produce recommendations for its marketing and sales teams.

The database provided to us, sports, contains five tables, with product_id being the primary key for all of them:info, finance, reviews, traffic, and brands.

1. Let's explore the data.
```
%%sql
postgresql:///sports

-- Count all columns as total_rows
-- Count the number of non-missing entries for description, listing_price, and last_visited
-- Join info, finance, and traffic
SELECT COUNT(*) AS total_rows, 
    COUNT(i.description) AS count_description, 
    COUNT(f.listing_price) AS count_listing_price, 
    COUNT(t.last_visited) AS count_last_visited 
FROM info i
INNER JOIN finance f
    ON i.product_id = f.product_id
INNER JOIN traffic AS t
    ON t.product_id = f.product_id;
 ```
 
 2. How do the price points of Nike and Adidas products differ? Answering this question can help us build a picture of the company's stock range and customer market. We will run a query to produce a distribution of the listing_price and the count for each price, grouped by brand.
```
%%sql

-- Select the brand, listing_price as an integer, and a count of all products in finance 
-- Join brands to finance on product_id
-- Aggregate results by brand and listing_price, and sort the results by listing_price in descending order
-- Filter for products with a listing_price more than zero
SELECT b.brand, f.listing_price::integer, COUNT(f.*)
FROM finance f
INNER JOIN brands b 
    ON f.product_id = b.product_id
GROUP BY b.brand, f.listing_price
HAVING listing_price > 0
ORDER BY listing_price DESC;
```

3. Let's build on our previous query by assigning labels to different price ranges, grouping by brand and label. We will also include the total revenue for each price range and brand.
```
%%sql

-- Select the brand, a count of all products in the finance table, and total revenue
-- Create four labels for products based on their price range, aliasing as price_category
-- Join brands to finance on product_id
-- Group results by brand and price_category, sort by total_revenue and filter out products missing a value for brand
SELECT b.brand, COUNT(f.*), SUM(f.revenue) as total_revenue,
CASE WHEN f.listing_price < 42 THEN 'Budget'
    WHEN f.listing_price >= 42 AND f.listing_price < 74 THEN 'Average'
    WHEN f.listing_price >= 74 AND f.listing_price < 129 THEN 'Expensive'
    ELSE 'Elite' END AS price_category
FROM finance f
INNER JOIN brands b 
    ON f.product_id = b.product_id
GROUP BY b.brand, price_category
HAVING b.brand IS NOT NULL
ORDER BY total_revenue DESC;
```

4. Note we have been looking at listing_price so far. The listing_price may not be the price that the product is ultimately sold for. To understand revenue better, let's take a look at the discount, which is the percent reduction in the listing_price when the product is actually sold. We would like to know whether there is a difference in the amount of discount offered between brands, as this could be influencing revenue.
```
%%sql

-- Select brand and average_discount as a percentage
-- Join brands to finance on product_id
-- Aggregate by brand
-- Filter for products without missing values for brand
SELECT b.brand, AVG(f.discount) * 100 AS average_discount
FROM brands b
INNER JOIN finance f 
    ON b.product_id = f.product_id
GROUP BY b.brand
HAVING b.brand IS NOT NULL
ORDER BY average_discount;
```

5. Now explore whether relationships exist between the columns in our database. We will check the strength and direction of a correlation between revenue and reviews.
```
%%sql

-- Calculate the correlation between reviews and revenue as review_revenue_corr
-- Join the reviews and finance tables on product_id
SELECT corr(r.reviews, f.revenue) AS review_revenue_corr
FROM reviews r
INNER JOIN finance f 
    ON r.product_id = f.product_id;
```

6. Perhaps the length of a product's description might influence a product's rating and reviews — if so, the company can produce content guidelines for listing products on their website and test if this influences revenue. Let's check this out!
```
%%sql

-- Calculate description_length
-- Convert rating to an integer and calculate average_rating
-- Join info to reviews on product_id and group the results by description_length
-- Filter for products without missing values for description, and sort results by description_length
SELECT TRUNC(LENGTH(i.description), -2) AS description_length,
    ROUND(AVG(r.rating::numeric), 2) AS average_rating
FROM info i
INNER JOIN reviews r 
    ON i.product_id = r.product_id
WHERE i.description IS NOT NULL
GROUP BY description_length
ORDER BY description_length;
```

7. As we know a correlation exists between reviews and revenue, one approach the company could take is to run experiments with different sales processes encouraging more reviews from customers about their purchases, such as by offering a small discount on future purchases.

Let's take a look at the volume of reviews by month to see if there are any trends or gaps we can look to exploit.
```
%%sql

-- Select brand, month from last_visited, and a count of all products in reviews aliased as num_reviews
-- Join traffic with reviews and brands on product_id
-- Group by brand and month, filtering out missing values for brand and month
-- Order the results by brand and month
SELECT b.brand, DATE_PART('month', t.last_visited) AS month, COUNT(r.*) AS num_reviews
FROM brands b
INNER JOIN traffic t 
    ON b.product_id = t.product_id
INNER JOIN reviews r 
    ON t.product_id = r.product_id
GROUP BY b.brand, month
HAVING b.brand IS NOT NULL
    AND DATE_PART('month', t.last_visited) IS NOT NULL
ORDER BY b.brand, month;
```
