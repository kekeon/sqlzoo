### The JOIN operation

1. Modify it to show the matchid and player name for all goals scored by Germany. To identify German players, check for: teamid = 'GER'.

```sql

SELECT matchid, player
FROM goal
WHERE teamid = 'GER'

```

2. Notice in the that the column matchid in the goal table corresponds to the id column in the game table. We can look up information about game 1012 by finding that row in the game table.

Show id, stadium, team1, team2 for just game 1012.

```sql

SELECT id, stadium, team1, team2
FROM game
WHERE id = 1012

```

3. The code below shows the player (from the goal) and stadium name (from the game table) for every goal scored.

    Modify it to show the player, teamid, stadium and mdate for every German goal..

```sql

SELECT player, teamid, stadium, mdate
FROM game
	JOIN goal ON id = matchid
WHERE teamid = 'GER'

```

4. Use the same JOIN as in the previous question.

    Show the team1, team2 and player for every goal scored by a player called Mario player LIKE 'Mario%'

```sql

SELECT team1, team2, player
FROM game
	JOIN goal ON id = matchid
WHERE player LIKE 'Mario%'

```

5. The table eteam gives details of every national team including the coach. You can JOIN goal to eteam using the phrase goal JOIN eteam on teamid=id.

    Show player, teamid, coach, gtime for all goals scored in the first 10 minutes gtime<=10

```sql

SELECT player, teamid, coach, gtime
FROM goal
	JOIN eteam ON teamid = id
WHERE gtime <= 10

```

6. To JOIN game with eteam you could use either
game JOIN eteam ON (team1=eteam.id) or game JOIN eteam ON (team2=eteam.id)

    Notice that because id is a column name in both game and eteam you must specify eteam.id instead of just id

    List the the dates of the matches and the name of the team in which 'Fernando Santos' was the team1 coach.

```sql

SELECT mdate, teamname
FROM game JOIN eteam e ON team1 = e.id
WHERE e.coach = 'Fernando Santos'

```

7. List the player for every goal scored in a game where the stadium was 'National Stadium, Warsaw'.

```sql

SELECT player
FROM game JOIN goal ON id = matchid
WHERE stadium = 'National Stadium, Warsaw'

```

8. The example query shows all goals scored in the Germany-Greece quarterfinal.
    Instead show the name of all players who scored a goal against Germany.

    Select goals scored only by non-German players in matches where GER was the id of either team1 or team2.

    You can use teamid!='GER' to prevent listing German players.

    You can use DISTINCT to stop players being listed twice.

```sql

SELECT DISTINCT player
FROM goal a
	INNER JOIN game b
	ON a.matchid = b.id
		AND teamid != 'GER'
		AND (b.team1 = 'GER'
			OR b.team2 = 'GER')

```

9. Show teamname and the total number of goals scored.

```sql

SELECT teamname, COUNT(teamid)
  FROM eteam JOIN goal ON id=teamid
  GROUP BY teamname

```

10. Show the stadium and the number of goals scored in each stadium.

```sql

SELECT stadium, COUNT(teamid)
FROM game
	JOIN goal ON id = matchid
GROUP BY stadium

```

11. Show the stadium and the number of goals scored in each stadium.

```sql

SELECT matchid, mdate, COUNT(matchid)
FROM game g
	JOIN goal ON id = matchid
WHERE g.team1 = 'POL'
	OR g.team2 = 'POL'
GROUP BY matchid, mdate

```

12. For every match where 'GER' scored, show matchid, match date and the number of goals scored by 'GER'.

```sql

SELECT matchid, mdate, COUNT(teamid)
FROM game g
	JOIN goal
	ON id = matchid
		AND teamid = 'GER'
GROUP BY matchid, mdate

```

13. List every match with the goals scored by each team as shown. This will use "CASE WHEN" which has not been explained in any previous exercises.

```sql

SELECT mdate, team1, SUM(CASE 
		WHEN teamid = team1 THEN 1
		ELSE 0
	END) AS score1, team2
	, SUM(CASE 
		WHEN teamid = team2 THEN 1
		ELSE 0
	END) AS score2
FROM game
	LEFT OUTER  JOIN goal ON matchid = id
GROUP BY mdate, team1, team2

```
