# More JOIN Operations

1. List the films where the yr is 1962 [Show id, title]

    ```sql
    SELECT id, title
     FROM movie
     WHERE yr=1962
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

4. What are the titles of the films with id 11768, 11955, 21191

    ```sql
    SELECT title
     FROM movie
     WHERE id IN (11768,11955,21191)
    ```

5. What id number does the actor 'Glenn Close' have?

    ```sql
    SELECT id
     FROM actor
     WHERE name = 'Glenn Close'
    ```

6. What is the id of the film 'Casablanca'

    ```sql
    SELECT id
     FROM movie
     WHERE title = 'Casablanca'
    ```

7. Obtain the cast list for 'Casablanca'. Use the id value that you obtained in the previous question.

    ```sql
    SELECT name
     FROM casting JOIN actor ON id = actorid
     WHERE movieid = 11768
    ```

8. Obtain the cast list for the film 'Alien'

    ```sql
    SELECT name
     FROM casting JOIN actor a ON a.id = actorid
     JOIN movie m ON m.id = movieid
     WHERE title = 'Alien'
    ```

9. List the films in which 'Harrison Ford' has appeared

    ```sql
    SELECT title
     FROM movie m JOIN casting ON movieid = m.id
     JOIN actor a ON actorid = a.id
     WHERE name = 'Harrison Ford'
    ```

10. List the films where 'Harrison Ford' has appeared - but not in the star role. [Note: the ord field of casting gives the position of the actor. If ord=1 then this actor is in the starring role]

    ```sql
    SELECT DISTINCT title
     FROM movie m JOIN casting ON m.id = movieid
     JOIN actor a ON a.id = actorid
     WHERE ord != 1 AND name = 'Harrison Ford'
    ```

11. List the films together with the leading star for all 1962 films.

    ```sql
    SELECT title, name
     FROM movie m JOIN casting ON m.id = movieid
     JOIN actor a ON a.id = actorid
     WHERE ord =1 AND yr = 1962
    ```

12. Which were the busiest years for 'John Travolta', show the year and the number of movies he made each year for any year in which he made more than 2 movies.

    ```sql
    SELECT yr,COUNT(title) FROM
      movie JOIN casting ON movie.id=movieid
             JOIN actor   ON actorid=actor.id
    WHERE name='John Travolta'
    GROUP BY yr
    HAVING COUNT(title)=(SELECT MAX(c) FROM
    (SELECT yr,COUNT(title) AS c FROM
       movie JOIN casting ON movie.id=movieid
             JOIN actor   ON actorid=actor.id
     WHERE name='John Travolta'
     GROUP BY yr) AS t
    )
    ```

13. List the film title and the leading actor for all of the films 'Julie Andrews' played in.

    ```sql
    SELECT title, name FROM movie m JOIN casting ON m.id = movieid
    JOIN actor a ON a.id=actorid WHERE ord =1 AND movieid IN (SELECT movieid FROM casting
    WHERE actorid IN (
      SELECT id FROM actor
      WHERE name='Julie Andrews'))
    ```

14. Obtain a list in alphabetical order of actors who've had at least 30 starring roles.

    ```sql
    SELECT name FROM actor JOIN (SELECT actorid, COUNT(*)
     FROM casting WHERE ord = 1
     GROUP BY actorid
     HAVING COUNT(*) >= 30) actors ON id = actorid ORDER BY name
    ```

15. List the films released in the year 1978 ordered by the number of actors in the cast.

    ```sql
    SELECT title, COUNT(actorid) as ca
     FROM movie JOIN casting ON id = movieid
     WHERE yr = 1978 GROUP BY title
     ORDER BY ca DESC
    ```

16. List all the people who have worked with 'Art Garfunkel'.

    ```sql
    SELECT name FROM actor
     JOIN casting ON id = actorid
     WHERE movieid IN (SELECT movieid
        FROM movie m JOIN casting ON m.id = movieid
        JOIN actor a ON a.id = actorid
        WHERE name = 'Art Garfunkel')
     AND name != 'Art Garfunkel'
    ```
