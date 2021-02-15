Q. 1 -> Combine these two tables into one table containing all of the fields in economies2010. The economies table is also included for reference.

Sort this resulting single table by country code and then by year, both in ascending order.

```sql
SELECT * FROM economies2010
UNION
SELECT * FROM economies2015
ORDER BY code, year;
```

Q. 2 -> Determine all (non-duplicated) country codes in either the cities or the currencies table. The result should be a table with only one field called country_code.

Sort by country_code in alphabetical order.

```sql
SELECT country_code
FROM cities
UNION
SELECT code
FROM currencies
ORDER BY country_code;
```

Q. 3 -> Determine all combinations (include duplicates) of country code and year that exist in either the economies or the populations tables. Order by code then year.

The result of the query should only have two columns/fields. Think about how many records this query should result in.

You'll use code very similar to this in your next exercise after the video. Make note of this code after completing it.

```sql
SELECT code, year
FROM economies
UNION ALL
SELECT country_code, year
FROM populations
ORDER BY code, year;
```

Q. 4 -> Again, order by code and then by year, both in ascending order.
Note the number of records here (given at the bottom of query result) compared to the similar UNION ALL query result (814 records).

```sql
SELECT code, year
FROM economies
INTERSECT
SELECT country_code, year
FROM populations
ORDER BY code, year;
```

Q. 5 -> Use INTERSECT to answer this question with countries and cities!

```sql
SELECT name FROM countries
INTERSECT
SELECT name FROM cities;
```

Q. 6 -> Which of the following combinations of terms and definitions is correct?

ans. INTERSECT: returns only records appearing in both tables.

Q. 7 -> Order the resulting field in ascending order.
Can you spot the city/cities that are actually capital cities which this query misses?

```sql
SELECT name
FROM cities
EXCEPT
SELECT capital
FROM countries
ORDER BY name;
```

Q. 8 -> Order by capital in ascending order.
The cities table contains information about 236 of the world's most populous cities. The result of your query may surprise you in terms of the number of capital cities that DO NOT appear in this list!

```sql
SELECT capital
FROM countries
EXCEPT
SELECT name
FROM cities
ORDER BY capital;
```

Q. 9 -> Flash back to our Intro to SQL for Data Science course and begin by selecting all country codes in the Middle East as a single field result using SELECT, FROM, and WHERE.

Comment out the answer to the previous tab by surrounding it in /_ and _/. You'll come back to it!
Below the commented code, select only unique languages by name appearing in the languages table.
Order the resulting single field table by name in ascending order.

Now combine the previous two queries into one query:
Add a WHERE IN statement to the SELECT DISTINCT query, and use the commented out query from the first instruction in there. That way, you can determine the unique languages spoken in the Middle East.
Carefully review this result and its code after completing it. It serves as a great example of subqueries, which are the focus of Chapter 4.

```sql
SELECT code
FROM countries
WHERE region LIKE 'Middle East';

SELECT DISTINCT name FROM languages
ORDER BY name;

SELECT DISTINCT name FROM languages
WHERE code IN(
SELECT code
FROM countries
WHERE region LIKE 'Middle East'
)
ORDER BY name;
```

Q. 10 ->

```sql
SELECT DISTINCT name
FROM languages
WHERE code IN
  (SELECT code
   FROM countries
   WHERE region = 'Middle East')
ORDER BY name;
```

Sometimes problems solved with semi-joins can also be solved using an inner join.

```sql
SELECT languages.name AS language
FROM languages
INNER JOIN countries
ON languages.code = countries.code
WHERE region = 'Middle East'
ORDER BY language;
```

ans. DISTINCT.

Q. 11 -> Begin by determining the number of countries in countries that are listed in Oceania using SELECT, FROM, and WHERE.

Complete an inner join with countries AS c1 on the left and currencies AS c2 on the right to get the different currencies used in the countries of Oceania.
Match ON the code field in the two tables.
Include the country code, country name, and basic_unit AS currency.
Observe query result and make note of how many different countries are listed here.

Note that not all countries in Oceania were listed in the resulting inner join with currencies. Use an anti-join to determine which countries were not included!

Use NOT IN and (SELECT code FROM currencies) as a subquery to get the country code and country name for the Oceanian countries that are not included in the currencies table.

```sql
SELECT COUNT(name)
FROM countries
WHERE continent = 'Oceania'

SELECT c1.code, c1.name, c2.basic_unit AS currency
FROM countries AS c1
JOIN currencies AS c2
USING(code)
WHERE c1.continent = 'Oceania';

SELECT code, name
FROM countries AS c1
WHERE continent = 'Oceania'
AND code NOT IN(
	SELECT code FROM currencies
);
```

Q. 12 -> Identify the country codes that are included in either economies or currencies but not in populations.
Use that result to determine the names of cities in the countries that match the specification in the previous instruction.

```sql
SELECT name
FROM cities AS c1
WHERE country_code IN
(
    SELECT e.code
    FROM economies AS e
    UNION
    SELECT c2.code
    FROM currencies AS c2
    EXCEPT
    SELECT p.country_code
    FROM populations AS p
);
```
