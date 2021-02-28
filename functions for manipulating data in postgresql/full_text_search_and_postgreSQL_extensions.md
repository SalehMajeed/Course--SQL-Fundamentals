Q. 1 -> Select all columns for all records that begin with the word GOLD.

Now select all records that end with the word GOLD.

Finally, select all records that contain the word 'GOLD'.

```sql
SELECT *
FROM film
WHERE title LIKE 'GOLD%';

SELECT *
FROM film
WHERE title LIKE '%GOLD';

SELECT *
FROM film
WHERE title LIKE '%GOLD%';
```

Q. 2 -> Select the film description and convert it to a tsvector data type.

```sql
SELECT to_tsvector(description)
FROM film;
```

Q. 3 -> Select the title and description columns from the film table.
Perform a full-text search on the title column for the word elf.

```sql
SELECT title, description
FROM film
WHERE to_tsvector(title) @@ to_tsquery('elf');
```

Q. 4 -> Create a new enumerated data type called compass_position.
Use the four positions of a compass as the values.

Verify that the new data type has been created by looking in the pg_type system table.

```sql
CREATE TYPE compass_position AS ENUM (
  	'North',
  	'South',
  	'East',
  	'West'
);

CREATE TYPE compass_position AS ENUM (
  	'North',
  	'South',
  	'East',
  	'West'
);

SELECT *
FROM pg_type
WHERE typname='compass_position';
```

Q. 5 -> Select the column_name, data_type, udt_name.
Filter for the rating column in the film table.

Select all columns from the pg_type table where the type name is equal to mpaa_rating.

```sql
SELECT column_name, data_type, udt_name
FROM INFORMATION_SCHEMA.COLUMNS
WHERE table_name ='film' AND column_name='rating';

SELECT *
FROM pg_type
WHERE typname ='mpaa_rating'
```

Q. 6 -> Select the title and inventory_id columns from the film and inventory tables in the database.

inventory_id is currently held by a customer and alias the column as held_by_cust.

Now filter your query to only return records where the inventory_held_by_customer() function returns a non-null value.

```sql
SELECT
f.title,
i.inventory_id
FROM film AS f
INNER JOIN inventory AS i ON f.film_id=i.film_id;

SELECT
f.title,
i.inventory_id,
inventory_held_by_customer(i.inventory_id) AS held_by_cust
FROM film as f
INNER JOIN inventory AS i ON f.film_id=i.film_id

SELECT
f.title,
i.inventory_id,
inventory_held_by_customer(i.inventory_id) as held_by_cust
FROM film as f
INNER JOIN inventory AS i ON f.film_id=i.film_id
WHERE
inventory_held_by_customer(i.inventory_id) IS NOT NULL;
```

Q. 7 -> Enable the pg_trgm extension

Now confirm that both fuzzystrmatch and pg_trgm are enabled by selecting all rows from the appropriate system table.

```sql
CREATE EXTENSION IF NOT EXISTS pg_trgm;

SELECT *
FROM pg_extension;

```

Q. 8 -> Select the film title and description.

Calculate the similarity between the title and description.

```sql
SELECT
title,
description
FROM film;

SELECT
title,
description,
SIMILARITY(title, description)
FROM film;
```

Q. 9 -> Select the film title and film description.
Calculate the levenshtein distance for the film title with the string JET NEIGHBOR.

```sql
SELECT
title,
description,
levenshtein(title,'JET NEIGHBOR') AS distance
FROM film
ORDER BY 3;
```

Q. 10 -> Select the title and description for all DVDs from the film table.
Perform a full-text search by converting the description to a tsvector and match it to the phrase 'Astounding & Drama' using a tsquery in the WHERE clause.

Add a new column that calculates the similarity of the description with the phrase 'Astounding Drama'.
Sort the results by the new similarity column in descending order.

```sql
SELECT
title,
description
FROM film
WHERE
to_tsvector(description) @@
to_tsquery('Astounding & Drama');

SELECT
title,
description,
SIMILARITY(description, 'Astounding Drama')
FROM film
WHERE
to_tsvector(description) @@
to_tsquery('Astounding & Drama')
ORDER BY
SIMILARITY(title, description) DESC;
```
