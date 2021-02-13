# Aggregate functions

## AVG() -> return average.

## MAX() -> return maximum value.

## MIN() -> return minimum value.

## SUM() -> return sum of all values.

# ALIASING -> assign temporary name to something.

# ORDER BY -> used to sort result in ascending or descending order. default is ascending.

Q. 1 -> Use the SUM function to get the total duration of all films.

Get the average duration of all films.

Get the duration of the shortest film.

Get the duration of the longest film.

```sql
SELECT SUM(duration) FROM films;

SELECT AVG(duration) FROM films;

SELECT MIN(duration) FROM films;

SELECT MAX(duration) FROM films;
```

Q. 2 -> Use the SUM function to get the total amount grossed by all films.

Get the average amount grossed by all films.

Get the amount grossed by the worst performing film.

Get the amount grossed by the best performing film.

```sql
SELECT SUM(gross) FROM films;

SELECT AVG(gross) FROM films;

SELECT MIN(gross) FROM films;

SELECT MAX(gross) FROM films;

```

SELECT SUM(gross) FROM films WHERE release_year >= 2000;

SELECT AVG(gross) FROM films WHERE title LIKE 'A%';

SELECT MIN(gross) FROM films WHERE release_year = 1994;

SELECT MAX(gross) FROM films WHERE release_year BETWEEN 2000 AND 2012;

```sql
SELECT title, (gross - budget) AS net_profit from films;

SELECT title, (duration/ 60.0) as duration_hours FROM films;

SELECT AVG(duration) / 60.0 as avg_duration_hours FROM films;

SELECT COUNT(deathdate) * 100.0 / COUNT(*) AS percentage_dead FROM people

SELECT MAX(release_year) - MIN(release_year) AS difference FROM films;

SELECT(MAX(release_year) - MIN(release_year)) / 10.0 AS number_of_decades FROM films;
```
