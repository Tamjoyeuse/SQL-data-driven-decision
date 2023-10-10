# SQL-data-driven-decision



SELECT COUNT(customer_id) -- Count the total number of customers
FROM customers
WHERE date_of_birth BETWEEN '1980-01-01' AND '1989-12-31'; -- Select customers born between 1980-01-01 and 1989-12-31


SELECT customer_id,  -- Report the customer_id
       AVG(rating), -- Report the average rating per customer
       COUNT(rating), -- Report the number of ratings per customer
       COUNT(*) -- Report the number of movie rentals per customer
FROM renting
GROUP BY customer_id
HAVING COUNT(*) > 7 -- Select only customers with more than 7 movie rentals
ORDER BY AVG(rating); -- Order by the average rating in ascending order


SELECT 
	SUM(m.renting_price), -- Get the revenue from movie rentals
	COUNT(*), -- Count the number of rentals
	COUNT(DISTINCT r.customer_id) -- Count the number of customers
FROM renting AS r
LEFT JOIN movies AS m
ON r.movie_id = m.movie_id;


SELECT 
	SUM(m.renting_price), 
	COUNT(*), 
	COUNT(DISTINCT r.customer_id)
FROM renting AS r
LEFT JOIN movies AS m
ON r.movie_id = m.movie_id
-- Only look at movie rentals in 2018
WHERE date_renting > '2017-12-31' AND 
date_renting < '2019-01-01' ;


SELECT a.name, -- Create a list of movie titles and actor names
       m.title
FROM actsin AS ai
LEFT JOIN movies AS m
ON m.movie_id = ai.movie_id
LEFT JOIN actors AS a
ON a.actor_id = ai.actor_id;


SELECT m.title, -- Use a join to get the movie title and price for each movie rental
       renting_price
FROM renting AS r
LEFT JOIN movies AS m
ON m.movie_id = r.movie_id;


SELECT rm.title, -- Report the income from movie rentals for each movie 
      SUM(renting_price) AS income_movie
FROM
       (SELECT m.title,  
               m.renting_price
       FROM renting AS r
       LEFT JOIN movies AS m
       ON r.movie_id=m.movie_id) AS rm
GROUP BY rm.title
ORDER BY income_movie DESC; -- Order the result by decreasing income


SELECT a.gender, -- Report for male and female actors from the USA 
       MIN(a.year_of_birth), -- The year of birth of the oldest actor
      MAX(a.year_of_birth) -- The year of birth of the youngest actor
FROM
   (SELECT * -- Use a subsequen SELECT to get all information about actors from the USA
   FROM actors)
   AS a
   WHERE nationality = 'USA' -- Give the table the name a
GROUP BY gender;


SELECT *
FROM renting AS r
LEFT JOIN customers AS c   -- Add customer information
ON r.customer_id = c.customer_id
LEFT JOIN movies AS m   -- Add movie information
ON m.movie_id = r.movie_id;


SELECT *
FROM renting AS r
LEFT JOIN customers AS c
ON c.customer_id = r.customer_id
LEFT JOIN movies AS m
ON m.movie_id = r.movie_id
WHERE c.date_of_birth BETWEEN'1970-01-01'
AND '1979-12-31'; -- Select customers born in the 70s

SELECT m.title, 
COUNT(*),
AVG(r.rating) 
FROM renting AS r
LEFT JOIN customers AS c
ON c.customer_id = r.customer_id
LEFT JOIN movies AS m
ON m.movie_id = r.movie_id
WHERE c.date_of_birth BETWEEN '1970-01-01' AND '1979-12-31'
GROUP BY m.title 
HAVING COUNT(*) > 1 -- Remove movies with only one rental
ORDER BY AVG(r.rating) DESC; -- Order with highest rating first


SELECT *
FROM renting as r 
LEFT JOIN customers AS c  -- Augment table renting with information about customers 
ON c.customer_id = r.customer_id
LEFT JOIN actsin AS ai -- Augment the table renting with the table actsin 
ON ai.movie_id = r.movie_id
LEFT JOIN actors AS a -- Augment table renting with information about actors
ON a.actor_id = ai.actor_id;

SELECT a.name,  c.gender,
       COUNT(*) AS number_views, 
       AVG(r.rating) AS avg_rating
FROM renting as r
LEFT JOIN customers AS c
ON r.customer_id = c.customer_id
LEFT JOIN actsin as ai
ON r.movie_id = ai.movie_id
LEFT JOIN actors as a
ON ai.actor_id = a.actor_id

GROUP BY a.name, c.gender-- For each actor, separately for male and female customers
HAVING AVG(r.rating) IS NOT NULL AND COUNT(*) > 5-- Report only actors with more than 5 movie rentals
ORDER BY avg_rating DESC, number_views DESC;

SELECT *
FROM renting AS r -- Augment the table renting with information about customers
LEFT JOIN customers AS c
ON c.customer_id = r.customer_id
LEFT JOIN movies AS m -- Augment the table renting with information about movies
ON m.movie_id = r.movie_id
WHERE r.date_renting > '2018-12-31'; -- Select only records about rentals since the beginning of 2019


SELECT 
	c.country,                   -- For each country report
	COUNT(renting_id) AS number_renting, -- The number of movie rentals
	AVG(rating) AS average_rating, -- The average rating
	SUM(renting_price) AS revenue         -- The revenue from movie rentals
FROM renting AS r
LEFT JOIN customers AS c
ON c.customer_id = r.customer_id
LEFT JOIN movies AS m
ON m.movie_id = r.movie_id
WHERE date_renting >= '2019-01-01'
GROUP BY c.country
ORDER BY c.country;
