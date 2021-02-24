Q. 1 -> Concatenate the first_name and last_name columns separated by a single space followed by email surrounded by < and >.

Now use the CONCAT() function to do the same operation as the previous step.

```sql
SELECT first_name || ' ' || last_name || ' <' || email || '>' AS full_email FROM customer;

SELECT CONCAT(first_name, ' ' , last_name, ' <', email, '>') AS full_email FROM customer
```

Q. 2 -> Convert the film category name to uppercase.
Convert the first letter of each word in the film's title to upper case.
Concatenate the converted category name and film title separated by a colon.
Convert the description column to lowercase.

```sql
SELECT
UPPER(name)  || ': ' || INITCAP(title) AS film_category,
LOWER(description) AS description
FROM
film AS f
INNER JOIN film_category AS fc
ON f.film_id = fc.film_id
INNER JOIN category AS c
ON fc.category_id = c.category_id;
```

Q. 3 -> Replace all whitespace with an underscore.

```sql
SELECT
REPLACE(title, ' ', '_') AS title
FROM film;
```

Q. 4 -> Select the title and description columns from the film table.
Find the number of characters in the description column with the alias desc_len.

```sql
SELECT
title,
description,
LENGTH(description) AS desc_len
FROM film;
```

Q. 5 -> Select the first 50 characters of the description column with the alias short_desc

```sql
SELECT
LEFT(description, 50) AS short_desc
FROM
film AS f;
```

Q. 6 -> Extract only the street address without the street number from the address column.
Use functions to determine the starting and ending position parameters.

```sql
SELECT
SUBSTRING(address FROM POSITION(' ' IN address)+1 FOR LENGTH(address))
FROM
address;
```

Q. 7 -> Extract the characters to the left of the @ of the email column in the customer table and alias it as username.
Now use SUBSTRING to extract the characters after the @ of the email column and alias the new derived field as domain.

```sql
SELECT
LEFT(email, POSITION('@' IN email)-1) AS username,
SUBSTRING(email FROM POSITION('@' IN email)+1 FOR LENGTH(email)) AS domain
FROM customer;
```

Q. 8 -> Add a single space to the end or right of the first_name column using a padding function.
Use the || operator to concatenate the padded first_name to the last_name column.

Now add a single space to the left or beginning of the last_name column using a different padding function than the first step.
Use the || operator to concatenate the first_name column to the padded last_name.

Add a single space to the right or end of the first_name column.
Add the characters < to the right or end of last_name column.
Finally, add the characters > to the right or end of the email column.

```sql
SELECT
RPAD(first_name, LENGTH(first_name)+1) || last_name AS full_name
FROM customer;

SELECT
first_name || LPAD(last_name, LENGTH(last_name)+1) AS full_name
FROM customer;

SELECT
RPAD(first_name, LENGTH(first_name)+1)
|| RPAD(last_name, LENGTH(last_name)+2, ' <')
|| RPAD(email, LENGTH(email)+1, '>') AS full_email
FROM customer;
```

Q. 9 -> Convert the film category name to uppercase and use the CONCAT() concatenate it with the title.
Truncate the description to the first 50 characters and make sure there is no leading or trailing whitespace after truncating.

```sql
SELECT
CONCAT(UPPER(c.name), ': ', f.title) AS film_category,
TRIM(LEFT(description, 50)) AS film_desc
FROM
film AS f
INNER JOIN film_category AS fc
ON f.film_id = fc.film_id
INNER JOIN category AS c
ON fc.category_id = c.category_id;
```

Q. 10 -> Get the first 50 characters of the description column
Determine the position of the last whitespace character of the truncated description column and subtract it from the number 50 as the second parameter in the first function above.

```sql
SELECT
UPPER(c.name) || ': ' || f.title AS film_category,
LEFT(description, 50 -
POSITION(
' ' IN REVERSE(LEFT(description, 50))
)
)
FROM
film AS f
INNER JOIN film_category AS fc
ON f.film_id = fc.film_id
INNER JOIN category AS c
ON fc.category_id = c.category_id;
```
