# Filtering IN SQL

## KEYWORDS ->

### AND -> used for multiple conditions.

### OR -> used for match some conditions.

### BETWEEN -> provide range between values inclusively(start and end value include also).

### IN -> used to replace multiple OR conditions.

### LIKE AND NOT LIKE -> search pattern.

## EXPRESSIONS ->

### IS NULL -> to verify whether column is empty.

### IS NOT NULL -> to verify whether column is not empty.

## WILD CARDS ->

### \* -> select all the text.

### % -> match zero, one or many character.

### \_ -> match single character.

WHERE Clause allow to perform filteration. here are some listed operators that used with WHERE Clause ->

1. =
2. <> or !=
3. <
4. >
5. <=
6. > =

Q. What does NULL represent?

ans. A missing value.

Q. What does the BETWEEN keyword do?

ans. Filter values in a specified range.

Q. What does the OR operator do?

ans. Display only rows that meet at least one of the specified conditions.
press

Q. 1 -> What does the following query return?
SELECT title
FROM films
WHERE release_year > 2000;

ans. Film released after 2000

Q. 2 -> Get all details for all films released in 2016.

Get the number of films released before 2000.

Get the title and release year of films released after 2000.

```sql
SELECT * FROM films WHERE release_year = 2016;

SELECT COUNT(*) FROM films WHERE release_year < 2000;

SELECT title, release_year FROM films WHERE release_year > 2000;

```

Q. 3 -> Get all details for all French language films.

Get the name and birth date of the person born on November 11th, 1974. Remember to use ISO date format ('1974-11-11')!

Get the number of Hindi language films.

Get all details for all films with an R certification.

```sql
SELECT * FROM films WHERE language = 'French';

SELECT name, birthdate FROM people where birthdate = '1974-11-11';

SELECT COUNT(*) FROM films WHERE language = 'Hindi';

SELECT * FROM films WHERE certification = 'R';
```

Q. 4 -> Get the title and release year for all Spanish language films released before 2000.

Get all details for Spanish language films released after 2000.

Get all details for Spanish language films released after 2000, but before 2010.

```sql
SELECT title,release_year FROM films WHERE language = 'Spanish' AND release_year < 2000;

SELECT * FROM films WHERE language = 'Spanish' AND release_year > 2000;

SELECT * FROM films WHERE language = 'Spanish' AND release_year > 2000 AND release_year < 2010;
```

Q. 5 -> Get the title and release year for films released in the 90s.

```sql
SELECT title, release_year FROM films
WHERE
release_year >= 1990 AND release_year < 2000
AND(language = 'French' OR language = 'Spanish')
AND gross > 2000000;
;

```

Q. 6 -> Get the title and release year of all films released between 1990 and 2000 (inclusive).

Now, build on your previous query to select only films that have budgets over $100 million.

Now restrict the query to only return Spanish language films.

Finally, modify to your previous query to include all Spanish language or French language films with the same criteria as before. Don't forget your parentheses!

```sql
SELECT title, release_year
FROM films
WHERE release_year BETWEEN 1990 AND 2000
AND budget > 100000000
AND (language = 'Spanish' OR  language = 'French');
```

Q. 7 -> Get the title and release year of all films released in 1990 or 2000 that were longer than two hours. Remember, duration is in minutes!

Get the title and language of all films which were in English, Spanish, or French.

Get the title and certification of all films with an NC-17 or R certification.

```sql
SELECT title, release_year FROM films WHERE release_year IN (1990, 2000) AND duration > 120;

SELECT title, language FROM films WHERE language IN('English','Spanish','French');

SELECT title, certification FROM films WHERE certification IN('NC-17','R');

```

Q. 8 -> Get the names of people who are still alive, i.e. whose death date is missing.

Get the title of every film which doesn't have a budget associated with it.

Get the number of films which don't have a language associated with them.

```sql
SELECT name FROM people WHERE deathdate IS NULL;

SELECT title from films WHERE budget IS NULL;

SELECT COUNT(*) FROM films WHERE language IS NULL;

```

Q. 9 -> Get the names of all people whose names begin with 'B'. The pattern you need is 'B%'.

Get the names of people whose names have 'r' as the second letter. The pattern you need is '\_r%'.

Get the names of people whose names don't start with A. The pattern you need is 'A%'.

```sql
SELECT name FROM people WHERE name LIKE 'B%';

SELECT name FROM people WHERE name LIKE '_r%';

SELECT name FROM people WHERE name NOT LIKE 'A%';
```
