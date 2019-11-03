### SELECT ELECT within SELECT Tutorial

1. List each country name where the population is larger than that of 'Russia'.

```sql

SELECT name
FROM world
WHERE population > (
	SELECT population
	FROM world
	WHERE name = 'Russia'
)

```

2. Show the names of countries in Europe with a per capita GDP greater than 'United Kingdom'.

```sql

SELECT name
FROM world
WHERE gdp / population > (
		SELECT gdp / population
		FROM world
		WHERE name = 'United Kingdom'
	)
	AND continent = 'Europe'

```

3. List the name and continent of countries in the continents containing either Argentina or Australia. Order by name of the country.

```sql

SELECT name, continent
FROM world
WHERE continent IN (
	SELECT continent
	FROM world
	WHERE name IN ('Argentina ', 'Australia')
)
ORDER BY name

```

4. Which country has a population that is more than Canada but less than Poland? Show the name and the population.

```sql

SELECT name, population
FROM world
WHERE population > (
		SELECT population
		FROM world
		WHERE name = 'Canada'
	)
	AND population < (
		SELECT population
		FROM world
		WHERE name = 'Poland'
	)

```

5. Show the name and the population of each country in Europe. Show the population as a percentage of the population of Germany.


```sql

SELECT name
	, CONCAT(ROUND(population / (
		SELECT population
		FROM world
		WHERE name = 'Germany'
	) * 100, 0), '%')
FROM world
WHERE continent = 'Europe'

```

6. Which countries have a GDP greater than every country in Europe? [Give the name only.] (Some countries may have NULL gdp values)

```sql

SELECT name
FROM world
WHERE gdp > ALL (
	SELECT gdp
	FROM world
	WHERE continent = 'Europe'
		AND gdp > 0
)

```

7. Find the largest country (by area) in each continent, show the continent, the name and the area:

```sql

SELECT continent, name, area
FROM world x
WHERE area >= ALL (
	SELECT area
	FROM world y
	WHERE y.continent = x.continent
		AND area > 0
)

```

8. List each continent and the name of the country that comes first alphabetically.

```sql

SELECT w.continent, w.name
FROM world w
WHERE w.name = (
	SELECT name
	FROM world s
	WHERE w.continent = s.continent
	ORDER BY s.name
	LIMIT 1
)

```

9. Find the continents where all countries have a population <= 25000000. Then find the names of the countries associated with these continents. Show name, continent and population.

```sql

SELECT w.name, w.continent, w.population
FROM world w
WHERE 25000000 >= ALL (
	SELECT s.population
	FROM world s
	WHERE s.continent = w.continent
		AND s.population > 0
)

```

10. Some countries have populations more than three times that of any of their neighbours (in the same continent). Give the countries and continents.

```sql

SELECT w.name, w.continent FROM world w WHERE w.population / 3 >= ALL (SELECT s.population FROM world s WHERE w.continent = s.continent AND s.population > 0 AND w.name != s.name
)

```