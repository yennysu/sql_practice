# Window functions

### Source:
### https://sqlzoo.net/wiki/Window_functions

### Data:
<b>Table: ge</b>

| yr|firstName|lastName|constituency|party|votes|
|-|-|-|-|-|-|
2015|	Ian	|Murray|	S14000024|	Labour|	19293|
2015|	Neil	|Hay|	S14000024|	Scottish National Party	|16656|
2015|	Miles	|Briggs|	S14000024|	Conservative|	8626|
2015|	Phyl	|Meyer|	S14000024|	Green|	2090|


### 1. Show the lastName, party and votes for the constituency 'S14000024' in 2017.

```sql
SELECT lastName, party, votes
FROM ge
WHERE constituency='S14000024'
    AND yr=2017
ORDER BY votes DESC;
```

### 2. Who won? You can use the RANK function to see the order of the candidates. If you RANK using (ORDER BY votes DESC) then the candidate with the most votes has rank 1.

Show the party and RANK for constituency S14000024 in 2017. List the output by party

```sql
SELECT party, votes, RANK() OVER (ORDER BY votes DESC) as pos
FROM ge
WHERE constituency ='S14000024'
    AND yr=2017
ORDER BY party;
```

### 3.The 2015 election is a different PARTITION to the 2017 election. We only care about the order of votes for each year.

Use PARTITION to show the ranking of each party in S14000021 in each year. Include yr, party, votes and ranking (the party with the most votes is 1).

```sql
SELECT yr,
       party,
       votes,
       RANK() OVER (PARTITION BY yr ORDER BY votes DESC) AS pos
FROM ge
WHERE constituency='S14000021'
ORDER BY party, yr;'
```

### 4. Edinburgh constituencies are numbered S14000021 to S14000026.

Use PARTITION BY constituency to show the ranking of each party in Edinburgh in 2017. Order your results so the winners are shown first, then ordered by constituency.

``` sql
SELECT constituency,
       party,
       votes,
       RANK() OVER (PARTITION BY yr, constituency ORDER BY votes DESC) AS pos
FROM ge
WHERE yr = 2017
    AND constituency BETWEEN 'S14000021' AND 'S14000026'
ORDER BY pos, constituency;
```

### 5. You can use SELECT within SELECT to pick out only the winners in Edinburgh.

Show the parties that won for each Edinburgh constituency in 2017.

```sql
SELECT constituency, party
FROM 
    (
        SELECT constituency,
        party,
        votes,
        RANK() OVER (PARTITION BY yr, constituency ORDER BY votes DESC) AS pos
        FROM ge
        WHERE yr = 2017
            AND constituency BETWEEN 'S14000021' AND 'S14000026'
        ORDER BY pos, constituency
    ) AS edin
WHERE pos = 1
```

### 6. You can use COUNT and GROUP BY to see how each party did in Scotland. Scottish constituencies start with 'S'

Show how many seats for each party in Scotland in 2017.

```sql
SELECT party, COUNT(party)
FROM 
    (
        SELECT constituency,
        party,
        votes,
        RANK() OVER (PARTITION BY yr, constituency ORDER BY votes DESC) AS pos
        FROM ge
        WHERE yr = 2017
            AND constituency LIKE 'S%'
        ORDER BY pos, constituency
    ) AS scot
WHERE pos = 1
GROUP BY party;
```