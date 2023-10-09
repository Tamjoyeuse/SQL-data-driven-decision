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
