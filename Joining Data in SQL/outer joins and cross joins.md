Q. 1 -> Fill in the code based on the instructions in the code comments to complete the inner join. Note how many records are in the result of the join in the query result tab.

Change the code to perform a LEFT JOIN instead of an INNER JOIN. After executing this query, note how many records the query result contains.

```sql
SELECT c1.name AS city, code, c2.name AS country,
region, city_proper_pop
FROM cities AS c1
INNER JOIN countries AS c2
ON c1.country_code = c2.code ORDER BY code DESC;

SELECT c1.name AS city, code, c2.name AS country,
region, city_proper_pop
FROM cities AS c1
LEFT JOIN countries AS c2
ON c1.country_code = c2.code
ORDER BY code DESC;
```

Q. 2 -> Perform an inner join. Alias the name of the country field as country and the name of the language field as language.
Sort based on descending country name.

Perform a left join instead of an inner join. Observe the result, and also note the change in the number of records in the result.
Carefully review which records appear in the left join result, but not in the inner join result.

```sql
SELECT c.name AS country, local_name, l.name AS language, percent
FROM countries AS c
INNER JOIN languages AS l
ON c.code = l.code
ORDER BY country DESC;

Select c.name AS country, local_name, l.name AS language, percent
FROM countries AS c
LEFT JOIN languages AS l
USING(code)
ORDER BY country DESC;
```

Q. 3 -> Begin with a left join with the countries table on the left and the economies table on the right.
Focus only on records with 2010 as the year.

Modify your code to calculate the average GDP per capita AS avg_gdp for each region in 2010.
Select the region and avg_gdp fields.

Arrange this data on average GDP per capita for each region in 2010 from highest to lowest average GDP per capita.

```sql
SELECT name, region, gdp_percapita
FROM countries AS c
LEFT JOIN economies AS e
USING (code)
WHERE year = 2010;

SELECT c.region, AVG(gdp_percapita) AS avg_gdp
FROM countries AS c
LEFT JOIN economies AS e
USING(code)
WHERE year = 2010
GROUP BY c.region;

SELECT c.region, AVG(gdp_percapita) AS avg_gdp
From countries AS c
Left join economies AS e
USING(code)
WHERE year = 2010
GROUP BY region
ORDER BY avg_gdp DESC;
```

Q. 4 -> The left join code is commented out here. Your task is to write a new query using rights joins that produces the same result as what the query using left joins produces. Keep this left joins code commented as you write your own query just below it using right joins to solve the problem.

Note the order of the joins matters in your conversion to using right joins!

```sql
SELECT cities.name AS city, urbanarea_pop, countries.name AS country,
indep_year, languages.name AS language, percent
FROM languages
RIGHT JOIN countries
ON languages.code = countries.code
RIGHT JOIN cities
ON countries.code = cities.country_code
ORDER BY city, language;
```

Q. 5 -> Choose records in which region corresponds to North America or is NULL.

Repeat the same query as above but use a LEFT JOIN instead of a FULL JOIN. Note what has changed compared to the FULL JOIN result!

Repeat the same query as above but use an INNER JOIN instead of a FULL JOIN. Note what has changed compared to the FULL JOIN and LEFT JOIN results!

```sql
SELECT name AS country, code, region, basic_unit
FROM countries
FULL JOIN currencies
USING (code)
WHERE region = 'North America' or region IS NULL
ORDER BY region;

SELECT name AS country, code, region, basic_unit
FROM countries
LEFT JOIN currencies
USING (code)
WHERE region = 'North America' OR region IS NULL
ORDER BY region;

SELECT name AS country, code, region, basic_unit
FROM countries
JOIN currencies
USING (code)
WHERE region = 'North America' OR region IS NULL
ORDER BY region;
```

Q. 6 -> Choose records in which countries.name starts with the capital letter 'V' or is NULL.
Arrange by countries.name in ascending order to more clearly see the results.

Repeat the same query as above but use a left join instead of a full join. Note what has changed compared to the full join result!

Repeat once more, but use an inner join instead of a left join. Note what has changed compared to the full join and left join results.

```sql
SELECT countries.name, code, languages.name AS language
FROM languages
FULL JOIN countries
USING (code)
Where countries.name LIKE 'V%' OR countries.name IS NULL
ORDER BY countries.name;

SELECT countries.name, code, languages.name AS language
FROM languages
LEFT JOIN countries
USING (code)
WHERE countries.name LIKE 'V%' OR countries.name IS NULL
ORDER BY countries.name;

SELECT countries.name, code, languages.name AS language
FROM languages
JOIN countries
USING (code)
WHERE countries.name LIKE 'V%' OR countries.name IS NULL
ORDER BY countries.name;
```

Q. 7 -> Complete a full join with countries on the left and languages on the right.
Next, full join this result with currencies on the right.
Use LIKE to choose the Melanesia and Micronesia regions (Hint: 'M%esia').
Select the fields corresponding to the country name AS country, region, language name AS language, and basic and fractional units of currency.

```sql
SELECT c1.name AS country, c1.region, l.name AS language,
basic_unit, frac_unit
FROM countries AS c1
FULL JOIN languages AS l
USING (code)
FULL JOIN currencies AS c2
USING (code)
WHERE region LIKE 'Melanesia' OR region LIKE 'Micronesia';
```

Q. 8 -> A(n) **_ join is a join combining the results of a _** join and a \_\_\_ join.

ans. None of the above are true.

Q. 9 -> Create the cross join as described above. (Recall that cross joins do not use ON or USING.)
Make use of LIKE and Hyder% to choose Hyderabad in both countries.
Select only the city name AS city and language name AS language.

Use an inner join instead of a cross join. Think about what the difference will be in the results for this inner join result and the one for the cross join.

```sql
SELECT c.name AS city, l.name AS language
FROM cities AS c
CROSS JOIN languages AS l
WHERE c.name LIKE 'Hyder%';


SELECT c.name AS city, l.name AS language
FROM cities AS c
INNER JOIN languages AS l
ON c.country_code = l.code
WHERE c.name LIKE 'Hyder%';
```

Q. 10 -> Select country name AS country, region, and life expectancy AS life_exp.
Make sure to use LEFT JOIN, WHERE, ORDER BY, and LIMIT.

```sql
SELECT c.name AS country, c.region, p.life_expectancy AS life_exp
FROM countries AS c
JOIN populations AS p
ON c.code = p.country_code
WHERE p.year = 2010
ORDER BY life_exp
LIMIT 5;
```
