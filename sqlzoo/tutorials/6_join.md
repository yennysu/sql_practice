# The JOIN operation

### Source:
### https://sqlzoo.net/wiki/The_JOIN_operation

### Sample Data:
Table game


| id | mdate | stadium | team1 | team2 |
|-------------|-----------|---------|------------|--------------|
1001 |	8 June 2012	|National Stadium, Warsaw	|POL	|GRE
1002 |	8 June 2012	|Stadion Miejski (Wroclaw)	|RUS	|CZE

Table goal


| matchid | teamid | player | gtime |
|-------------|-----------|---------|------------|
1001	|POL	|Robert Lewandowski	|17
1001|GRE|	Dimitris Salpingidis	|51

Table eteam

| id | teamname | player |
|-|-|-|
POL |Poland|	Franciszek Smuda
RUS	|Russia|	Dick Advocaat
CZE	|Czech Republic|	Michal Bilek

## 1. Show the matchid and player name for all goals scored by Germany. To identify German players, check for: teamid = 'GER'

```sql
SELECT matchid, player
FROM goal
WHERE teamid = 'GER';
```

## 2. Show id, stadium, team1, team2 for just game 1012

```sql
SELECT id, stadium, team1, team2
FROM game
WHERE id = 1012;
```

## 3. Show the player, teamid, stadium and mdate for every German goal.

```sql
SELECT x.player, x.teamid, g.stadium, g.mdate
FROM game g
JOIN goal x
    ON g.id = x.matchid
WHERE x.teamid = 'GER';
```

## 4. Show the team1, team2 and player for every goal scored by a player called Mario player LIKE 'Mario%'

``` sql
SELECT game.team1, game.team2, goal.player
FROM game
JOIN goal
    ON game.id = goal.matchid
WHERE goal.player LIKE 'Mario%';

```

## 5. Show player, teamid, coach, gtime for all goals scored in the first 10 minutes gtime<=10

```sql
SELECT goal.player, goal.teamid, e.coach, goal.gtime
FROM goal
JOIN eteam e
    ON goal.teamid = e.id
WHERE goal.gtime <= 10;
```

## 6. List the the dates of the matches and the name of the team in which 'Fernando Santos' was the team1 coach.

```sql
SELECT g.mdate, e.teamname
FROM game g
JOIN eteam e
    ON g.team1 = e.id
WHERE e.coach = 'Fernando Santos';
```

## 7. List the player for every goal scored in a game where the stadium was 'National Stadium, Warsaw'

```sql
SELECT goal.player
FROM goal
JOIN game
    ON goal.matchid = game.id
WHERE game.stadium = 'National Stadium, Warsaw';
```

## 8. Show the name of all players who scored a goal against Germany

```sql
SELECT DISTINCT goal.player
FROM goal
JOIN game
    ON goal.matchid = game.id
WHERE (game.team1 = 'GER' OR game.team2 = 'GER')
    AND goal.teamid <> 'GER';
```

## 9. Show teamname and the total number of goals scored.

```sql
SELECT eteam.teamname, COUNT(goal.teamid)
FROM eteam
JOIN goal
    ON eteam.id = goal.teamid
GROUP BY eteam.teamname;
```

## 10. Show the stadium and the number of goals scored in each stadium.

```sql
SELECT game.stadium, COUNT(goal.teamid)
FROM game
JOIN goal
    ON game.id = goal.matchid
GROUP BY game.stadium;
```

## 11. For every match involving 'POL', show the matchid, date and the number of goals scored.

```sql
SELECT game.id, game.mdate, COUNT(goal.teamid)
FROM game
JOIN goal
    ON game.id = goal.matchid
WHERE game.team1 = 'POL' OR game.team2 = 'POL'
GROUP BY game.id, game.mdate;
```

## 12. For every match where 'GER' scored, show matchid, match date and the number of goals scored by 'GER'

```sql
SELECT game.id, game.mdate, COUNT(goal.teamid)
FROM game
JOIN goal
    ON game.id = goal.matchid
WHERE goal.teamid = 'GER'
GROUP BY game.id, game.mdate;
```

## 13. List every match with the goals scored by each team as shown. This will use "CASE WHEN" which has not been explained in any previous exercises. Sort your result by mdate, matchid, team1 and team2.

```sql
SELECT mdate,
       team1,
       SUM(CASE WHEN teamid=team1 THEN 1 ELSE 0 END) AS score1,
       team2,
       SUM(CASE WHEN teamid=team2 THEN 1 ELSE 0 END) AS score2
FROM game
JOIN goal
    ON game.id = goal.matchid
GROUP BY mdate, team1, team2, teamid
ORDER BY mdate, matchid, team1, team2;
```