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

6. What is the id of the film 'Casablanca'.


```sql

SELECT id
FROM movie
WHERE title ='Casablanca'

```

7. Obtain the cast list for 'Casablanca'.

    Use movieid=11768, (or whatever value you got from the previous question)
```sql

SELECT name
FROM casting
	JOIN actor ON actorid = id
WHERE movieid = 11768

```