# TallerSakilaDB

## Excercise 1

Insert a record into the 'film' table using dummy values, ensuring referential integrity with other tables.

```  
INSERT INTO film ( title, description, release_year, language_id, rental_duration, rental_rate, length, replacement_cost, rating, special_features, last_update)
VALUES ('LA VITA È BELLA', 'A Epic Drama About the Survival of a Child in a Nazi Concentration Camp',1997, 2, 3, 4.99, 116, 14.99, 'PG-13', 'Trailers', NOW());
```
![image](https://github.com/elDiego04/TallerSakilaDB/assets/117874546/4fbcbe2e-5af9-4a88-ae46-8a5de176ab79)


## Excercise 2

Which films are longer than the average duration of films?
```
SELECT title, length
FROM film
WHERE length > (SELECT AVG(length) FROM film);
```
![image](https://github.com/elDiego04/TallerSakilaDB/assets/117874546/54e021c0-83ca-410b-89ca-6ad97ec62066)


## Excercise 3

Which films are currently rented at the store with store_id = 1?
```
SELECT f.title, i.store_id, r.return_date
FROM film f
JOIN inventory i ON f.film_id = i.film_id
JOIN rental r ON i.inventory_id = r.inventory_id
WHERE i.store_id = 1
AND r.return_date IS NULL; 
```
![image](https://github.com/elDiego04/TallerSakilaDB/assets/117874546/fe3c9076-806e-4a93-b18a-06e2fe9699a4)


## Excercise 4

Of the movies at the store with store_id = 1, which ones were rented for a longer duration than the average rental period?
```
SELECT f.title, i.store_id
FROM film f
JOIN inventory i ON f.film_id = i.film_id
JOIN rental r ON i.inventory_id = r.inventory_id
WHERE i.store_id = 1
GROUP BY f.title
HAVING DATEDIFF(MAX(r.return_date), MIN(r.rental_date)) > (
    SELECT AVG(DATEDIFF(return_date, rental_date))
    FROM rental
    WHERE inventory_id IN (
        SELECT inventory_id
        FROM inventory
        WHERE store_id = 1
    )
);
```
![image](https://github.com/elDiego04/TallerSakilaDB/assets/117874546/2644cfb8-3e5c-45a8-b7f4-05b337dbb95c)

 
## Excercise 5

Which actors are part of the cast of 5 or fewer movies?
```
SELECT actor_id, first_name, last_name
FROM actor
WHERE actor_id IN (
    SELECT actor_id
    FROM film_actor
    GROUP BY actor_id
    HAVING COUNT(film_id) <= 5
);
```
![image](https://github.com/elDiego04/TallerSakilaDB/assets/117874546/4b42b2bd-a699-4497-ab5c-ddcd86062fb6)

## Excercise 6

Which last names do not repeat among different actors?
```
SELECT DISTINCT last_name
FROM actor;
```
![image](https://github.com/elDiego04/TallerSakilaDB/assets/117874546/027ed665-731f-4900-a206-8992f202cbf9)


## Excercise 7

Create a view with the top 3 genres that generate the highest revenue. List them in descending order, considering the 'amount' field from the payment table for the calculation.
```
CREATE VIEW top_genre_revenue AS
SELECT c.name AS genre, SUM(p.amount) AS total_revenue
FROM category c
JOIN film_category fc ON c.category_id = fc.category_id
JOIN film f ON fc.film_id = f.film_id
JOIN inventory i ON f.film_id = i.film_id
JOIN rental r ON i.inventory_id = r.inventory_id
JOIN payment p ON r.rental_id = p.rental_id
GROUP BY c.name
ORDER BY total_revenue DESC
LIMIT 3;

SELECT * FROM sakila.top_genre_revenue;
```
![image](https://github.com/elDiego04/TallerSakilaDB/assets/117874546/679e479a-2460-444f-b74b-d1d2ce1b178b)


## Excercise 8

Select the top two most-viewed movies in each city.
```
SELECT
    city,
    film_id,
    title,
    views
FROM (
    SELECT 
        c.city,
        f.film_id,
        f.title,
        count(*) AS views,
        ROW_NUMBER() OVER (PARTITION BY c.city ORDER BY COUNT(*) DESC) AS row_num
    FROM city c
    JOIN address a on c.city_id = a.city_id
    JOIN customer cu on a.address_id = cu.address_id
    JOIN rental r on cu.customer_id = r.customer_id
    JOIN inventory i on r.inventory_id = i.inventory_id
    JOIN film f on i.film_id = f.film_id
    GROUP BY c.city_id, c.city, f.film_id, f.title
) AS numbered_views
WHERE row_num <= 2
ORDER BY views DESC;
```
![image](https://github.com/elDiego04/TallerSakilaDB/assets/117874546/077444f5-1879-4528-9fda-48d44384ce98)


## Excercise 9

Select the first name, last name, and email of all customers from the United States who have not made any film rentals in the last three months.
```
SELECT c.first_name, c.last_name, c.email
FROM customer c
JOIN address a ON c.address_id = a.address_id
JOIN city ci ON a.city_id = ci.city_id
JOIN country co ON ci.country_id = co.country_id
WHERE co.country = 'United States'
AND c.customer_id NOT IN (
    SELECT DISTINCT customer_id
    FROM rental
    WHERE rental_date >= DATE_SUB(NOW(), INTERVAL 3 MONTH));
```
![image](https://github.com/elDiego04/TallerSakilaDB/assets/117874546/4810f34b-816b-4c15-a640-767117b1a8b2)


## Excercise 10

Select the top 3 customers from each store based on the number of rentals made. Utilize the Rank, Dense_Rank, and Row_Number functions, and create an additional boolean field indicating records where these three functions return the same value (0) and records where these three functions do not return the same value (1).
```
WITH ranked_customers AS (
    SELECT
        *,
        RANK() OVER (PARTITION BY store_id ORDER BY rental_count DESC) AS rank_value,
        DENSE_RANK() OVER (PARTITION BY store_id ORDER BY rental_count DESC) AS dense_rank_value,
        ROW_NUMBER() OVER (PARTITION BY store_id ORDER BY rental_count DESC) AS row_num
    FROM (
        SELECT
            c.customer_id,
            i.store_id,
            COUNT(r.rental_id) AS rental_count
        FROM customer c
        JOIN rental r ON c.customer_id = r.customer_id
        JOIN inventory i ON r.inventory_id = i.inventory_id
        GROUP BY c.customer_id, i.store_id
    ) AS rental_counts
)
SELECT *,
       CASE
           WHEN rank_value = dense_rank_value AND dense_rank_value = row_num THEN 0
           ELSE 1
       END AS same_rank
FROM ranked_customers
WHERE rank_value <= 3;
```
![image](https://github.com/elDiego04/TallerSakilaDB/assets/117874546/6ccfe458-9fa6-44be-8e19-8f279e701a71)













