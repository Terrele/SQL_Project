What issues will you address by cleaning the data?

Fix date issue in analytics and all_sessions table
Fix unit price issue in analytics and all sessions table



Queries:
Below, provide the SQL queries you used to clean your data.

```SQL
-- convert string to date format
SELECT TO_DATE(date,'YYYYMMDD') as date
FROM analytics
```
```SQL
-- convert string to date format

SELECT TO_DATE(date,'YYYYMMDD') as date
FROM all_sessions
```
```SQL
SELECT CAST(unit_price/1000000 as real) AS new_price 
FROM analytics
```
```SQL
SELECT CAST(product_price/1000000 as real) AS new_product_price
FROM all_sessions
```
```SQL
SELECT CAST(product_revenue/1000000 as real) AS new_product_revenue
FROM all_sessions
```
```SQL
SELECT CAST(total_transaction_revenue/1000000 as real) AS total_transaction_revenue
FROM all_sessions
```
```SQL
SELECT CAST(product_refund_amount/1000000 as real) AS product_refund_amount
FROM all_sessions
```
```SQL
SELECT CAST(item_revenue/1000000 as real) AS item_revenue
FROM all_sessions
```
```SQL
-- Changing the v2_product_category from a file path to a category
SELECT split_part(v2_product_category,'/', -2)
AS v2_product_category
FROM all_sessions
```
```SQL
-- Changing currency_code null values to "USD" since that is the only currency that was used. Could also change the currency code to the mode if there was more than one currency used in the table.
SELECT CASE WHEN currency_code is null THEN 'USD'
	   ELSE currency_code
	   END
	   AS currency_code
	   FROM all_sessions
```

```SQL
-- Contains only null values, provides no useful information
ALTER TABLE all_sessions  
DROP COLUMN search_keyword, product_refund_amount, item_quantity, item_revenue
```

```SQL
-- Convert time in milliseconds to minutes and seconds; can also do a column with seconds only to stay consistent with the time_on_site column which is in seconds but I find minutes and seconds easier to read.
SELECT time/(60*100) AS time_in_minutes, (time/100)%60 AS time_in_seconds FROM all_sessions
```

```SQL
-- The following query displays all the product SKU with more than 1 product name
-- Each SKU should have 1 product name for consistency
SELECT COUNT(DISTINCT v2_product_name), product_sku FROM all_sessions GROUP BY product_sku HAVING COUNT(DISTINCT v2_product_name) > 1 ORDER BY count DESC
```

```SQL
-- This query shows the most used product name for the SKU in product_sku
SELECT COUNT(*), v2_product_name FROM
(SELECT * from all_sessions WHERE product_sku = 'GGOEGEVA022399') as sub
GROUP BY v2_product_name
ORDER BY count DESC
LIMIT 1
```

```SQL
-- Making the product names consistent
SELECT full_visitor_id, product_sku, 'Micro Wireless Earbud' as v2_product_name 
FROM all_sessions 
WHERE product_sku = 'GGOEGEVA022399'
```