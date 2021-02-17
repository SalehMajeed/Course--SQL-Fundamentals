TIMESTAMP -> time + date value. ISO 8601 Format.

INTERVAL -> day + time data in period or time.used for arithmetic day.

Q. 1 -> You learned about some of the common data types that you'll work within PostgreSQL, some characteristics of these types, and how to determine the data type of a column in an existing table. Think back to the video and answer the following question:

Which of the following is not a valid text data type in PostgreSQL?

ans. STRING.

Q. 2 -> Select all columns from the INFORMATION_SCHEMA.TABLES system database. Limit results that have a public table_schema.

Select all columns from the INFORMATION_SCHEMA.COLUMNS system database. Limit by table_name to actor

```sql
 SELECT *
 FROM INFORMATION_SCHEMA.TABLES
 WHERE table_schema = 'public';

  SELECT *
 FROM INFORMATION_SCHEMA.COLUMNS
 WHERE table_name = 'actor';
```

Q. 3 -> Select the column name and data type from the INFORMATION_SCHEMA.COLUMNS system database.
Limit results to only include the customer table.

```sql
SELECT
column_name,
data_type
FROM INFORMATION_SCHEMA.COLUMNS
WHERE table_name = 'customer';
```

Q. 4 -> Which of the following is NOT correct?

ans. TIME data types are stored with a timezone by default.

Q. 5 -> Select the rental date and return date from the rental table.
Add an INTERVAL of 3 days to the rental_date to calculate the expected return date`.

```sql
SELECT
rental_date,
return_date,
rental_date + INTERVAL '3 day' AS expected_return_date
FROM rental;
```

Q. 6 -> Select the title and special features from the film table and compare the results between the two columns.

Select all films that have a special feature Trailers by filtering on the first index of the special_features ARRAY.

Now let's select all films that have Deleted Scenes in the second index of the special_features ARRAY.

```sql
SELECT
title,
special_features
FROM film;

SELECT
title,
special_features
FROM film
WHERE special_features[1] = 'Trailers';

SELECT
title,
special_features
FROM film
WHERE special_features[2] = 'Deleted Scenes';
```

Q. 7 -> Match 'Trailers' in any index of the special_features ARRAY regardless of position.

```sql
SELECT
title,
special_features
FROM film
WHERE 'Trailers' = ANY (special_features);
```

Q. 8 -> Use the contains operator to match the text Deleted Scenes in the special_features column.

```sql
SELECT
title,
special_features
FROM film
WHERE special_features @> ARRAY['Deleted Scenes'];
```
