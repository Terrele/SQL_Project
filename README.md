# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals
Find out which products sell the most in profit and which products are the most popular globally, in each country, city, country and city, and compare it yearly.

## Process
### Step 1
Convert Excel (csv) to PGAdmin (SQL) due to analytics table being too large to fit in Excel. Can also use SQL to query data.
### Step 2
Understand the data. Try to figure out what values each column is trying to give and the type of value.
### Step 3
Clean the data. Fix date formats, price conversions, wrong date format, null columns, etc.

## Results
This table 

## Challenges 
1. Understanding the data. There is a lack of information in all the columns without any comments on what the column is trying to return. 
E.g. The unit price was multiplied by 10^6. We could've also assumed it was multiplied by 10^5 and all the prices would've been 10x bigger. 
What is channel grouping? social engagement type? page path level 1? session quality dim?
What is ordered quantity from the products table? Is it how many products the company ordered? Or is it how many products the customer ordered? (sales)

I believe this would be better in the real world scenario, there would be much less missing and null data, we would come up with the data ourselves and have a much deeper understanding of what our table and columns are trying to return. In the real world, you can also ask the person that created the table what they are looking for.

2. Not being able to update the table. I find it much more difficult to work with data without being able to update the table with clean values as I go along.

## Future Goals
1. Fully understand the data.
2. Clean all the data more thoroughly.
