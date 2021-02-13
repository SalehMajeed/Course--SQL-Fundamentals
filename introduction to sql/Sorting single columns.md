# Sorting single columns

# GROUP BY -> allows you to group result according to columns.

# HAVING -> sql doesn't allow aggregate function in WHERE so HAVING is used instead of WHERE.

```sql
SELECT name FROM people ORDER BY name;

SELECT name FROM people ORDER BY birthdate;

SELECT birthdate, name FROM people ORDER BY birthdate;

```

```sql
SELECT title FROM films WHERE release_year IN (2000, 2012) ORDER BY release_year;

SELECT * FROM films WHERE release_year <> 2015 ORDER BY duration;

SELECT title, gross FROM films WHERE title LIKE 'M%' ORDER BY title;
```

```sql
SELECT imdb_score, film_id FROM reviews ORDER BY imdb_score DESC;

SELECT title FROM films ORDER BY title DESC;

SELECT title, duration FROM films ORDER BY duration DESC;
```

```sql
SELECT birthdate, name FROM people ORDER BY birthdate, name;

SELECT release_year, duration, title FROM films ORDER BY release_year, duration;

SELECT certification, release_year, title FROM films ORDER BY certification, release_year;

SELECT name, birthdate FROM people ORDER BY name, birthdate;
```

```sql
SELECT release_year, COUNT(*) FROM films GROUP BY release_year;

SELECT release_year, AVG(duration) FROM films GROUP BY release_year;

SELECT release_year, MAX(budget) FROM films GROUP BY release_year;

SELECT imdb_score, COUNT(*) FROM reviews GROUP BY imdb_score;
```

```sql
SELECT release_year, MIN(gross) FROM films GROUP BY release_year;

SELECT language, SUM(gross) FROM films GROUP BY language;

SELECT country, SUM(budget) FROM films GROUP BY country;

SELECT  release_year, country, MAX(budget) FROM films GROUP BY release_year,country ORDER BY release_year, country;

SELECT country, release_year, MIN(gross) FROM films GROUP BY country, release_year ORDER BY country, release_year;
```

```sql
SELECT release_year
FROM films
GROUP BY release_year
HAVING COUNT(title) > 200;
```

```sql
SELECT release_year, budget, gross FROM films;

SELECT release_year, budget, gross
FROM films
WHERE release_year > 1990;

SELECT release_year
FROM films
WHERE release_year > 1990
GROUP BY release_year;

SELECT release_year, AVG(budget) AS avg_budget, AVG(gross) AS avg_gross
FROM films
WHERE release_year > 1990
GROUP BY release_year;

SELECT release_year, AVG(budget) AS avg_budget, AVG(gross) AS avg_gross
FROM films
WHERE release_year > 1990
GROUP BY release_year
HAVING AVG(budget) > 60000000;

SELECT release_year, AVG(budget) AS avg_budget, AVG(gross) AS avg_gross
FROM films
WHERE release_year > 1990
GROUP BY release_year
HAVING AVG(budget) > 60000000
ORDER BY avg_gross DESC;
```

```sql
SELECT country, AVG(budget) AS avg_budget, AVG(gross) AS avg_gross
 FROM films
 GROUP BY country
 HAVING COUNT(*) > 10
 ORDER BY country
 LIMIT 5;
```

```sql
SELECT title, imdb_score
FROM films
JOIN reviews
ON films.id = reviews.film_id
WHERE title = 'To Kill a Mockingbird';
```
