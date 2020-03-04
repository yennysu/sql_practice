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
                    WHERE name='Russia')
```

## 2. Richer than UK
### Show the countries in Europe with a per capita GDP greater than 'United Kingdom'.

## 3. Neighbours of Argentina and Australia
### List the name and continent of countries in the continents containing either Argentina or Australia. Order by name of the country.

## 4. Between Canada and Poland
### Which country has a population that is more than Canada but less than Poland? Show the name and the population.

## 5. Percentages of Germany
### Show the name and the population of each country in Europe. Show the population as a percentage of the population of Germany.

## 6. Bigger than every country in Europe
### Which countries have a GDP greater than every country in Europe? [Give the name only.] (Some countries may have NULL gdp values)

## 7. Largest in each continent
### Find the largest country (by area) in each continent, show the continent, the name and the area:

## 8. First country of each continent (alphabetically)
### List each continent and the name of the country that comes first alphabetically.

## 9. Find the continents where all countries have a population <= 25000000. Then find the names of the countries associated with these continents. Show name, continent and population.

## 10. Some countries have populations more than three times that of any of their neighbours (in the same continent). Give the countries and continents.