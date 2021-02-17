ROLLUP -> GROUP BY subclass includes extra row for group-level aggregation.

CUBE -> Same as ROLLUP BUT not hierarchical.

COALESCE -> takes value and return first non-null value.

STRING_AGG -> take all column value and concat them with seperator.

Q. 1 -> Create the correct extension.
Fill in the column names of the pivoted table.

```sql
CREATE EXTENSION IF NOT EXISTS tablefunc;
SELECT * FROM CROSSTAB($$
SELECT
Gender, Year, Country
FROM Summer_Medals
WHERE
Year IN (2008, 2012)
AND Medal = 'Gold'
AND Event = 'Pole Vault'
ORDER By Gender ASC, Year ASC;
$$) AS ct (Gender VARCHAR,
"2008" VARCHAR,
"2012" VARCHAR)
ORDER BY Gender ASC;
```

Q. 2 -> Count the gold medals that France (FRA), the UK (GBR), and Germany (GER) have earned per country and year.

Select the country and year columns, then rank the three countries by how many gold medals they earned per year.

Pivot the query's results by Year by filling in the new table's correct column names.

```sql
SELECT
Country,
Year,
COUNT(*) AS Awards
FROM Summer_Medals
WHERE
Country IN ('FRA', 'GBR', 'GER')
AND Year IN (2004, 2008, 2012)
AND Medal = 'Gold'
GROUP BY Country, Year
ORDER BY Country ASC, Year ASC;

WITH Country_Awards AS (
SELECT
Country,
Year,
COUNT(*) AS Awards
FROM Summer_Medals
WHERE
Country IN ('FRA', 'GBR', 'GER')
AND Year IN (2004, 2008, 2012)
AND Medal = 'Gold'
GROUP BY Country, Year)
SELECT
Country,
Year,
Year :: INTEGER AS rank
FROM Country_Awards
ORDER BY Country ASC, Year ASC;

CREATE EXTENSION IF NOT EXISTS tablefunc;
SELECT * FROM CROSSTAB($$
WITH Country_Awards AS (
SELECT
Country,
Year,
COUNT(*) AS Awards
FROM Summer_Medals
WHERE
Country IN ('FRA', 'GBR', 'GER')
AND Year IN (2004, 2008, 2012)
AND Medal = 'Gold'
GROUP BY Country, Year)
SELECT
Country,
Year,
RANK() OVER(
PARTITION BY Year
ORDER BY Awards DESC) :: INTEGER AS rank
FROM Country_Awards
ORDER BY Country ASC, Year ASC;
$$) AS ct (Country VARCHAR,
"2004" INTEGER,
"2008" INTEGER,
"2012" INTEGER)
Order by Country ASC;
```

Q. 3 -> Count the gold medals awarded per country and gender.
Generate Country-level gold award counts.

```sql
SELECT
Country,
Gender,
COUNT(*) AS Gold_Awards
FROM Summer_Medals
WHERE
Year = 2004
AND Medal = 'Gold'
AND Country IN ('DEN', 'NOR', 'SWE')
GROUP BY Country, ROLLUP(Gender)
ORDER BY Country ASC, Gender ASC;
```

Q. 4 -> Count the medals awarded per gender and medal type.
Generate all possible group-level counts (per gender and medal type subtotals and the grand total).

```sql
SELECT
Gender,
Medal,
count(*) AS Awards
FROM Summer_Medals
WHERE
Year = 2012
AND Country = 'RUS'
GROUP BY CUBE(Gender,Medal)
ORDER BY Gender ASC, Medal ASC;
```

Q. 5 -> Turn the nulls in the Country column to All countries, and the nulls in the Gender column to All genders.

```sql
SELECT
COALESCE(Country, 'All countries') AS Country,
COALESCE(Gender, 'All genders') AS Gender,
COUNT(*) AS Awards
FROM Summer_Medals
WHERE
Year = 2004
AND Medal = 'Gold'
AND Country IN ('DEN', 'NOR', 'SWE')
GROUP BY ROLLUP(Country, Gender)
ORDER BY Country ASC, Gender ASC;
```

Q. 6 -> Rank countries by the medals they've been awarded.

Return the top 3 countries by medals awarded as one comma-separated string.

```sql
WITH Country_Medals AS (
SELECT
Country,
COUNT(*) AS Medals
FROM Summer_Medals
WHERE Year = 2000
AND Medal = 'Gold'
GROUP BY Country)
SELECT
Country,
RANK() OVER(ORDER BY Medals DESC) AS Rank
FROM Country_Medals
ORDER BY Rank ASC;

WITH Country_Medals AS (
SELECT
Country,
COUNT(*) AS Medals
FROM Summer_Medals
WHERE Year = 2000
AND Medal = 'Gold'
GROUP BY Country),
Country_Ranks AS (
SELECT
Country,
RANK() OVER (ORDER BY Medals DESC) AS Rank
FROM Country_Medals
ORDER BY Rank ASC)
SELECT STRING_AGG(Country,', ')
FROM Country_Ranks
WHERE Rank <= 3;
```
