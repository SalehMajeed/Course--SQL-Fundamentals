LEAD -> value at n'th row after current row.

FIRST_VALUE -> return first value of the table OR partition.

LAST_VALUE -> return last value of the table or partition.

NTILE(n) -> split data into n equal pages.

Q. 1 -> For each year, fetch the current gold medalist and the gold medalist 3 competitions ahead of the current row.

```sql
WITH Discus_Medalists AS (
SELECT DISTINCT
Year,
Athlete
FROM Summer_Medals
WHERE Medal = 'Gold'
AND Event = 'Discus Throw'
AND Gender = 'Women'
AND Year >= 2000)
SELECT
year,
Athlete,
LEAD(Athlete, 3) OVER (ORDER BY year ASC) AS Future_Champion
FROM Discus_Medalists
ORDER BY Year ASC;
```

Q. 2 -> Return all athletes and the first athlete ordered by alphabetical order.

```sql
WITH All_Male_Medalists AS (
SELECT DISTINCT
Athlete
FROM Summer_Medals
WHERE Medal = 'Gold'
AND Gender = 'Men')
SELECT
Athlete,
FIRST_VALUE(Athlete) OVER (
ORDER BY Athlete ASC
) AS First_Athlete
FROM All_Male_Medalists;
```

Q. 3 -> Return the year and the city in which each Olympic games were held.
Fetch the last city in which the Olympic games were held.

```sql
WITH Hosts AS (
SELECT DISTINCT Year, City
FROM Summer_Medals)
SELECT
Year,
City,
LAST_VALUE(City) OVER (
ORDER BY Year ASC
RANGE BETWEEN
UNBOUNDED PRECEDING AND
UNBOUNDED FOLLOWING
) AS Last_City
FROM Hosts
ORDER BY Year ASC;
```

Q. 4 -> Rank each athlete by the number of medals they've earned -- the higher the count, the higher the rank -- with identical numbers in case of identical values.

```sql
WITH Athlete_Medals AS (
SELECT
Athlete,
COUNT(*) AS Medals
FROM Summer_Medals
GROUP BY Athlete)
SELECT
Athlete,
Medals,
RANK() OVER (ORDER BY Medals DESC) AS Rank_N
FROM Athlete_Medals
ORDER BY Medals DESC;
```

Q. 5 -> Rank each country's athletes by the count of medals they've earned -- the higher the count, the higher the rank -- without skipping numbers in case of identical values.

```sql
WITH Athlete_Medals AS (
SELECT
Country, Athlete, COUNT(*) AS Medals
FROM Summer_Medals
WHERE
Country IN ('JPN', 'KOR')
AND Year >= 2000
GROUP BY Country, Athlete
HAVING COUNT(*) > 1)
SELECT
Country,
Athlete,
DENSE_RANK() OVER (PARTITION BY Country
ORDER BY Medals DESC) AS Rank_N
FROM Athlete_Medals
ORDER BY Country ASC, RANK_N ASC;
```

Q. 6 -> You have the following table:

```sql
| Country | Medals |
|---------|--------|
| IRN     | 23     |
| IRQ     | 19     |
| LBN     | 19     |
| SYR     | 19     |
| BHR     | 7      |
| KSA     | 3      |
```

If you were to use DENSE_RANK to order the Medals column in descending order, what rank would BHR be assigned?

ans. 3

Q. 7 -> Split the distinct events into exactly 111 groups, ordered by event in alphabetical order.

```sql
WITH Events AS (
SELECT DISTINCT Event
FROM Summer_Medals)
SELECT
Event,
NTILE(111) OVER (ORDER BY Event ASC) AS Page
FROM Events
ORDER BY Event ASC;
```

Q. 8 -> Split the athletes into top, middle, and bottom thirds based on their count of medals.

Return the average of each third.

```sql
WITH Athlete_Medals AS (
SELECT Athlete, COUNT(*) AS Medals
FROM Summer_Medals
GROUP BY Athlete
HAVING COUNT(*) > 1)
SELECT
Athlete,
Medals,
NTILE(3) OVER(ORDER BY Medals DESC) AS Third
FROM Athlete_Medals
ORDER BY Medals DESC, Athlete ASC;

WITH Athlete_Medals AS (
SELECT Athlete, COUNT(*) AS Medals
FROM Summer_Medals
GROUP BY Athlete
HAVING COUNT(*) > 1),
Thirds AS (
SELECT
Athlete,
Medals,
NTILE(3) OVER (ORDER BY Medals DESC) AS Third
FROM Athlete_Medals)
SELECT
Third,
AVG(Medals) AS Avg_Medals
FROM Thirds
GROUP BY Third
ORDER BY Third ASC;
```
