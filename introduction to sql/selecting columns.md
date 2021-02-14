Q.1 -> From looking at the tabs, who is the first person listed in the people table?

```sql
select * from people limit 1;
```

Q. 2 -> Notice the query result tab in the bottom right corner of your screen. This is where the results of your SQL queries will be displayed.

Run this query in the editor and check out the resulting table in the query result tab!

SELECT name FROM people;
Who is the second person listed in the query result?

```sql
select name from people where id = 2;
```

Q.3 -> If you submit the code to the right, you'll see that you get two types of errors.

SQL errors are shown below the editor. These are errors returned by the SQL engine. You should see:

syntax error at or near "'DataCamp <3 SQL'" LINE 2: 'DataCamp <3 SQL' ^

DataCamp errors are shown in the Instructions box. These will let you know in plain English where you went wrong in your code! You should see:

You need to add SELECT at the start of line 2!

```sql
select 'DataCamp <3 SQL' AS result;
```

Q. 4 -> Submit the query in the editor! Don't worry, you'll learn how it works soon.

Now change 'SQL' to 'SQL is' and click Submit!

Finally, change 'SQL is' to 'SQL is cool' and click Submit!

```sql
SELECT 'SQL is cool'
AS result;
```

# SQL Intro

it is used to interacting with relational database. where database is the collection of table where table is collection of rows and columns. each row represent an single entity and column represent fields of that entity and query is the request for the data whether it could be delete, modify, add etc.

in sql keywords are not case-sensitive. eg. -> SELECT, FROM. while semicolon (\(;\)) indicates the termination of query.

## KEYWORDS ->

### LIMIT -> used for certain the result.

### DISTINCT -> used for select unique values from column.

### COUNT -> used to get total number of entity in the table.

Q. 1 -> Select the title column from the films table.

Select the release_year column from the films table.

Select the name of each person in the people table.

```sql
SELECT title FROM films;

SELECT release_year FROM films;

SELECT name FROM people;
```

Q. 2 -> Get the title of every film from the films table.

Get the title and release year for every film.

Get the title, release year and country for every film.

Get all columns from the films table.

```sql
SELECT title, release_year FROM films;

SELECT title, release_year FROM films;

SELECT title, release_year, country FROM films;

SELECT * FROM films;
```

Q. 3 -> Get all the unique countries represented in the films table.

Get all the different film certifications from the films table.

Get the different types of film roles from the roles table.

```sql
SELECT DISTINCT country FROM films;

SELECT DISTINCT certification FROM films;

SELECT DISTINCT role FROM roles;
```

Q. 4 -> How many records are contained in the reviews table?

```sql
SELECT COUNT(*) FROM reviews;
```

Q. 5 -> Count the number of rows in the people table.

Count the number of (non-missing) birth dates in the people table.

Count the number of unique birth dates in the people table.

Count the number of unique languages in the films table.

Count the number of unique countries in the films table.

```sql
SELECT COUNT(*) FROM people;

SELECT COUNT(birthdate) FROM people;

SELECT COUNT(DISTINCT birthdate) FROM people;

SELECT COUNT(DISTINCT language) FROM films;

SELECT COUNT(DISTINCT country) FROM films;
```
