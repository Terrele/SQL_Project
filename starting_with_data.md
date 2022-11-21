Question 1: Which section was most viewed on the website?

SQL Queries: 
```SQL
SELECT split_part(v2_product_category,'/', 2) AS v2_product_category, COUNT(split_part(v2_product_category,'/', 2))
FROM all_sessions
GROUP BY split_part(v2_product_category,'/', 2)
ORDER BY count DESC
```

Answer: 

"Apparel"	5200
"Shop by Brand"	3626
"Electronics"	1315
"Accessories"	958
"Office"	919
	817
"Bags"	761
"Drinkware"	703
"Nest"	436
"Lifestyle"	188

People looked at apparel the most at 5200 views.


Question 2: How long did the average person spend on the website by country in seconds?

SQL Queries:

```SQL
SELECT country, AVG(time_on_site) as avg_time
FROM all_sessions
GROUP BY country
HAVING AVG(time_on_site) is not null
ORDER BY avg_time DESC
```

Answer:

"Peru"	859.1818181818181818
"Nigeria"	752.5000000000000000
"Tunisia"	726.0000000000000000
"Slovenia"	696.0000000000000000
"Kenya"	572.3333333333333333
"El Salvador"	568.0000000000000000
"Trinidad & Tobago"	561.0000000000000000
"Jersey"	518.0000000000000000
"Réunion"	465.0000000000000000
"Morocco"	457.1666666666666667


Question 3: Which product was ordered the most according to the sales report?

SQL Queries:

```SQL
SELECT product_sku, total_ordered, name
FROM sales_report
order BY total_ordered DESC
```

Answer:

"GGOEGOAQ012899"	456	"Ballpoint LED Light Pen"
"GGOEGDHC074099"	334	" 17oz Stainless Steel Sport Bottle"
"GGOEGOCB017499"	319	"Leatherette Journal"
"GGOEGOCC077999"	290	" Spiral Journal with Pen"
"GGOEGFYQ016599"	253	"Foam Can and Bottle Cooler"
"GGOEGOCB078299"	250	" Leather Journal-Black"
"GGOEGHPJ080310"	189	" Blackout Cap"
"GGOEADHH073999"	167	"Android 17oz Stainless Steel Sport Bottle"
"GGOEGAAX0037"	146	" Sunglasses"
"GGOENEBQ078999"	112	" Cam Outdoor Security Camera - USA"


Question 4: Which products did we order the most of according to the products table?

SQL Queries:

```SQL
SELECT sku, name, ordered_quantity
FROM products
ORDER BY ordered_quantity DESC
```

Answer:

"GGOEGFSR022099"	" Kick Ball"	15170
"GGOEGDHC018299"	" 22 oz Water Bottle"	10075
"GGOEGAAX0074"	" 22 oz Water Bottle"	8942
"GGOEGAAX0037"	" Sunglasses"	4204
"GGOEGOCC077999"	" Spiral Journal with Pen"	3896
"GGOEYFKQ020699"	" Custom Decals"	3786
"GGOEGCBQ016499"	"SPF-15 Slim & Slender Lip Balm"	3682
"GGOEGOLC013299"	"Spiral Notebook and Pen Set"	3582
"GGOEGOCB017499"	"Leatherette Journal"	3071
"GGOENEBQ078999"	" Cam Outdoor Security Camera - USA"	2719

#### We have 22 oz water bottle twice with different SKU's, it could be that they are just different brands.

Question 5: What is the percentage of visitors who have made a purchase by country?

SQL Queries:

WITH sub1 as 
(SELECT count(*) AS no_revenue, country
FROM all_sessions
WHERE total_transaction_revenue IS null
GROUP BY country
ORDER BY no_revenue DESC
), sub2 as
(SELECT count(*) AS yes_revenue, country
FROM all_sessions
WHERE total_transaction_revenue IS NOT null
GROUP BY country
ORDER BY yes_revenue DESC
)
SELECT sub1.country, no_revenue, yes_revenue
FROM sub1
FULL OUTER JOIN sub2
ON sub1.country = sub2.country

Answer:

country     no_Revenue     yes_Revenue
"United States"	8651	76
"India"	719	
"United Kingdom"	668	
"Canada"	640	2
"Germany"	336	
"Japan"	241	
"Australia"	224	1
"France"	218	
"Taiwan"	174	
"Netherlands"	158	
"Brazil"	149	
"Italy"	135	
"Spain"	119	
"Singapore"	118	
"Mexico"	103	
"Philippines"	100	
"Russia"	99	
"Indonesia"	90	
"Ireland"	87	
"Poland"	84	
"Switzerland"	84	1
"Czechia"	81	
"Sweden"	76	
"Hong Kong"	74	
"Belgium"	67	

### We can see that United states has (8651+76) visits where 76 are purchases and 8651 are not purchases. This seems like a very low ratio but if you compare it to India, there are 719 visits from India with 0 purchases.