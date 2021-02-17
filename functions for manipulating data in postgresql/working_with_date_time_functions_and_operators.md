AGE -> for difference between two timestamp.

Q. 1 -> Subtract the rental_date from the return_date to calculate the number of days_rented.

Now use the AGE() function to calculate the days_rented.

```sql
SELECT f.title, f.rental_duration,
r.return_date - r.rental_date AS days_rented
FROM film AS f
INNER JOIN inventory AS i ON f.film_id = i.film_id
INNER JOIN rental AS r ON i.inventory_id = r.inventory_id
ORDER BY f.title;

SELECT f.title, f.rental_duration,
AGE(r.return_date, r.rental_date) AS days_rented
FROM film AS f
INNER JOIN inventory AS i ON f.film_id = i.film_id
INNER JOIN rental AS r ON i.inventory_id = r.inventory_id
ORDER BY f.title;
```

Q. 2 -> Convert rental_duration by multiplying it with a 1 day INTERVAL
Subtract the rental_date from the return_date to calculate the number of days_rented.
Exclude rentals with a NULL value for return_date.

```sql
SELECT
f.title,
INTERVAL '1' day * f.rental_duration,
r.return_date - r.rental_date AS days_rented
FROM film AS f
INNER JOIN inventory AS i ON f.film_id = i.film_id
INNER JOIN rental AS r ON i.inventory_id = r.inventory_id
WHERE r.return_date IS NOT NULL
ORDER BY f.title;
```

Q. 3 -> Convert rental_duration by multiplying it with a 1-day INTERVAL.
Add it to the rental date.

```sql
SELECT
f.title,
r.rental_date,
f.rental_duration,
INTERVAL '1' day * f.rental_duration + r.rental_date AS expected_return_date,
r.return_date
FROM film AS f
INNER JOIN inventory AS i ON f.film_id = i.film_id
INNER JOIN rental AS r ON i.inventory_id = r.inventory_id
ORDER BY f.title;
```

Q. 4 -> Use the console to explore the NOW(), CURRENT_TIMESTAMP, CURRENT_DATE and CURRENT_TIME functions and their outputs to determine which of the following is NOT correct?

```sql
SELECT NOW(), CURRENT_TIMESTAMP, CURRENT_DATE, CURRENT_TIME;
```

ans. CURRENT_TIMESTAMP returns the current timestamp without timezone.

Q. 5 -> Use NOW() to select the current timestamp with timezone.

Select the current date without any time value.

Now, let's use the CAST() function to eliminate the timezone from the current timestamp.

Finally, let's select the current date
Use CAST() to retrieve the same result from the NOW() function.

```sql
SELECT NOW();

SELECT CURRENT_DATE;

SELECT CAST( NOW() AS TIMESTAMP );

SELECT
CURRENT_DATE,
CAST( NOW() AS date );
```

Q. 6 -> Select the current timestamp without timezone and alias it as right_now

Now select a timestamp five days from now and alias it as five_days_from_now.

Finally, let's use a second-level precision with no fractional digits for both the right_now and five_days_from_now fields.

```sql
SELECT CURRENT_TIMESTAMP::TIMESTAMP AS right_now;

SELECT
CURRENT_TIMESTAMP::timestamp AS right_now,
INTERVAL '5 day' + CURRENT_TIMESTAMP AS five_days_from_now;

SELECT
CURRENT_TIMESTAMP(0)::timestamp AS right_now,
interval '5 days' + CURRENT_TIMESTAMP(0) AS five_days_from_now;
```

Q. 7 -> Get the day of the week from the rental_date column.

Count the total number of rentals by day of the week.

```sql
SELECT
EXTRACT(dow FROM rental_date) AS dayofweek
FROM rental
LIMIT 100;

SELECT
EXTRACT(dow FROM rental_date) AS dayofweek,
COUNT(rental_date) as rentals
FROM rental
GROUP BY 1;
```

Q. 8 -> Truncate the rental_date field by year.

Now modify the previous query to truncate the rental_date by month.

Let's see what happens when we truncate by day of the month.

Finally, count the total number of rentals by rental_day and alias it as rentals.

```sql
SELECT DATE_TRUNC('year', rental_date) AS rental_year
FROM rental;

SELECT DATE_TRUNC('month', rental_date) AS rental_month
FROM rental;

SELECT DATE_TRUNC('day', rental_date) AS rental_day
FROM rental;

SELECT
DATE_TRUNC('day', rental_date) AS rental_day,
COUNT(rental_date) AS rentals
FROM rental
GROUP BY 1;
```

Q. 9 -> Extract the day of the week from the rental_date column using the alias dayofweek.
Use an INTERVAL in the WHERE clause to select records for the 90 day period starting on 5/1/2005.

Finally, use a CASE statement and DATE_TRUNC() to create a new column called past_due which will be TRUE if the rental_days is greater than the rental_duration otherwise, it will be FALSE.

```sql
SELECT
EXTRACT(DOW FROM rental_date) AS dayofweek,
AGE(return_date, rental_date) AS rental_days
FROM rental AS r
WHERE
rental_date BETWEEN CAST('2005-05-01' AS TIMESTAMP)
AND CAST('2005-05-01' AS TIMESTAMP) + INTERVAL '90 day';

SELECT
c.first_name || ' ' || c.last_name AS customer_name,
f.title,
r.rental_date,
EXTRACT(dow FROM r.rental_date) AS dayofweek,
AGE(r.return_date, r.rental_date) AS rental_days,
CASE WHEN DATE_TRUNC('day', AGE(r.return_date, r.rental_date)) >
f.rental_duration * INTERVAL '1' day
THEN TRUE
ELSE FALSE END AS past_due
FROM
film AS f
INNER JOIN inventory AS i
ON f.film_id = i.film_id
INNER JOIN rental AS r
ON i.inventory_id = r.inventory_id
INNER JOIN customer AS c
ON c.customer_id = r.customer_id
WHERE
r.rental_date BETWEEN CAST('2005-05-01' AS DATE)
AND CAST('2005-05-01' AS DATE) + INTERVAL '90 day';
```
