Q 1. -> Calculate triple the average home + away goals scored across all matches. This will become your subquery in the next step. Note that this column does not have an alias, so it will be called ?column? in your results.

Select the date, home goals, and away goals in the main query.
Filter the main query for matches where the total goals scored exceed the value in the subquery.

```sql
SELECT
3 * AVG(home_goal + away_goal)
FROM matches_2013_2014;

SELECT
date,
home_goal,
away_goal
FROM  matches_2013_2014
WHERE (home_goal + away_goal) >
(SELECT 3 * AVG(home_goal + away_goal)
FROM matches_2013_2014);
```

Q 2. -> Create a subquery in the WHERE clause that retrieves all unique hometeam_ID values from the match table.
Select the team_long_name and team_short_name from the team table. Exclude all values from the subquery in the main query.

```sql
SELECT
team_long_name,
team_short_name
FROM team
WHERE team_api_id NOT IN
(SELECT DISTINCT hometeam_ID  FROM match);
```

Q 3. -> Create a subquery in WHERE clause that retrieves all hometeam_ID values from match with a home_goal score greater than or equal to 8.
Select the team_long_name and team_short_name from the team table. Include all values from the subquery in the main query.

```sql
SELECT
team_long_name,
team_short_name
FROM team
WHERE team_api_id IN
(SELECT hometeam_ID
FROM match
WHERE home_goal >= 8);
```

Q 4. -> Create the subquery to be used in the next step, which selects the country ID and match ID (id) from the match table.
Filter the query for matches with greater than or equal to 10 goals.

Construct a subquery that selects only matches with 10 or more total goals.
Inner join the subquery onto country in the main query.
Select name from country and count the id column from match.

```sql
SELECT
country_id,
id
FROM match
WHERE (home_goal + away_goal) >= 10;

SELECT
c.name AS country_name,
COUNT(sub.id) AS matches
FROM country AS c
INNER JOIN (SELECT country_id, id
FROM match
WHERE (home_goal + away_goal) >= 10) AS sub
ON c.id = sub.country_id
GROUP BY country_name;
```

Q 5. -> Complete the subquery inside the FROM clause. Select the country name from the country table, and the home goal, and away goal columns from the match table.
Create a column in the subquery that adds home and away goals, called total_goals. This will be used to filter the main query.
Select the country, date, home goals, and away goals in the main query.
Filter the main query for games with 10 or more total goals.

```sql
SELECT
subq.country,
subq.date,
subq.home_goal,
subq.away_goal
FROM
(SELECT name AS country,
m.date,
m.home_goal,
m.away_goal,
(m.home_goal + m.away_goal) AS total_goals
FROM match AS m
LEFT JOIN country AS c
ON m.country_id = c.id) AS subq
WHERE total_goals >= 10;
```

Q 6. -> In the subquery, select the average total goals by adding home_goal and away_goal.
Filter the results so that only the average of goals in the 2013/2014 season is calculated.
In the main query, select the average total goals by adding home_goal and away_goal. This calculates the average goals for each league.
Filter the results in the main query the same way you filtered the subquery. Group the query by the league name.

```sql
SELECT
l.name AS league,
ROUND(AVG(m.home_goal + m.away_goal), 2) AS avg_goals,
(SELECT ROUND(AVG(home_goal + away_goal), 2)
FROM match
WHERE season = '2013/2014') AS overall_avg
FROM league AS l
LEFT JOIN match AS m
ON l.country_id = m.country_id
WHERE season = '2013/2014'
GROUP BY league;
```

Q 7. -> Select the average goals scored in a match for each league in the main query.
Select the average goals scored in a match overall for the 2013/2014 season in the subquery.
Subtract the subquery from the average number of goals calculated for each league.
Filter the main query so that only games from the 2013/2014 season are included.

```sql
SELECT
name AS league,
ROUND(AVG(m.home_goal + m.away_goal),2) AS avg_goals,
ROUND(AVG(m.home_goal + m.away_goal) -
(SELECT AVG(home_goal + away_goal)
FROM match
WHERE season = '2013/2014'),2) AS diff
FROM league AS l
LEFT JOIN match AS m
ON l.country_id = m.country_id
WHERE season = '2013/2014'
GROUP BY l.name;
```

Q 8. -> Extract the average number of home and away team goals in two SELECT subqueries.
Calculate the average home and away goals for the specific stage in the main query.
Filter both subqueries and the main query so that only data from the 2012/2013 season is included.
Group the query by the m.stage column.

```sql
SELECT
m.stage,
ROUND(AVG(m.home_goal + m.away_goal),2) AS avg_goals,
ROUND((SELECT AVG(home_goal + away_goal)
FROM match
WHERE season = '2012/2013'),2) AS overall
FROM match AS m
WHERE m.season = '2012/2013'
GROUP BY stage;
```

Q 9. -> Calculate the average home goals and average away goals from the match table for each stage in the FROM clause subquery.
Add a subquery to the WHERE clause that calculates the overall average home goals.
Filter the main query for stages where the average home goals is higher than the overall average.
Select the stage and avg_goals columns from the s subquery into the main query.

```sql
SELECT
s.stage,
ROUND(s.avg_goals,2) AS avg_goals
FROM
(SELECT
stage,
AVG(home_goal + away_goal) AS avg_goals
FROM match
WHERE season = '2012/2013'
GROUP BY stage) AS s
WHERE
s.avg_goals > (SELECT AVG(home_goal + away_goal)
FROM match WHERE season = '2012/2013');
```

Q 10. -> Create a subquery in SELECT that yields the average goals scored in the 2012/2013 season. Name the new column overall_avg.
Create a subquery in FROM that calculates the average goals scored in each stage during the 2012/2013 season.
Filter the main query for stages where the average goals exceeds the overall average in 2012/2013.

```sql
SELECT
s.stage,
ROUND(s.avg_goals,2) AS avg_goal,
(SELECT AVG(home_goal + away_goal) FROM match WHERE season = '2012/2013') AS overall_avg
FROM
(SELECT
stage,
AVG(home_goal + away_goal) AS avg_goals
FROM match
WHERE season = '2012/2013'
GROUP BY stage) AS s
WHERE
s.avg_goals > (SELECT AVG(home_goal + away_goal)
FROM match WHERE season = '2012/2013');
```
