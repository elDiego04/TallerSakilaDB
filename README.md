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
```
 
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
```

## Excercise 8

Select the top two most-viewed movies in each city.
```
```

## Excercise 9

Select the first name, last name, and email of all customers from the United States who have not made any film rentals in the last three months.
```
```

## Excercise 10

Select the top 3 customers from each store based on the number of rentals made. Utilize the Rank, Dense_Rank, and Row_Number functions, and create an additional boolean field indicating records where these three functions return the same value (0) and records where these three functions do not return the same value (1).
```
``` 












