### More JOIN operations

1. List the films where the yr is 1962 [Show id, title].

```sql

SELECT id, title
FROM movie
WHERE yr = 1962

```

2. Give year of 'Citizen Kane'.

```sql

SELECT yr
FROM movie
WHERE title = 'Citizen Kane'

```

3. List all of the Star Trek movies, include the id, title and yr (all of these movies include the words Star Trek in the title). Order results by year.

```sql

SELECT id, title, yr
FROM movie
WHERE title LIKE 'Star Trek%'
ORDER BY yr

```

4. What id number 11768, 11955, 21191 movie have?

```sql

SELECT title
FROM movie
WHERE id IN (11768, 11955, 21191)

```

5. What id number does the actor 'Glenn Close' have?.

```sql

SELECT id
FROM actor
WHERE name = 'Glenn Close'

```

5. What id movie 'Casablanca' have?.

```sql

SELECT id
FROM movie
WHERE title ='Casablanca'

```

7. Obtain the cast list for 'Casablanca'.

    The cast list is the names of the actors who were in the movie.

    Use movieid=11768, (or whatever value you got from the previous question)

```sql

SELECT name
FROM casting
	JOIN actor ON actorid = id
WHERE movieid = 11768

```


7. Obtain the cast list for the film 'Alien'.

```sql

SELECT a.name
FROM actor a
	JOIN (
		SELECT c.actorid AS actorid
		FROM movie m
			JOIN casting c
			ON m.id = c.movieid
				AND m.title = 'Alien'
	) b
	ON a.id = b.actorid

```

8. List the films in which 'Harrison Ford' has appeared.

```sql

SELECT m.title
FROM movie m
	JOIN (
		SELECT c.movieid AS id
		FROM actor a
			JOIN casting c
			ON a.id = c.actorid
				AND a.name = 'Harrison Ford'
	) s
	ON s.id = m.id

```

9. List the films where 'Harrison Ford' has appeared - but not in the starring role. [Note: the ord field of casting gives the position of the actor. If ord=1 then this actor is in the starring role].

```sql

SELECT m.title
FROM movie m
	JOIN (
		SELECT c.movieid AS id, c.ord
		FROM actor a
			JOIN casting c
			ON a.id = c.actorid
				AND a.name = 'Harrison Ford'
				AND c.ord != 1
	) s
	ON s.id = m.id

```

10. List the films together with the leading star for all 1962 films.

```sql

SELECT s.title, a.name
FROM actor a
	JOIN (
		SELECT m.title AS title, c.actorid AS id
		FROM movie m
			JOIN casting c
			ON m.id = c.movieid
				AND m.yr = 1962
				AND ord = 1
	) s
	ON s.id = a.id

```

11. Which were the busiest years for 'Rock Hudson', show the year and the number of movies he made each year for any year in which he made more than 2 movies.

```sql

SELECT yr, COUNT(title) AS num
FROM (
	SELECT movieid
	FROM casting c
	WHERE actorid IN (
		SELECT id
		FROM actor
		WHERE name = 'Rock Hudson'
	)
) s
	JOIN movie m ON s.movieid = m.id
GROUP BY yr
HAVING num > 2

```

12. Which were the busiest years for 'Rock Hudson', show the year and the number of movies he made each year for any year in which he made more than 2 movies.

```sql

SELECT yr, COUNT(title) AS num
FROM (
	SELECT movieid
	FROM casting c
	WHERE actorid IN (
		SELECT id
		FROM actor
		WHERE name = 'Rock Hudson'
	)
) s
	JOIN movie m ON s.movieid = m.id
GROUP BY yr
HAVING num > 2

```

13. List the film title and the leading actor for all of the films 'Julie Andrews' played in.

```sql

SELECT DISTINCT title, name
FROM movie
	JOIN casting ON casting.movieid = movie.id
	JOIN actor
	ON casting.actorid = actor.id
		AND ord = 1
WHERE movieid IN (
	SELECT ex.id
	FROM movie ex
		JOIN casting ON casting.movieid = ex.id
		JOIN actor ON actor.id = casting.actorid
	WHERE name = 'Julie Andrews'
)

```

14. Obtain a list, in alphabetical order, of actors who've had at least 30 starring roles.

```sql

SELECT a.name
FROM casting c
	JOIN actor a
	ON c.actorid = a.id
		AND c.ord = 1
GROUP BY c.actorid, a.name
HAVING COUNT(ord) >= 30
ORDER BY a.name

```

15. List the films released in the year 1978 ordered by the number of actors in the cast, then by title.

```sql

SELECT DISTINCT m.title, COUNT(actorid)
FROM movie m
	JOIN casting c ON m.id = c.movieid
WHERE m.yr = 1978
GROUP BY m.title
ORDER BY COUNT(actorid) DESC, CONVERT(m.title USING utf8)

```

16. List all the people who have worked with 'Art Garfunkel'.

```sql

SELECT a.name
FROM casting c
	JOIN actor a
	ON c.actorid = a.id
		AND a.name != 'Art Garfunkel'
WHERE movieid IN (
	SELECT movieid
	FROM casting
	WHERE actorid = (
		SELECT id
		FROM actor
		WHERE name = 'Art Garfunkel'
	)
)

```