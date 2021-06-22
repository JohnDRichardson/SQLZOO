# SQLZOO
My answers for [SQLzoo](https://sqlzoo.net) questions

## Sections:
1. [SELECT basics](#select-basics)
2. [SELECT from WORLD](#select-from-world)
3. [SELECT from NOBEL](#select-from-nobel)
4. [SELECT within SELECT](#select-within-select)
5. [SUM and COUNT](#sum-and-count)
6. [JOIN](#join)
7. [More JOIN](#more-join)
8. [Using NULL](#using-null)
9. [Self JOIN](#self-join)

## SELECT basics

1. The example uses a WHERE clause to show the population of 'France'. Note that strings (pieces of text that are data) should be in 'single quotes'; Modify it to show the population of Germany
```sql
  SELECT population
    FROM world
  WHERE name = 'Germany'
```
2. Checking a list The word IN allows us to check if an item is in a list. The example shows the name and population for the countries 'Brazil', 'Russia', 'India' and 'China'. Show the name and the population for 'Sweden', 'Norway' and 'Denmark'.
```sql
  SELECT name, population
    FROM world
  WHERE name IN ('Sweden', 'Norway', 'Denmark')
```
3. Which countries are not too small and not too big? BETWEEN allows range checking (range specified is inclusive of boundary values). The example below shows countries with an area of 250,000-300,000 sq. km. Modify it to show the country and the area for countries with an area between 200,000 and 250,000.
```sql
  SELECT name, area
    FROM world
  WHERE area BETWEEN 200000 AND 250000
```

## SELECT from WORLD

1. Read the notes about this table. Observe the result of running this SQL command to show the name, continent and population of all countries.
```sql
  SELECT name, continent, population
    FROM world
```
2. How to use WHERE to filter records. Show the name for the countries that have a population of at least 200 million. 200 million is 200000000, there are eight zeros.
```sql
  SELECT name
    FROM world
  WHERE population > 200000000
```
3. Give the name and the per capita GDP for those countries with a population of at least 200 million.
```sql
  SELECT name, gdp/population as per_capita_GDP 
    FROM world
  WHERE population >=200000000
```
4. Show the name and population in millions for the countries of the continent 'South America'. Divide the population by 1000000 to get population in millions.
```sql
  SELECT name, population / 1000000 
    FROM world
  WHERE  continent = 'South America'
```
5. Show the name and population for France, Germany, Italy
```sql
  SELECT name,population
    FROM world
  WHERE name IN ('France','Germany','Italy')
```
6. Show the countries which have a name that includes the word 'United'
```sql
  SELECT name
    FROM world
  WHERE name LIKE '%United%'
```
7. Two ways to be big: A country is big if it has an area of more than 3 million sq km or it has a population of more than 250 million. Show the countries that are big by area or big by population. Show name, population and area.
```sql
  SELECT name, population, area
    FROM world
  WHERE population > 250000000 or area > 3000000
```
8. Exclusive OR (XOR). Show the countries that are big by area (more than 3 million) or big by population (more than 250 million) but not both. Show name, population and area.
```sql
  SELECT name, population,area
    FROM world
  WHERE (population>250000000 OR area>3000000)
    AND NOT(population>250000000 AND area>3000000)
```
9. Show the name and population in millions and the GDP in billions for the countries of the continent 'South America'. Use the ROUND function to show the values to two decimal places.
```sql
  SELECT name, 
         ROUND(population/1000000, 2) as population, 
         ROUND(GDP/1000000000, 2) as GDP
    FROM world
  WHERE continent = 'South America'
```
10. Show the name and per-capita GDP for those countries with a GDP of at least one trillion (1000000000000; that is 12 zeros). Round this value to the nearest 1000.
```sql
  SELECT name, ROUND(GDP/population,-3) as GDP_per_capita
    FROM world
  WHERE GDP >= 1000000000000
```
11. Show the name and capital where the name and the capital have the same number of characters.
```sql
  SELECT name, capital
    FROM world
  WHERE LEN(name) = LEN(capital)
 ```
 12. Show the name and the capital where the first letters of each match. Don't include countries where the name and the capital are the same word.
 ```sql
  SELECT name, capital
    FROM world
  WHERE LEFT(name, 1) = LEFT(capital, 1) 
    AND name <> capital
 ```
 13. Find the country that has all the vowels and no spaces in its name.
```sql
  SELECT name
     FROM world
  WHERE name LIKE '%a%' AND
    name LIKE '%e%' AND
    name LIKE '%i%' AND
    name LIKE '%o%' AND
    name LIKE '%u%' AND
    name NOT LIKE '% %'
```

## SELECT from Nobel

1. Change the query shown so that it displays Nobel prizes for 1950.
```sql
  SELECT yr, subject, winner
    FROM nobel
  WHERE yr = 1950
```
2. Show who won the 1962 prize for Literature.
```sql
  SELECT winner
    FROM nobel
  WHERE yr = 1962
    AND subject = 'Literature'
```
3. Show the year and subject that won 'Albert Einstein' his prize.
```sql
  SELECT yr, subject
    FROM nobel
  WHERE winner = 'Albert Einstein'
```
4. Give the name of the 'Peace' winners since the year 2000, including 2000.
```sql
  SELECT winner
    FROM nobel
  WHERE subject = 'Peace'
    AND yr >= 2000
```
5. Show all details (yr, subject, winner) of the Literature prize winners for 1980 to 1989 inclusive.
```sql
  SELECT yr,subject,winner
    FROM nobel
  WHERE subject = 'Literature'
    AND yr BETWEEN 1980 AND 1989
```
6. Show all details of the presidential winners:Theodore Roosevelt, Woodrow Wilson, Jimmy Carter, Barack Obama
```sql
  SELECT *
    FROM nobel
  WHERE winner IN ('Theodore Roosevelt', 
                   'Woodrow Wilson', 
                   'Jimmy Carter', 
                   'Barack Obama')
```
7. Show the winners with first name John
```sql
  SELECT winner
    FROM nobel
  WHERE winner LIKE 'John %'
```
8. Show the year, subject, and name of Physics winners for 1980 together with the Chemistry winners for 1984.
```sql
  SELECT *
    FROM nobel
  WHERE (subject='physics' AND yr=1980)
     OR (subject='chemistry' AND yr=1984)
```
9. Show the year, subject, and name of winners for 1980 excluding Chemistry and Medicine
```sql
  SELECT *
    FROM nobel
  WHERE yr=1980 AND
    subject NOT IN ('Chemistry','Medicine')
```
10. Show year, subject, and name of people who won a 'Medicine' prize in an early year (before 1910, not including 1910) together with winners of a 'Literature' prize in a later year (after 2004, including 2004)
```sql
  SELECT *
    FROM nobel 
  WHERE (subject='Medicine' AND yr <1910)
     OR (subject='Literature' AND yr>=2004)
```
11. Find all details of the prize won by PETER GRÜNBERG
```sql
  SELECT *
    FROM nobel 
  WHERE winner = 'Peter Grünberg'
```
12. Find all details of the prize won by EUGENE O'NEILL
```sql
  SELECT *
    FROM nobel 
  WHERE winner = 'Eugene O''NEILL'
```
13. List the winners, year and subject where the winner starts with Sir. Show the the most recent first, then by name order.
```sql
  SELECT winner, yr, subject
    FROM nobel 
  WHERE winner LIKE 'Sir%'
  ORDER BY yr DESC,
           winner ASC
```
14. Show the 1984 winners and subject ordered by subject and winner name; but list Chemistry and Physics last.
```sql
  SELECT winner, subject
    FROM nobel
   WHERE yr=1984
   ORDER BY
     CASE WHEN subject IN ('Physics','Chemistry') THEN 1 ELSE 0 END,
     subject,
     winner
```

## SELECT within SELECT

1. List each country name where the population is larger than that of 'Russia'.
```sql
  SELECT name FROM world
  WHERE population >
       (SELECT population FROM world
        WHERE name='Russia')
```
2. Show the countries in Europe with a per capita GDP greater than 'United Kingdom'.
```sql
  SELECT name from world
  WHERE continent = 'Europe' 
    AND gdp/population > (SELECT gdp/population FROM world
                          WHERE name = 'United Kingdom')
```
3. List the name and continent of countries in the continents containing either Argentina or Australia. Order by name of the country.
```sql
  SELECT name, continent FROM world
  WHERE continent IN (SELECT continent FROM world 
                      WHERE name IN('Argentina', 'Australia'))
  ORDER BY name
```
4. Which country has a population that is more than Canada but less than Poland? Show the name and the population.
```sql
  SELECT name, population FROM world
  WHERE population > (SELECT population FROM world WHERE name = 'Canada') 
    AND population < (SELECT population FROM world WHERE name = 'Poland') 
```
5. Show the name and the population of each country in Europe. Show the population as a percentage of the population of Germany.
```sql
SELECT name, 
       CONCAT(CASTROUND(population/(SELECT population FROM world WHERE name = 'Germany') *100,0) as int), '%')  as percentage
  FROM world
WHERE continent = 'Europe'
```
6. Which countries have a GDP greater than every country in Europe? [Give the name only.] (Some countries may have NULL gdp values)
```sql
  SELECT name FROM world
  WHERE gdp > ALL(SELECT gdp FROM world WHERE gdp >0 AND continent = 'Europe')
```
7. Find the largest country (by area) in each continent, show the continent, the name and the area:
```sql
  SELECT continent, name, area FROM world x
    WHERE area >= ALL
      (SELECT area FROM world y
          WHERE y.continent=x.continent
            AND area>0)
``` 
8. List each continent and the name of the country that comes first alphabetically.
```sql
  SELECT continent, name FROM world as x
  WHERE name = (SELECT TOP 1 name FROM world as y 
                WHERE x.continent = y.continent 
                ORDER BY name) 
```
9. Find the continents where all countries have a population <= 25000000. Then find the names of the countries associated with these continents. Show name, continent and population.
```sql
  SELECT name, continent, population FROM world
  WHERE continent <> ALL(SELECT continent FROM world
                         WHERE population > 25000000)
```
10. Some countries have populations more than three times that of any of their neighbours (in the same continent). Give the countries and continents.
```sql
  SELECT name, continent FROM world AS x
  WHERE population >  ALL (SELECT 3 * y.population FROM world AS y
                             WHERE x.continent = y.continent
                               AND x.name != y.name)
```

## SUM and COUNT

1. Show the total population of the world.
```sql
  SELECT SUM(population) as population
  FROM world
```
2. List all the continents - just once each.
```sql
  SELECT DISTINCT(continent) as continent
  FROM world
```
3. Give the total GDP of Africa
```sql
  SELECT SUM(gdp) as GDP FROM world
  WHERE continent = 'Africa'
```
4.
```sql
  SELECT COUNT(name) as count FROM world
  WHERE area >= 1000000
```
5. What is the total population of ('Estonia', 'Latvia', 'Lithuania')
```sql
  SELECT SUM(population) as population FROM world
  WHERE name IN('Estonia', 'Latvia', 'Lithuania')
```
6. For each continent show the continent and number of countries.
```sql
  SELECT continent, COUNT(name) as count FROM world
  GROUP BY(continent)
```
7. For each continent show the continent and number of countries with populations of at least 10 million.
```sql
  SELECT continent, COUNT(name) as count FROM world
  WHERE population >= 10000000
  GROUP BY(continent)
```
8. List the continents that have a total population of at least 100 million.
```sql
  SELECT continent FROM world
  GROUP BY continent
  HAVING SUM(population)>= 100000000
```

## Join

1. Modify it to show the matchid and player name for all goals scored by Germany. To identify German players, check for: teamid = 'GER'
```sql
  SELECT matchid, player FROM goal
  WHERE teamid LIKE 'GER'
```
2. Show id, stadium, team1, team2 for just game 1012
```sql
  SELECT id, stadium, team1, team2 FROM game
  WHERE id=1012
```
3. 
```sql
  SELECT player, teamid, stadium, mdate
    FROM game JOIN goal ON game.id = goal.matchid
  WHERE teamid='GER'
```
4. Show the team1, team2 and player for every goal scored by a player called Mario player LIKE 'Mario%'
```sql
  SELECT team1, team2, player
    FROM game JOIN goal ON game.id = goal.matchid
  WHERE player LIKE 'Mario%'
```
5. Show player, teamid, coach, gtime for all goals scored in the first 10 minutes gtime<=10
```sql
  SELECT player, teamid, coach, gtime
    FROM goal JOIN eteam ON goal.teamid = eteam.id
  WHERE gtime<=10
```
6. List the dates of the matches and the name of the team in which 'Fernando Santos' was the team1 coach.
```sql
  SELECT mdate, teamname
    FROM game JOIN eteam ON game.team1 = eteam.id
  WHERE coach='Fernando Santos'
```
7. List the player for every goal scored in a game where the stadium was 'National Stadium, Warsaw'
```sql
  SELECT player
    FROM goal JOIN game ON goal.id = game.matchid
  WHERE stadium = 'National Stadium, Warsaw'
```
8. Instead show the name of all players who scored a goal against Germany.
```sql
SELECT DISTINCT player
  FROM game JOIN goal ON game.matchid = goal.id 
WHERE (team1 = 'GER' OR team2 = 'GER')
  AND teamid!='GER'
```
9. Show teamname and the total number of goals scored. 
```sql
  SELECT teamname, COUNT(teamname) as count
    FROM eteam JOIN goal ON eteam.id = goal.teamid
  GROUP BY teamname
```
10. Show the stadium and the number of goals scored in each stadium.
```sql
SELECT stadium, COUNT(goal.matchid) as goals
  FROM goal JOIN game ON goal.matchid=game.id
GROUP BY stadium
```
11. For every match involving 'POL', show the matchid, date and the number of goals scored.
```sql
  SELECT matchid, mdate, COUNT(goal.teamid) as goals
    FROM game JOIN goal ON game.id = goal.matchid 
  WHERE (team1 = 'POL' OR team2 = 'POL')
  GROUP BY matchid, mdate
```
12. For every match where 'GER' scored, show matchid, match date and the number of goals scored by 'GER'
```sql
  SELECT matchid,mdate,COUNT(teamid) as goals
    FROM game JOIN goal ON game.id = goal.matchid 
  WHERE (teamid='GER')
  GROUP BY matchid, mdate
```
13. List every match with the goals scored by each team as shown. This will use "CASE WHEN" which has not been explained in any previous exercises.
```sql
  SELECT mdate,
         team1, SUM(CASE WHEN teamid = team1 THEN 1 ELSE 0 END) score1,
         team2, SUM(CASE WHEN teamid = team2 THEN 1 ELSE 0 END) score2
    FROM game LEFT JOIN goal ON game.id = goal.matchid
  GROUP BY mdate, matchid, team1, team2
```

## Join

1. List the films where the yr is 1962 [Show id, title]
```sql
  SELECT id, title
   FROM movie
  WHERE yr=1962
```
2. Give year of 'Citizen Kane'.
```sql
  SELECT yr FROM movie 
  WHERE title='Citizen Kane'
```
3. List all of the Star Trek movies, include the id, title and yr (all of these movies include the words Star Trek in the title). Order results by year.
```sql
  SELECT id, title, yr FROM movie
  WHERE title LIKE 'Star Trek%'
  ORDER BY yr
```
4. What id number does the actor 'Glenn Close' have? 
```sql
  SELECT id FROM actor
  WHERE name= 'Glenn Close'
```
5. What is the id of the film 'Casablanca'
```sql
  SELECT id FROM movie 
  WHERE title='Casablanca'
```
6. Obtain the cast list for 'Casablanca'.
```sql
  SELECT actor.name
  FROM actor JOIN casting ON casting.actorid = actor.id
  WHERE casting.movieid = 27
```
7. Obtain the cast list for the film 'Alien'
```sql
SELECT actor.name
  FROM actor
   JOIN casting ON (casting.actorid = actor.id)
   JOIN movie ON (movie.id = casting.movieid)
WHERE movie.title = 'Alien'
```
8. List the films in which 'Harrison Ford' has appeared
```sql
  SELECT title
    FROM movie, casting, actor
  WHERE name='Harrison Ford'
      AND casting.movieid = movie.id
      AND casting.actorid = actor.id
 
  SELECT movie.title
    FROM movie
     JOIN casting ON casting.movieid = movie.id
     JOIN actor ON actor.id = casting.actorid
  WHERE actor.name = 'Harrison Ford'
```
9. List the films where 'Harrison Ford' has appeared - but not in the starring role. [Note: the ord field of casting gives the position of the actor. If ord=1 then this actor is in the starring role]
```sql
  SELECT title
    FROM movie, casting, actor
  WHERE name='Harrison Ford'
    AND movieid = movie.id
    AND actorid = actor.id
    AND ord<>1
```
10. List the films together with the leading star for all 1962 films.
```sql
  SELECT title, name
    FROM movie, casting, actor
  WHERE yr=1962
    AND casting.movieid = movie.id
    AND casting.actorid = actor.id
    AND ord = 1
```
11. Which were the busiest years for 'Rock Hudson', show the year and the number of movies he made each year for any year in which he made more than 2 movies.
```sql
  SELECT yr,COUNT(title) as movies FROM
    movie JOIN casting ON movie.id = movieid
          JOIN actor   ON actorid = actor.id
  WHERE name='Doris Day'
  GROUP BY yr
  HAVING COUNT(title) > 1
```
12. List the film title and the leading actor for all of the films 'Julie Andrews' played in.
```sql
  SELECT title, name
    FROM movie, casting, actor
    WHERE casting.movieid = movie.id
      AND casting.actorid = actor.id
      AND ord = 1
      AND movieid IN (SELECT movieid FROM casting, actor
                      WHERE casting.actorid = actor.id
                        AND name = 'Julie Andrews')
```
13. Obtain a list, in alphabetical order, of actors who've had at least 15 starring roles.
```sql
SELECT name
  FROM  casting, actor
  WHERE casting.actorid = actor.id
    AND ord = 1
  Group by name
  Having count(name)>=15
```
14. List the films released in the year 1978 ordered by the number of actors in the cast, then by title.
```sql
  SELECT title, count(actorid) as actors
    FROM  movie, casting
    WHERE casting.movieid = movie.id
      AND yr = 1978
    Group by title
    ORDER BY actors desc, title
```
15. List all the people who have worked with 'Art Garfunkel'.
```sql
  SELECT distinct actor.name
  FROM movie, casting, actor
  WHERE casting.movieid = movie.id
    AND actor.id = casting.actorid
    AND movie.id IN (SELECT movieid
                     FROM casting, actor
                     WHERE casting.actorid = actor.id
                     AND actor.name = 'Art Garfunkel')
    AND actor.name <> 'Art Garfunkel'
```
