### Teachers and Departments

> stops

| id | name |
| --- | -- |

> route

| num | company | pos | stop |
|--   |   --    | --   | --  |


1. How many stops are in the database.

```sql

SELECT 
  COUNT(id) 
FROM 
  stops

```

2. Find the id value for the stop 'Craiglockhart'.

```sql

SELECT 
  id 
FROM 
  stops 
WHERE 
  name = 'Craiglockhart'

```

3. Find the id value for the stop 'Craiglockhart'.

```sql

SELECT 
  id, 
  name 
FROM 
  route r 
  JOIN stops s ON r.stop = s.id 
  AND r.company = 'LRT' 
  AND r.num = '4'

```

4. The query shown gives the number of routes that visit either London Road (149) or Craiglockhart (53). Run the query and notice the two services that link these stops have a count of 2. Add a HAVING clause to restrict the output to these two routes.

```sql

SELECT 
  company, 
  num, 
  COUNT(*) 
FROM 
  route 
WHERE 
  stop = 149 
  OR stop = 53 
GROUP BY 
  company, 
  num 
HAVING 
  COUNT(*) >= 2

```

5. Execute the self join shown and observe that b.stop gives all the places you can get to from Craiglockhart, without changing routes. Change the query so that it shows the services from Craiglockhart to London Road.

```sql

SELECT 
  a.company, 
  a.num, 
  a.stop, 
  b.stop 
FROM 
  route a 
  JOIN route b ON (
    a.company = b.company 
    AND a.num = b.num
  ) 
WHERE 
  a.stop = 53 
  AND b.stop = 149

```

6. The query shown is similar to the previous one, however by joining two copies of the stops table we can refer to stops by name rather than by number. Change the query so that the services between 'Craiglockhart' and 'London Road' are shown. If you are tired of these places try 'Fairmilehead' against 'Tollcross'.

```sql

SELECT 
  a.company, 
  a.num, 
  stopa.name, 
  stopb.name 
FROM 
  route a 
  JOIN route b ON (
    a.company = b.company 
    AND a.num = b.num
  ) 
  JOIN stops stopa ON (a.stop = stopa.id) 
  JOIN stops stopb ON (b.stop = stopb.id) 
WHERE 
  stopa.name = 'Craiglockhart' 
  AND stopb.name = 'London Road'

```

7. The query shown is similar to the previous one, however by joining two copies of the stops table we can refer to stops by name rather than by number. Change the query so that the services between 'Craiglockhart' and 'London Road' are shown. If you are tired of these places try 'Fairmilehead' against 'Tollcross'.

```sql

SELECT 
  a.company, 
  a.num, 
  stopa.name, 
  stopb.name 
FROM 
  route a 
  JOIN route b ON (
    a.company = b.company 
    AND a.num = b.num
  ) 
  JOIN stops stopa ON (a.stop = stopa.id) 
  JOIN stops stopb ON (b.stop = stopb.id) 
WHERE 
  stopa.name = 'Craiglockhart' 
  AND stopb.name = 'London Road'

```

7. Give a list of all the services which connect stops 115 and 137 ('Haymarket' and 'Leith').

```sql

SELECT 
  DISTINCT a.company, 
  a.num 
FROM 
  route a 
  JOIN route b ON a.company = b.company 
  AND a.num = b.num 
  AND a.stop = 115 
  AND b.stop = 137

```

8. Give a list of the services which connect the stops 'Craiglockhart' and 'Tollcross'.

```sql

SELECT 
  DISTINCT a.company, 
  a.num 
FROM 
  route a 
  JOIN route b ON a.company = b.company 
  AND a.num = b.num 
  AND a.stop = (
    SELECT 
      id 
    FROM 
      stops 
    WHERE 
      name = 'Craiglockhart'
  ) 
  AND b.stop = (
    SELECT 
      id 
    FROM 
      stops 
    WHERE 
      name = 'Tollcross'
  )
```

9. Give a distinct list of the stops which may be reached from 'Craiglockhart' by taking one bus, including 'Craiglockhart' itself, offered by the LRT company. Include the company and bus no. of the relevant services.

```sql

SELECT 
  s1.name, 
  a.company, 
  a.num 
FROM 
  route a 
  JOIN route b ON (
    a.company = b.company 
    AND a.num = b.num
  ) 
  JOIN stops s1 ON (a.stop = s1.id) 
  JOIN stops s2 ON (b.stop = s2.id) 
WHERE 
  s2.name = 'Craiglockhart'

```

10. Find the routes involving two buses that can go from Craiglockhart to Lochend. Show the bus no. and company for the first bus, the name of the stop for the transfer,
  and the bus no. and company for the second bus.

  Hint
  Self-join twice to find buses that visit Craiglockhart and Lochend, then join those on matching stops.


[本题难度大,参考](https://blog.csdn.net/Hielw/article/details/80170921)
```sql

SELECT 
  DISTINCT bus1.num, 
  bus1.company, 
  name, 
  bus2.num, 
  bus2.company 
FROM 
  (
    SELECT 
      start1.num, 
      start1.company, 
      stop1.stop 
    FROM 
      route AS start1 
      JOIN route AS stop1 ON start1.num = stop1.num 
      AND start1.company = stop1.company 
      AND start1.stop != stop1.stop 
    WHERE 
      start1.stop = (
        SELECT 
          id 
        FROM 
          stops 
        WHERE 
          name = 'Craiglockhart'
      )
  ) AS bus1 
  JOIN (
    SELECT 
      start2.num, 
      start2.company, 
      start2.stop 
    FROM 
      route AS start2 
      JOIN route AS stop2 ON start2.num = stop2.num 
      AND start2.company = stop2.company 
      AND start2.stop != stop2.stop 
    WHERE 
      stop2.stop = (
        SELECT 
          id 
        FROM 
          stops 
        WHERE 
          name = 'Lochend'
      )
  ) AS bus2 ON bus1.stop = bus2.stop 
  JOIN stops ON bus1.stop = stops.id
  
```