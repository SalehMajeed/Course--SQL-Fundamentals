PRECEDING -> before current row.
FOLLOWING -> after current row.

Q. 1 -> Return the athletes, the number of medals they earned, and the medals running total, ordered by the athletes' names in alphabetical order.

```sql
WITH Athlete_Medals AS (
SELECT
Athlete, COUNT(*) AS Medals
FROM Summer_Medals
WHERE
Country = 'USA' AND Medal = 'Gold'
AND Year >= 2000
GROUP BY Athlete)
SELECT
Athlete,
Medals,
SUM(Medals) OVER (ORDER BY Athlete ASC) AS Max_Medals
FROM Athlete_Medals
ORDER BY Athlete ASC;
```

Q. 2 -> Return the year, country, medals, and the maximum medals earned so far for each country, ordered by year in ascending order.

```sql
WITH Country_Medals AS (
SELECT
Year, Country, COUNT(*) AS Medals
FROM Summer_Medals
WHERE
Country IN ('CHN', 'KOR', 'JPN')
AND Medal = 'Gold' AND Year >= 2000
GROUP BY Year, Country)
SELECT
Country,
year,
Medals,
MAX(Medals) OVER (PARTITION BY country
ORDER BY year ASC) AS Max_Medals
FROM Country_Medals
ORDER BY Country ASC, Year ASC;
```

Q. 3 -> Return the year, medals earned, and minimum medals earned so far.

```sql
WITH France_Medals AS (
SELECT
Year, COUNT(*) AS Medals
FROM Summer_Medals
WHERE
Country = 'FRA'
AND Medal = 'Gold' AND Year >= 2000
GROUP BY Year)
SELECT
Year,
Medals,
MIN(Medals) OVER (ORDER BY Year ASC) AS Min_Medals
FROM France_Medals
ORDER BY Year ASC;
```

Q. 4 -> ROWS BETWEEN 3 PRECEDING AND 2 FOLLOWING

ans. 6

Q. 5 -> Return the year, medals earned, and the maximum medals earned, comparing only the current year and the next year.

```sql
WITH Scandinavian_Medals AS (
SELECT
Year, COUNT(*) AS Medals
FROM Summer_Medals
WHERE
Country IN ('DEN', 'NOR', 'FIN', 'SWE', 'ISL')
AND Medal = 'Gold'
GROUP BY Year)
SELECT
Year,
Medals,
MAX(Medals) OVER (ORDER BY Year ASC
ROWS BETWEEN CURRENT ROW
AND 1 FOLLOWING) AS Max_Medals
FROM Scandinavian_Medals
ORDER BY Year ASC;
```

Q. 6 -> Return the athletes, medals earned, and the maximum medals earned, comparing only the last two and current athletes, ordering by athletes' names in alphabetical order.

```sql
WITH Chinese_Medals AS (
SELECT
Athlete, COUNT(*) AS Medals
FROM Summer_Medals
WHERE
Country = 'CHN' AND Medal = 'Gold'
AND Year >= 2000
GROUP BY Athlete)
SELECT
Athlete,
Medals,
MAX(Medals) OVER (ORDER BY Athlete ASC
ROWS BETWEEN 2 PRECEDING
AND CURRENT ROW) AS Max_Medals
FROM Chinese_Medals
ORDER BY Athlete ASC;
```

Q. 7 -> If you want your moving average to cover the last 3 and current Olympic games, how would you define its frame?

ans. ROWS BETWEEN 3 PRECEDING AND CURRENT ROW.

Q. 8 -> Calculate the 3-year moving average of medals earned.

```sql
WITH Russian_Medals AS (
SELECT
Year, COUNT(*) AS Medals
FROM Summer_Medals
WHERE
Country = 'RUS'
AND Medal = 'Gold'
AND Year >= 1980
GROUP BY Year)
SELECT
Year, Medals,
AVG(Medals) OVER
(ORDER BY Year ASC
ROWS BETWEEN
2 PRECEDING AND CURRENT ROW) AS Medals_MA
FROM Russian_Medals
ORDER BY Year ASC;
```

Q. 9 -> Calculate the 3-year moving sum of medals earned per country.

```sql
WITH Country_Medals AS (
SELECT
Year, Country, COUNT(*) AS Medals
FROM Summer_Medals
GROUP BY Year, Country)
SELECT
Year, Country, Medals,
SUM(Medals) OVER
(PARTITION BY Country
ORDER BY Year ASC
ROWS BETWEEN
2 PRECEDING AND CURRENT ROW) AS Medals_MA
FROM Country_Medals
ORDER BY Country ASC, Year ASC;
```
