Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:

```SQL
SELECT country, city, SUM(CAST(total_transaction_revenue/1000000 as real)) as total_transaction_revenue 
FROM all_sessions 
WHERE total_transaction_revenue IS NOT null
AND city != 'not available in demo dataset'
GROUP BY country, city
ORDER BY total_transaction_revenue DESC
```

Answer:

"United States"	"San Francisco"	1564.32
"United States"	"Sunnyvale"	992.23
"United States"	"Atlanta"	854.44
"United States"	"Palo Alto"	608
"Israel"	"Tel Aviv-Yafo"	602
"United States"	"New York"	530.36
"United States"	"Mountain View"	483.36002
"United States"	"Los Angeles"	479.48
"United States"	"Chicago"	449.52
"United States"	"Seattle"	358
"Australia"	"Sydney"	358
"United States"	"San Jose"	262.38
"United States"	"Austin"	157.78
"United States"	"Nashville"	157
"United States"	"San Bruno"	103.77
"Canada"	"Toronto"	82.16
"Canada"	"New York"	67.99
"United States"	"Houston"	38.98
"United States"	"Columbus"	21.99
"Switzerland"	"Zurich"	16.99

**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:

```SQL
-- There are rows with country and city being the same thing and rows with city being null. E.g. Country and City = 'Hong Kong' and Country = 'Hong Kong' and City = 'not available in demo dataset'. This would change the average since they belong to different rows even though they are the same.

WITH newtable AS (
SELECT country, CASE WHEN city = 'not available in demo dataset' THEN country
				ELSE city
				END as
city, total_ordered
FROM all_sessions al
JOIN sales_by_sku sa
ON al.product_sku = sa.product_sku
)
SELECT country, city, CAST(AVG(total_ordered) as real) 
FROM newtable
GROUP BY country, city
ORDER BY avg DESC
```

Answer:

"Saudi Arabia"	"Riyadh"	319
"Czechia"	"Brno"	319
"United States"	"Rexburg"	250.5
"United States"	"Sacramento"	189
"Portugal"	"Lisbon"	189
"United States"	"Kalamazoo"	105
"Russia"	"Saint Petersburg"	101.25
"United States"	"Avon"	100
"Italy"	"Rome"	97.75
"Taiwan"	"Longtan District"	97
"Canada"	"Sherbrooke"	97
"United States"	"Nashville"	94
"United Arab Emirates"	"Dubai"	89.2
"Kuwait"	"Kuwait"	85.75
"Oman"	"Oman"	85
"Laos"	"Laos"	85
"Ethiopia"	"Ethiopia"	85
"Japan"	"Shinjuku"	73
"India"	"Pune"	71.083336
"South Africa"	"Westville"	70
"Brazil"	"Rio de Janeiro"	65.333336
"Croatia"	"Croatia"	63.875
"United States"	"Bellingham"	62
"Russia"	"Vladivostok"	62
"Argentina"	"Santa Fe"	60
"Saudi Arabia"	"Saudi Arabia"	59.166668
"United States"	"San Antonio"	58.166668
"South Korea"	"Seoul"	57.64706
"Papua New Guinea"	"Papua New Guinea"	57.5
"Honduras"	"Honduras"	56

**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:

```SQL
SELECT country, v2_product_category, date, COUNT(v2_product_category)  FROM (
SELECT country, split_part(v2_product_category,'/', -2) AS v2_product_category, DATE_PART('year', TO_DATE(date,'YYYYMMDD')) as date
FROM all_sessions) AS sub
GROUP BY 3,2,1
ORDER BY count DESC
```

```SQL
SELECT city, v2_product_category, date, COUNT(v2_product_category) FROM (
SELECT city, DATE_PART('year', TO_DATE(date,'YYYYMMDD')) as date, split_part(v2_product_category,'/', -2) AS v2_product_category
FROM all_sessions) AS sub
WHERE city != 'not available in demo dataset' AND city != '(not set)'
GROUP BY 1,2,3
ORDER BY count DESC
```

```SQL
SELECT country, city, v2_product_category, date, COUNT(v2_product_category) FROM (
SELECT country, city, DATE_PART('year', TO_DATE(date,'YYYYMMDD')) as date, split_part(v2_product_category,'/', -2) AS v2_product_category
FROM all_sessions) AS sub
WHERE city != 'not available in demo dataset' AND city != '(not set)'
GROUP BY 1,2,3,4
ORDER BY count DESC

```

Answer:
"United States"	"YouTube"	2017	597
"United States"	"Men's-T-Shirts"	2016	522
"United States"	"Men's-T-Shirts"	2017	404
"United States"	"Apparel"	2016	346

"Mountain View"	"Nest-USA"	2017	65
"Mountain View"	"Men's-T-Shirts"	2016	62
"Mountain View"	"Office"	2016	57
"Mountain View"	"Google"	2016	53

"United States"	"Mountain View"	"Nest-USA"	2017	65
"United States"	"Mountain View"	"Men's-T-Shirts"	2016	62
"United States"	"Mountain View"	"Office"	2016	57
"United States"	"Mountain View"	"Google"	2016	53
"United States"	"Mountain View"	"Men's-T-Shirts"	2017	48

In 2017, Youtube was very popular


**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:

```SQL
-- Get all transactions from the sessions table 
SELECT COUNT(v2_product_name), v2_product_name, country, city 
FROM all_sessions
WHERE transactions is not null
GROUP BY 2,3,4
ORDER BY count DESC
```


Answer:

2	"Nest® Learning Thermostat 3rd Gen-USA - Stainless Steel"	"United States"	"not available in demo dataset"
2	"Nest® Protect Smoke + CO White Wired Alarm-USA"	"United States"	"not available in demo dataset"
1	"26 oz Double Wall Insulated Bottle"	"United States"	"Houston"
1	"Android 17oz Stainless Steel Sport Bottle"	"United States"	"San Francisco"
1	"Android Infant Short Sleeve Tee Pewter"	"United States"	"Mountain View"
1	"Android Men's Long Sleeve Badge Crew Tee Heather"	"United States"	"not available in demo dataset"
1	"Android Men's Vintage Henley"	"United States"	"Sunnyvale"
1	"Android Rise 14 oz Mug"	"United States"	"San Francisco"
1	"Android Women's Fleece Hoodie"	"United States"	"Mountain View"
1	"Android Women's Short Sleeve Tri-blend Badge Tee Light Grey"	"United States"	"not available in demo dataset"





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:

```SQL
SELECT SUM(CAST(total_transaction_revenue/1000000 as real)) as revenue, country, city 
FROM all_sessions
GROUP BY country, city
HAVING SUM(total_transaction_revenue) IS NOT null
ORDER BY revenue DESC
```

Answer:

6092.56	"United States"	"not available in demo dataset"
1564.32	"United States"	"San Francisco"
992.23	"United States"	"Sunnyvale"
854.44	"United States"	"Atlanta"
608	"United States"	"Palo Alto"
602	"Israel"	"Tel Aviv-Yafo"
530.36	"United States"	"New York"
483.36002	"United States"	"Mountain View"
479.48	"United States"	"Los Angeles"
449.52	"United States"	"Chicago"
358	"United States"	"Seattle"
358	"Australia"	"Sydney"
262.38	"United States"	"San Jose"
157.78	"United States"	"Austin"
157	"United States"	"Nashville"
103.77	"United States"	"San Bruno"
82.16	"Canada"	"Toronto"
67.99	"Canada"	"New York"
38.98	"United States"	"Houston"
21.99	"United States"	"Columbus"
16.99	"Switzerland"	"Zurich"






