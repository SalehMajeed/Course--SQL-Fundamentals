Q. 1 -> Select the match ID, country name, season, home, and away goals from the match and country tables.
Complete the query that calculates the average number of goals scored overall and then includes the aggregate value in each row using a window function.

```sql
SELECT
m.id,
c.name AS country,
m.season,
m.home_goal,
m.away_goal,
AVG(m.home_goal +m.away_goal) OVER() AS overall_avg
FROM match AS m
LEFT JOIN country AS c ON m.country_id = c.id;
```

Q. 2 -> Select the league name and average total goals scored from league and match.
Complete the window function so it calculates the rank of average goals scored across all leagues in the database.
Order the rank by the average total of home and away goals scored.

```sql
SELECT
l.name AS league,
AVG(m.home_goal + m.away_goal) AS avg_goals,
RANK() OVER(ORDER BY AVG(m.home_goal + m.away_goal)) AS league_rank
FROM league AS l
LEFT JOIN match AS m
ON l.id = m.country_id
WHERE m.season = '2011/2012'
GROUP BY l.name
ORDER BY league_rank;
```

Q. 3 -> Complete the same parts of the query as the previous exercise.
Complete the window function to rank each league from highest to lowest average goals scored.
Order the main query by the rank you just created.

```sql
SELECT
l.name AS league,
AVG(m.home_goal + m.away_goal) AS avg_goals,
RANK() OVER(ORDER BY AVG(m.home_goal + m.away_goal) DESC) AS league_rank
FROM league AS l
LEFT JOIN match AS m
ON l.id = m.country_id
WHERE m.season = '2011/2012'
GROUP BY l.name
ORDER BY league_rank;
```

Q. 4 -> Complete the two window functions that calculate the home and away goal averages. Partition the window functions by season to calculate separate averages for each season.
Filter the query to only include matches played by Legia Warszawa, id = 8673.

```sql
SELECT
date,
season,
home_goal,
away_goal,
CASE WHEN hometeam_id = 8673 THEN 'home'
ELSE 'away' END AS warsaw_location,
AVG(home_goal) OVER(PARTITION BY season) AS season_homeavg,
AVG(away_goal) OVER(PARTITION BY season) AS season_awayavg
FROM match
WHERE
hometeam_id = 8673
OR awayteam_id = 8673
ORDER BY (home_goal + away_goal) DESC;
```

Q. 5 -> Construct two window functions partitioning the average of home and away goals by season and month.
Filter the data set by Legia Warszawa's team ID (8673) so that the window calculation excludes all teams who did not play against them.

```sql
SELECT
date,
season,
home_goal,
away_goal,
CASE WHEN hometeam_id = 8673 THEN 'home'
ELSE 'away' END AS warsaw_location,
AVG(home_goal) OVER(PARTITION BY season,
EXTRACT(MONTH FROM date)) AS season_mo_home,
AVG(away_goal) OVER(PARTITION BY season,
EXTRACT(MONTH FROM date)) AS season_mo_away
FROM match
WHERE
hometeam_id = 8673
OR awayteam_id = 8673
ORDER BY (home_goal + away_goal) DESC;
```

Q. 6 -> Complete the window function by:
Assessing the running total of home goals scored by FC Utrecht.
Assessing the running average of home goals scored.
Ordering both the running average and running total by date.

```sql
SELECT
date,
home_goal,
away_goal,
SUM(home_goal) OVER(ORDER BY date
ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_total,
AVG(home_goal) OVER(ORDER BY date
ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_avg
FROM match
WHERE
hometeam_id = 9908
AND season = '2011/2012';
```

Q. 7 -> Complete the window function by:
Assessing the running total of home goals scored by FC Utrecht.
Assessing the running average of home goals scored.
Ordering both the running average and running total by date, descending.

```sql
SELECT
date,
home_goal,
away_goal,
SUM(home_goal) OVER(ORDER BY date DESC
ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING) AS running_total,
AVG(home_goal) OVER(ORDER BY date DESC
ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING) AS running_avg
FROM match
WHERE
awayteam_id = 9908
AND season = '2011/2012';
```

Q. 8 -> Create a CASE statement that identifies each match as a win, lose, or tie for Manchester United.
Fill out the logical operators for each WHEN clause in the CASE statement (equals, greater than, less than).
Join the tables on home team ID from match, and team_api_id from team.
Filter the query to only include games from the 2014/2015 season where Manchester United was the home team.

```sql
SELECT
m.id,
t.team_long_name,
CASE WHEN m.home_goal > m.away_goal THEN 'MU Win'
WHEN m.home_goal < m.away_goal THEN 'MU Loss'
ELSE 'Tie' END AS outcome
FROM match AS m
LEFT JOIN team AS t
ON m.hometeam_id = t.team_api_id
WHERE
m.season = '2014/2015'
AND t.team_long_name = 'Manchester United';
```

Q. 9 -> Complete the CASE statement syntax.
Fill out the logical operators identifying each match as a win, loss, or tie for Manchester United.
Join the table on awayteam_id, and team_api_id.

```sql
SELECT
m.id,
t.team_long_name,
CASE WHEN m.home_goal > m.away_goal THEN 'MU Loss'
WHEN m.home_goal < m.away_goal THEN 'MU Win'
ELSE 'Tie' END AS outcome
FROM match AS m
LEFT JOIN team AS t
ON m.awayteam_id = t.team_api_id
WHERE
season = '2014/2015'
AND t.team_long_name = 'Manchester United';
```

Q. 10 -> Declare the home and away CTEs before your main query.
Join your CTEs to the match table using a LEFT JOIN.
Select the relevant data from the CTEs into the main query.
Select the date from match, team names from the CTEs, and home/ away goals from match in the main query.

```sql
WITH home AS (
SELECT m.id, t.team_long_name,
CASE WHEN m.home_goal > m.away_goal THEN 'MU Win'
WHEN m.home_goal < m.away_goal THEN 'MU Loss'
ELSE 'Tie' END AS outcome
FROM match AS m
LEFT JOIN team AS t ON m.hometeam_id = t.team_api_id),
away AS (
SELECT m.id, t.team_long_name,
CASE WHEN m.home_goal > m.away_goal THEN 'MU Win'
WHEN m.home_goal < m.away_goal THEN 'MU Loss'
ELSE 'Tie' END AS outcome
FROM match AS m
LEFT JOIN team AS t ON m.awayteam_id = t.team_api_id)
SELECT DISTINCT
m.date,
home.team_long_name AS home_team,
away.team_long_name AS away_team,
m.home_goal,
m.away_goal
FROM match AS m
LEFT JOIN home ON m.id = home.id
LEFT JOIN away ON m.id = away.id
WHERE m.season = '2014/2015'
AND (home.team_long_name = 'Manchester United'
OR away.team_long_name = 'Manchester United');
```

Q. 11 -> Set up the CTEs so that the home and away teams each have a name, ID, and score associated with them.
Select the date, home team name, away team name, home goal, and away goals scored in the main query.
Rank the matches and order by the difference in scores in descending order.

```sql
WITH home AS (
SELECT m.id, t.team_long_name,
CASE WHEN m.home_goal > m.away_goal THEN 'MU Win'
WHEN m.home_goal < m.away_goal THEN 'MU Loss'
ELSE 'Tie' END AS outcome
FROM match AS m
LEFT JOIN team AS t ON m.hometeam_id = t.team_api_id),
away AS (
SELECT m.id, t.team_long_name,
CASE WHEN m.home_goal > m.away_goal THEN 'MU Loss'
WHEN m.home_goal < m.away_goal THEN 'MU Win'
ELSE 'Tie' END AS outcome
FROM match AS m
LEFT JOIN team AS t ON m.awayteam_id = t.team_api_id)
SELECT DISTINCT
m.date,
home.team_long_name AS home_team,
away.team_long_name AS away_team,
m.home_goal, m.away_goal,
RANK() OVER(ORDER BY ABS(home_goal - away_goal) DESC) as match_rank
FROM match AS m
LEFT JOIN home ON m.id = home.id
LEFT JOIN away ON m.id = away.id
WHERE m.season = '2014/2015'
AND ((home.team_long_name = 'Manchester United' AND home.outcome = 'MU Loss')
OR (away.team_long_name = 'Manchester United' AND away.outcome = 'MU Loss'));
```
