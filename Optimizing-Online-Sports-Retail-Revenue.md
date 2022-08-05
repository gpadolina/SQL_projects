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
