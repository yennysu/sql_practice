# SELECT Within SELECT Tutorial

### Source:
### https://sqlzoo.net/wiki/SELECT_within_SELECT_Tutorial

### Sample Data:
| name        | continent | area    | population | gdp          |
|-------------|-----------|---------|------------|--------------|
| Afghanistan | Asia      | 652230  | 25500100   | 20343000000  |
| Albania     | Europe    | 28748   | 2831741    | 12960000000  |
| Algeria     | Africa    | 2381741 | 37100000   | 188681000000 |

## 1. Bigger than Russia
### List each country name where the population is larger than that of 'Russia'.

```sql
SELECT name
FROM world
WHERE population > (SELECT population
                    FROM world
                    WHERE name = 'Russia');
```

## 2. Richer than UK
### Show the countries in Europe with a per capita GDP greater than 'United Kingdom'.

```sql
SELECT name
FROM world
WHERE gdp/population > (SELECT
                            gdp/population
                        FROM world
                        WHERE name='United Kingdom'
                        )
    AND continent = 'Europe';
```

## 3. Neighbours of Argentina and Australia
### List the name and continent of countries in the continents containing either Argentina or Australia. Order by name of the country.

```sql
SELECT name,
       continent
FROM world
WHERE continent IN (
                    SELECT continent
                    FROM world
                    WHERE name IN ('Argentina', 'Australia')
                    )
ORDER BY name;
```

## 4. Between Canada and Poland
### Which country has a population that is more than Canada but less than Poland? Show the name and the population.
``` sql
SELECT name, population
FROM world
WHERE population > (SELECT population
                    FROM world
                    WHERE name = 'Canada')
  AND population < (SELECT population
                    FROM world
                    WHERE name = 'Poland');
```

## 5. Percentages of Germany
### Show the name and the population of each country in Europe. Show the population as a percentage of the population of Germany.
```sql
SELECT name,
       CONCAT(
           ROUND(
               (population / (SELECT population
                              FROM world
                              WHERE name = 'Germany')) * 100
               ), '%')
FROM world;
```

## 6. Bigger than every country in Europe
### Which countries have a GDP greater than every country in Europe? [Give the name only.] (Some countries may have NULL gdp values)

```sql
SELECT name
FROM world
WHERE gdp >= ALL(SELECT gdp
                 FROM world
                 WHERE gdp IS NOT NULL
                    AND continent = 'Europe'
);
```

## 7. Largest in each continent
### Find the largest country (by area) in each continent, show the continent, the name and the area:
#### This is an example of a correlated subquery

```sql
SELECT continent, name, area
FROM world x
WHERE area >= ALL(SELECT name
                  FROM world y
                  WHERE x.continent = y.continent);
```

## 8. First country of each continent (alphabetically)
### List each continent and the name of the country that comes first alphabetically.

```sql
SELECT continent, name
FROM world x
WHERE name <= ALL(SELECT name
                  FROM world y
                  WHERE x.continent = y.continent);
```

## 9. Find the continents where all countries have a population <= 25000000. Then find the names of the countries associated with these continents. Show name, continent and population.

```sql
SELECT name, continent, population
FROM world
WHERE continent IN (SELECT continent
                    FROM world
                    GROUP BY continent
                    HAVING MAX(population) <= 25000000);
```

## 10. Some countries have populations more than three times that of any of their neighbours (in the same continent). Give the countries and continents.

```sql
SELECT name, continent
FROM world x
WHERE population >= ALL(SELECT 3*population
                        FROM world y
                        WHERE x.continent = y.continent
                            AND x.name <> y.name);
```