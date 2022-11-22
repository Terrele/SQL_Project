What are your risk areas? Identify and describe them.
The risk areas is to verify that your queries are correct and giving you the results that you are looking for.


QA Process:
Describe your QA process and include the SQL queries used to execute it.

```SQL
--Total Rows including duplicates
select COUNT(*) from all_sessions
```
15134

```SQL
--Count the distinct number of rows
SELECT COUNT(*) FROM (
SELECT DISTINCT ON (full_visitor_id) * FROM all_sessions
) sub
```
14223

```SQL
--Count all the duplicate rows only
SELECT SUM(count) FROM (
SELECT COUNT(full_visitor_id)-1 AS count, full_visitor_id 
FROM all_sessions
GROUP BY full_visitor_id
HAVING count(full_visitor_id) > 1) sub
```
911
#### Total rows including duplicates = Distinct number of rows + duplicate rows only
#### We have 15134 = 14223 + 911 so we know the queries are correct.
#### This is just one of the many queries to verify that is working correctly.