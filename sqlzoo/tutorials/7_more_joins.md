# More JOIN operations

### Source:
### https://sqlzoo.net/wiki/More_JOIN_operations

### Data:
<b>Table: movie</b>

| id | title | yr | director | budget | gross |
|-|-|-|-|-|-|
|10003|"Crocodile" Dundee II|1988|38|15800000|239606210
|10004|'Til There Was You|1997|49|10000000	

<b>Table: actor</b>

| id | name |
|-|-|
|20| Paul Hogan|
|50| Jeanne Tripplehorn|

<b>Table: casting</b>

| movieid | actorid | ord |
|-|-|-|
|10003|20|4|
|10004|50|1|

### 1. List the films where the yr is 1962 [Show id, title]

```sql
SELECT id, title
FROM movie
WHERE yr = 1962;
```

### 2. When was Citizen Kane released?

```sql
SELECT yr
FROM movie
WHERE title = 'Citizen Kane';
```

### 3. List all of the Star Trek movies, include the id, title and yr (all of these movies include the words Star Trek in the title). Order results by year.

```sql
SELECT id, title, yr
FROM movie
WHERE title LIKE '%Star Trek%'
ORDER BY yr;
```

### 4. What id number does the actor 'Glenn Close' have?

``` sql
SELECT id
FROM actor
WHERE name = 'Glenn Close';
```

### 5. What is the id of the film 'Casablanca'?

```sql
SELECT id
FROM movie
WHERE title = 'Casablanca';
```

### 6. Obtain the cast list for 'Casablanca'. What is a cast list? Use movieid=11768, (or whatever value you got from the previous question)

```sql
SELECT a.name
FROM casting c
JOIN actor a
    ON c.actorid = a.id
WHERE c.movieid = 11768;
```

### 7. Obtain the cast list for the film 'Alien'

```sql
SELECT a.name
FROM casting c
JOIN actor a
    ON c.actorid = a.id
JOIN movie m
    ON c.movieid = m.id
WHERE m.title = 'Alien';
```

### 8. List the films in which 'Harrison Ford' has appeared.

```sql
SELECT m.title
FROM movie m
JOIN casting c
    ON m.id = c.movieid
JOIN actor a
    ON c.actorid = a.id
WHERE a.name = 'Harrison Ford';
```

### 9. List the films where 'Harrison Ford' has appeared - but not in the starring role. [Note: the ord field of casting gives the position of the actor. If ord=1 then this actor is in the starring role]

```sql
SELECT m.title
FROM movie m
JOIN casting c
    ON m.id = c.movieid
JOIN actor a
    ON c.actorid = a.id
WHERE a.name = 'Harrison Ford'
    AND c.ord > 1;
```

### 10. List the films together with the leading star for all 1962 films.

```sql
SELECT m.title, a.name
FROM movie m
JOIN casting c
    ON c.movieid = m.id
JOIN actor a
    ON c.actorid = a.id
WHERE m.yr = 1962
    AND c.ord = 1;
```

### 11. Which were the busiest years for 'Rock Hudson'. Show the year and the number of movies he made each year for any year in which he made more than 2 movies.

```sql
SELECT m.yr, COUNT(m.title)
FROM movie m
JOIN casting c
    ON m.id = c.movieid
JOIN actor a
    ON a.id = c.actorid
WHERE a.name = 'Rock Hudson'
GROUP BY m.yr
HAVING COUNT(m.title) > 2;
```

### 12. List the film title and the leading actor for all of the films 'Julie Andrews' played in.

```sql
SELECT m.title, a.name
FROM movie m
JOIN casting c
    ON m.id = c.movieid
JOIN actor a
    ON c.actorid = a.id
WHERE m.id IN (SELECT movieid
               FROM casting
               WHERE actorid IN (
                                    SELECT id
                                    FROM actor
                                    WHERE name = 'Julie Andrews'
              ))
    AND c.ord = 1;

```

### 13. Obtain a list, in alphabetical order, of actors who've had at least 15 starring roles.

```sql
SELECT a.name
FROM actor a
JOIN (SELECT actorid, COUNT(ord) as lead_count
      FROM casting
      WHERE ord = 1
      GROUP BY actorid) as la
    ON a.id = la.actorid
WHERE la.lead_count >= 15
ORDER BY a.name;
```

### 14. List the films released in the year 1978 ordered by the number of actors in the cast, then by title

```sql
SELECT m.title
FROM movie m
JOIN (SELECT movieid, COUNT(actorid) as actor_count
        FROM casting
        GROUP BY movieid) as mc
    ON m.id = mc.movieid
WHERE m.yr = 1978
ORDER BY mc.actor_count DESC, m.title;
```

### 15. List all the people who have worked with 'Art Garfunkel'.

```sql
SELECT a.name
FROM actor a
JOIN casting c
    ON a.id = c.actorid
WHERE c.movieid IN (SELECT c.movieid
                    FROM casting c
                    JOIN actor a
                        ON a.id = c.actorid
                    WHERE a.name = 'Art Garfunkel')
    AND a.name <> 'Art Garfunkel';
```