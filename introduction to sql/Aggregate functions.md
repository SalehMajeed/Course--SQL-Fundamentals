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

Q. 3 -> Use the SUM function to get the total amount grossed by all films made in the year 2000 or later.

Get the average amount grossed by all films whose titles start with the letter 'A'.

Get the amount grossed by the worst performing film in 1994.

Get the amount grossed by the best performing film between 2000 and 2012, inclusive.

```sql
SELECT SUM(gross) FROM films WHERE release_year >= 2000;

SELECT AVG(gross) FROM films WHERE title LIKE 'A%';

SELECT MIN(gross) FROM films WHERE release_year = 1994;

SELECT MAX(gross) FROM films WHERE release_year BETWEEN 2000 AND 2012;
```

Q. 4 -> What is the result of SELECT (10 / 3);?

ans. 3.

Q. 5 -> Get the title and net profit (the amount a film grossed, minus its budget) for all films. Alias the net profit as net_profit.

Get the title and duration in hours for all films. The duration is in minutes, so you'll need to divide by 60.0 to get the duration in hours. Alias the duration in hours as duration_hours.

Get the average duration in hours for all films, aliased as avg_duration_hours.

```sql
SELECT title, (gross - budget) AS net_profit from films;

SELECT title, (duration/ 60.0) as duration_hours FROM films;

SELECT AVG(duration) / 60.0 as avg_duration_hours FROM films;
```

Q. 6 -> Get the percentage of people who are no longer alive. Alias the result as percentage_dead. Remember to use 100.0 and not 100!

Get the number of years between the newest film and oldest film. Alias the result as difference.

Get the number of decades the films table covers. Alias the result as number_of_decades. The top half of your fraction should be enclosed in parentheses.

```sql
SELECT COUNT(deathdate) * 100.0 / COUNT(*) AS percentage_dead FROM people

SELECT MAX(release_year) - MIN(release_year) AS difference FROM films;

SELECT(MAX(release_year) - MIN(release_year)) / 10.0 AS number_of_decades FROM films;
```
