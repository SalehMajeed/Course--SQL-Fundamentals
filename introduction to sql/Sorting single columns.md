# GROUP BY -> allows you to group result according to columns.

# HAVING -> sql doesn't allow aggregate function in WHERE so HAVING is used instead of WHERE.

Q. 1 -> How do you think ORDER BY sorts a column of text values by default?

ans.
Alphabetically (A-Z)

Q. 2 -> Get the names of people from the people table, sorted alphabetically.

Get the names of people, sorted by birth date.

Get the birth date and name for every person, in order of when they were born.

```sql
SELECT name FROM people ORDER BY name;

SELECT name FROM people ORDER BY birthdate;

SELECT birthdate, name FROM people ORDER BY birthdate;
```

Q. 3 -> Get the title of films released in 2000 or 2012, in the order they were released.

Get all details for all films except those released in 2015 and order them by duration.

Get the title and gross earnings for movies which begin with the letter 'M' and order the results alphabetically.

```sql
SELECT title FROM films WHERE release_year IN (2000, 2012) ORDER BY release_year;
Finally, modify your query to order the results from highest average gross earnings to lowest.


SELECT * FROM films WHERE release_year <> 2015 ORDER BY duration;

SELECT title, gross FROM films WHERE title LIKE 'M%' ORDER BY title;
```

Q. 4 -> Get the IMDB score and film ID for every film from the reviews table, sorted from highest to lowest score.

Get the title for every film, in reverse order.

Get the title and duration for every film, in order of longest duration to shortest.

```sql
SELECT imdb_score, film_id FROM reviews ORDER BY imdb_score DESC;

SELECT title FROM films ORDER BY title DESC;

SELECT title, duration FROM films ORDER BY duration DESC;
```

Q. 5 -> Get the birth date and name of people in the people table, in order of when they were born and alphabetically by name.

Get the release year, duration, and title of films ordered by their release year and duration.

Get certifications, release years, and titles of films ordered by certification (alphabetically) and release year.

Get the names and birthdates of people ordered by name and birth date.

```sql
SELECT birthdate, name FROM people ORDER BY birthdate, name;

SELECT release_year, duration, title FROM films ORDER BY release_year, duration;

SELECT certification, release_year, title FROM films ORDER BY certification, release_year;

SELECT name, birthdate FROM people ORDER BY name, birthdate;
```

Q. 6 -> What is GROUP BY used for?

ans.
Performing operations by group.

Q. 7 -> Get the release year and count of films released in each year.

Get the release year and average duration of all films, grouped by release year.

Get the release year and largest budget for all films, grouped by release year.

Get the IMDB score and count of film reviews grouped by IMDB score in the reviews table.

```sql
SELECT release_year, COUNT(*) FROM films GROUP BY release_year;

SELECT release_year, AVG(duration) FROM films GROUP BY release_year;

SELECT release_year, MAX(budget) FROM films GROUP BY release_year;

SELECT imdb_score, COUNT(*) FROM reviews GROUP BY imdb_score;
```

Q. 8 -> Get the release year and lowest gross earnings per release year.

Get the language and total gross amount films in each language made.

Get the country and total budget spent making movies in each country.

Get the release year, country, and highest budget spent making a film for each year, for each country. Sort your results by release year and country.

Get the country, release year, and lowest amount grossed per release year per country. Order your results by country and release year.

```sql
SELECT release_year, MIN(gross) FROM films GROUP BY release_year;

SELECT language, SUM(gross) FROM films GROUP BY language;

SELECT country, SUM(budget) FROM films GROUP BY country;

SELECT  release_year, country, MAX(budget) FROM films GROUP BY release_year,country ORDER BY release_year, country;

SELECT country, release_year, MIN(gross) FROM films GROUP BY country, release_year ORDER BY country, release_year;
```

Q. 9 -> SELECT release_year
FROM films
GROUP BY release_year
HAVING COUNT(title) > 10;
shows only those years in which more than 10 films were released.

In how many different years were more than 200 movies released?

```sql
SELECT release_year
FROM films
GROUP BY release_year
HAVING COUNT(title) > 200;
```

Q. 10 -> Get the release year, budget and gross earnings for each film in the films table.

Modify your query so that only records with a release_year after 1990 are included.

Remove the budget and gross columns, and group your results by release year.

Modify your query to include the average budget and average gross earnings for the results you have so far. Alias the average budget as avg_budget; alias the average gross earnings as avg_gross.

Modify your query so that only years with an average budget of greater than $60 million are included.

Finally, modify your query to order the results from highest average gross earnings to lowest.

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

Q. 11 -> Get the country, average budget, and average gross take of countries that have made more than 10 films. Order the result by country name, and limit the number of results displayed to 5. You should alias the averages as avg_budget and avg_gross respectively.

```sql
SELECT country, AVG(budget) AS avg_budget, AVG(gross) AS avg_gross
 FROM films
 GROUP BY country
 HAVING COUNT(*) > 10
 ORDER BY country
 LIMIT 5;
```

Q. 12 -> Submit the code in the editor and inspect the results.

What is the IMDB score for the film To Kill a Mockingbird?

```sql
SELECT title, imdb_score
FROM films
JOIN reviews
ON films.id = reviews.film_id
WHERE title = 'To Kill a Mockingbird';

// imdb_score 8.4
```
