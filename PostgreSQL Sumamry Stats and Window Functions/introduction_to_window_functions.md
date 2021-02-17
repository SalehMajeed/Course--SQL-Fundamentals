ROW_NUMBER -> return row number.

OVER -> order, partition, rows/range, preceding/following/unbounded.

LAG -> used to get row before current.

Q. 1 -> Window functions vs GROUP BY
Which of the following is FALSE?

ans.
Window functions can open a "window" to another table, whereas GROUP BY functions cannot.

Q. 2 -> Number each row in the dataset.

```sql
SELECT
  *,
ROW_NUMBER() OVER() AS Row_N
FROM Summer_Medals
ORDER BY Row_N ASC;
```

Q. 3 -> Assign a number to each year in which Summer Olympic games were held.

```sql
SELECT
Year,
ROW_NUMBER() OVER() AS Row_N
FROM (
SELECT DISTINCT YEAR
FROM Summer_Medals
ORDER BY Year ASC
) AS Years
ORDER BY Year ASC;
```

Q. 4 -> Assign a number to each year in which Summer Olympic games were held so that rows with the most recent years have lower row numbers.

```sql
SELECT
Year,
ROW_NUMBER() OVER (ORDER BY year DESC) AS Row_N
FROM (
SELECT DISTINCT Year
FROM Summer_Medals
) AS Years
ORDER BY Year;
```

Q. 5 -> For each athlete, count the number of medals he or she has earned.

Having wrapped the previous query in the Athlete_Medals CTE, rank each athlete by the number of medals they've earned.

```sql
SELECT
athlete,
COUNT(athlete) AS Medals
FROM Summer_Medals
GROUP BY Athlete
ORDER BY Medals DESC;

WITH Athlete_Medals AS (
SELECT
Athlete,
COUNT(*) AS Medals
FROM Summer_Medals
GROUP BY Athlete)
SELECT
Athlete,
ROW_NUMBER() OVER (ORDER BY Athlete_Medals.Medals DESC) AS Row_N
FROM Athlete_Medals
ORDER BY Medals DESC;
```

Q. 6 -> Return each year's gold medalists in the Men's 69KG weightlifting competition.

Having wrapped the previous query in the Weightlifting_Gold CTE, get the previous year's champion for each year.

```sql
SELECT
year,
country AS champion
FROM Summer_Medals
WHERE
Discipline = 'Weightlifting' AND
Event = '69KG' AND
Gender = 'Men' AND
Medal = 'Gold';

WITH Weightlifting_Gold AS (
SELECT
Year,
Country AS champion
FROM Summer_Medals
WHERE
Discipline = 'Weightlifting' AND
Event = '69KG' AND
Gender = 'Men' AND
Medal = 'Gold')
SELECT
Year, Champion,
LAG(Champion,1) OVER
(ORDER BY champion ASC) AS Last_Champion
FROM Weightlifting_Gold
ORDER BY Year ASC;
```

Q. 7 -> Return the previous champions of each year's event by gender.

```sql
WITH Tennis_Gold AS (
SELECT DISTINCT
Gender, Year, Country
FROM Summer_Medals
WHERE
Year >= 2000 AND
Event = 'Javelin Throw' AND
Medal = 'Gold')
SELECT
Gender, Year,
Country AS Champion,
LAG(Country, 1) OVER (PARTITION BY Gender
ORDER BY Gender ASC) AS Last_Champion
FROM Tennis_Gold
ORDER BY Gender ASC, Year ASC;
```

Q. 8 -> Return the previous champions of each year's events by gender and event.

```sql
WITH Athletics_Gold AS (
SELECT DISTINCT
Gender, Year, Event, Country
FROM Summer_Medals
WHERE
Year >= 2000 AND
Discipline = 'Athletics' AND
Event IN ('100M', '10000M') AND
Medal = 'Gold')
SELECT
Gender, Year, Event,
Country AS Champion,
LAG(Country,1) OVER (PARTITION BY Gender, Event
ORDER BY Year ASC) AS Last_Champion
FROM Athletics_Gold
ORDER BY Event ASC, Gender ASC, Year ASC;
```

Q. 9 -> If you run ROW_NUMBER() OVER (PARTITION BY Year ORDER BY Medals DESC) on the following table, what row number would the 2008 Iranian record have?

```sql
| Year | Country | Medals |
|------|---------|--------|
| 2004 | IRN     | 32     |
| 2004 | LBN     | 17     |
| 2004 | KSA     | 4      |
| 2008 | IRQ     | 29     |
| 2008 | IRN     | 27     |
| 2008 | UAE     | 12     |
```

ans. 2
