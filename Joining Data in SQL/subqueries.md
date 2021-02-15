Q. 1 -> Begin by calculating the average life expectancy across all countries for 2015.

Recall that you can use SQL to do calculations for you. Suppose we wanted only records that were above 1.15 \* 100 in terms of life expectancy for 2015:

```sql
SELECT *
  FROM populations
WHERE life_expectancy > 1.15 * 100
  AND year = 2015;
```

Select all fields from populations with records corresponding to larger than 1.15 times the average you calculated in the first task for 2015. In other words, change the 100 in the example above with a subquery.

```sql
SELECT AVG(life_expectancy)
FROM populations
WHERE year = 2015;

SELECT *
FROM populations
WHERE life_expectancy > 1.15 * (
  SELECT AVG(life_expectancy) FROM populations
  WHERE year = 2015
)
AND year = 2015;
```

Q. 2 -> Make use of the capital field in the countries table in your subquery.

Select the city name, country code, and urban area population fields.

```sql
SELECT name, country_code, urbanarea_pop
FROM cities
WHERE name IN (
  SELECT capital FROM countries
)
ORDER BY urbanarea_pop DESC;
```

Q. 3 -> Just Submit Answer here!

Remove the comments around the second query and comment out the first query instead.

Convert the GROUP BY code to use a subquery inside of SELECT, i.e. fill in the blanks to get a result that matches the one given using the GROUP BY code in the first query.

Again, sort the result by cities_num descending and then by country ascending.

```sql
SELECT countries.name AS country, COUNT(*) AS cities_num
  FROM cities
    INNER JOIN countries
    ON countries.code = cities.country_code
GROUP BY country
ORDER BY cities_num DESC, country
LIMIT 9;

SELECT countries.name AS country,
  (SELECT COUNT(*)
   FROM cities
   WHERE countries.code = cities.country_code) AS cities_num
FROM countries
ORDER BY cities_num DESC, country
LIMIT 9;
```

Q. 4 -> Begin by determining for each country code how many languages are listed in the languages table using SELECT, FROM, and GROUP BY.
Alias the aggregated field as lang_num.

Include the previous query (aliased as subquery) as a subquery in the FROM clause of a new query.
Select the local name of the country from countries.
Also, select lang_num from subquery.
Make sure to use WHERE appropriately to match code in countries and in subquery.
Sort by lang_num in descending order.

```sql
SELECT code, COUNT(*) AS lang_num
FROM languages
GROUP BY code;

SELECT local_name, subquery.lang_num
FROM countries,
(
SELECT code, COUNT(*) AS lang_num
FROM languages
GROUP BY code
) AS subquery
WHERE countries.code = subquery.code
ORDER BY lang_num DESC;
```

Q. 5 -> Now it's time to append the second part's query to the first part's query using AND and IN to obtain the name of the country, its continent, and the maximum inflation rate for each continent in 2015!
For the sake of practice, change all joining conditions to use ON instead of USING (based upon the same column, code).
Revisit the sample output in the assignment text at the beginning of the exercise to see how this matches up.

Select the maximum inflation rate in 2015 AS max_inf grouped by continent using the previous step's query as a subquery in the FROM clause.

Thus, in your subquery you should:
Create an inner join with countries on the left and economies on the right with USING (without aliasing your tables or columns).
Retrieve the country name, continent, and inflation rate for 2015.
Alias the subquery as subquery.
This will result in the six maximum inflation rates in 2015 for the six continents as one field table. Make sure to not include continent in the outer SELECT statement.

Now it's time to append your second query to your first query using AND and IN to obtain the name of the country, its continent, and the maximum inflation rate for each continent in 2015.
For the sake of practice, change all joining conditions to use ON instead of USING.

```sql
SELECT name, continent, e.inflation_rate
FROM countries
JOIN economies AS e
USING(code)
WHERE year = 2015;

SELECT MAX(inflation_rate) AS max_inf
FROM (
SELECT name,continent, inflation_rate
FROM countries
INNER JOIN economies
USING(code)
WHERE year = 2015
) AS subquery
GROUP BY continent;

SELECT name, continent, inflation_rate
FROM countries
INNER JOIN economies
ON countries.code = economies.code
WHERE year = 2015
AND inflation_rate IN (
SELECT MAX(inflation_rate)
AS max_inf
FROM (
SELECT name, continent, inflation_rate
FROM countries
INNER JOIN economies
ON countries.code = economies.code
WHERE year = 2015
) AS subquery
GROUP BY continent);
```

Q. 6 -> Select the country code, inflation rate, and unemployment rate.
Order by inflation rate ascending.
Do not use table aliasing in this exercise.

```sql
SELECT code, inflation_rate, unemployment_rate
FROM economies
WHERE year = 2015 AND code NOT IN
(SELECT code
FROM countries
WHERE (gov_form = 'Constitutional Monarchy' OR gov_form LIKE '%Republic'))
ORDER BY inflation_rate;
```

Q. 7 -> Within which SQL clause are subqueries most frequently found?

ans. WHERE.

Q. 8 -> Select unique country names. Also select the total investment and imports fields.

Use a left join with countries on the left. (An inner join would also work, but please use a left join here.)
Match on code in the two tables AND use a subquery inside of ON to choose the appropriate languages records.

Order by country name ascending.
Use table aliasing but not field aliasing in this exercise.

```sql
SELECT DISTINCT c.name, e.total_investment, e.imports
FROM countries AS c
LEFT JOIN economies AS e
ON (c.code = e.code
AND c.code IN (
          SELECT l.code
          FROM languages AS l
          WHERE official = 'true'
        ) )
  WHERE region = 'Central America' AND year = 2015
ORDER BY name;
```

Q. 9 -> Include the name of region, its continent, and average fertility rate aliased as avg_fert_rate.
Sort based on avg_fert_rate ascending.
Remember that you'll need to GROUP BY all fields that aren't included in the aggregate function of SELECT.

```sql
SELECT c1.region, c1.continent, AVG(p.fertility_rate) AS avg_fert_rate
FROM countries AS c1
INNER JOIN populations AS p
ON c1.code = p.country_code
WHERE year = 2015
GROUP BY region, continent
ORDER BY avg_fert_rate;
```

Q. 10 -> Select the city name, country code, city proper population, and metro area population.
Calculate the percentage of metro area population composed of city proper population for each city in cities, aliased as city_perc.
Focus only on capital cities in Europe and the Americas in a subquery.
Make sure to exclude records with missing data on metro area population.
Order the result by city_perc descending.
Then determine the top 10 capital cities in Europe and the Americas in terms of this city_perc percentage.

```sql
SELECT name, country_code, city_proper_pop, metroarea_pop,
      (city_proper_pop / metroarea_pop) * 100 AS city_perc
FROM cities
WHERE name IN
    (SELECT capital
     FROM countries
     WHERE (continent = 'Europe'
        OR continent LIKE '%America'))
       AND metroarea_pop IS NOT NULL
ORDER BY city_perc DESC
LIMIT 10;
```
