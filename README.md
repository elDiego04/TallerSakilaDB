# TallerSakilaDB

## Excercise 1

Insert a record into the 'film' table using dummy values, ensuring referential integrity with other tables.

```  
INSERT INTO film ( title, description, release_year, language_id, rental_duration, rental_rate, length, replacement_cost, rating, special_features, last_update)
VALUES ('LA VITA È BELLA', 'A Epic Drama About the Survival of a Child in a Nazi Concentration Camp',1997, 2, 3, 4.99, 116, 14.99, 'PG-13', 'Trailers', NOW());
```
![image](https://github.com/elDiego04/TallerSakilaDB/assets/117874546/3fdbc1cf-d728-4532-8c1a-50b7c052d72a)


## Excercise 2

Which films are longer than the average duration of films?
```
``` 

## Excercise 3

Which films are currently rented at the store with store_id = 1?
```
```

## Excercise 4

Of the movies at the store with store_id = 1, which ones were rented for a longer duration than the average rental period?
```
```
 
## Excercise 5

Which actors are part of the cast of 5 or fewer movies?
```
```

## Excercise 6

Which last names do not repeat among different actors?
```
```

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












