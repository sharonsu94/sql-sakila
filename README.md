# sql-sakila

1a)

    USE sakila;
    SELECT first_name, last_name
    FROM actor;

1b)

    UPDATE actor SET first_name = UPPER(first_name);
    UPDATE actor SET last_name = UPPER(last_name);
    UPDATE actor SET `Actor Name` = CONCAT(first_name, ' ', last_name);

2a)

    SELECT actor_id, first_name, last_name from actor 
    WHERE first_name = 'JOE';

2b) 

    SELECT * FROM actor
    WHERE last_name LIKE '%G%' 
    AND last_name LIKE '%E%' 
    AND last_name LIKE '%N%';

2c)

    SELECT * FROM actor
    WHERE last_name LIKE '%L%'
    AND last_name LIKE '%I%'
    ORDER BY last_name, first_name;

2d)

    SELECT country_id, country FROM country 
    WHERE country IN ('Afghanistan', 'Bangladesh', 'China');    

3a)

    ALTER TABLE actor 
    ADD middle_name VARCHAR(100)
    AFTER first_name;

3b)

    ALTER TABLE actor
    MODIFY COLUMN middle_name blob;
  	  	  	

3c)

    ALTER TABLE actor
    DROP middle_name;

4a)

    SELECT last_name, COUNT(last_name) 
    FROM actor
    GROUP BY last_name;

4b)

    SELECT last_name, COUNT(last_name) 
    FROM actor
    GROUP BY last_name
    HAVING COUNT(last_name) >1;
  	
4c)

    UPDATE actor
    SET first_name = 'HARPO', `Actor Name` = 'HARPO WILLIAMS'
    WHERE `Actor Name` = 'GROUCHO WILLIAMS';

4d)

    UPDATE actor
    SET first_name = 'GROUCHO'
    WHERE last_name = 'WILLIAMS'
    AND first_name = 'HARPO';

    UPDATE actor
    SET first_name = 'MUCHO GROUCHO'
    WHERE last_name = 'WILLIAMS'
    AND first_name != 'GROUCHO';
  	
5a)

    SHOW CREATE TABLE sakila.address;

6a)

    SELECT s.first_name, s.last_name, a.address
    FROM staff s
    JOIN address a ON
    s.address_id = a.address_id;

6b)

    SELECT s.first_name, s.last_name, sum(p.amount)
    FROM staff s
    JOIN payment p ON
    s.staff_id = p.staff_id
    WHERE year(p.payment_date) = 2005 AND month(p.payment_date) = 08
    GROUP BY s.staff_id;

6c)

    SELECT f.title, COUNT(fa.actor_id)
    FROM film f
    JOIN film_actor fa
    WHERE f.film_id = fa.film_id
    GROUP BY f.title;
  	
6d)

    SELECT COUNT(i.inventory_id)
    FROM inventory i
    JOIN film f
    ON f.film_id = i.film_id
    WHERE f.title = 'Hunchback Impossible';

    6 copies of the film `Hunchback Impossible` exist
  	
6e)

    SELECT c.first_name, c.last_name, SUM(p.amount)
    FROM customer c
    JOIN payment p 
    WHERE c.customer_id = p.customer_id
    GROUP BY c.customer_id
    ORDER BY c.last_name;

7a)

    SELECT title
    FROM film 
    WHERE title LIKE 'K%' OR title LIKE 'Q%' AND language_id IN
    (
        SELECT language_id
        FROM `language`
        WHERE name = 'English');

7b)

    SELECT first_name, last_name
    FROM actor
    WHERE actor_id IN
    (
        SELECT actor_id
        FROM film_actor
        WHERE film_id IN
        (
            SELECT film_id
            FROM film
            WHERE title = 'ALONE TRIP'
        )
    );

7c)

    SELECT c.first_name, c.last_name, c.email
    FROM customer c
    JOIN address a 
    ON c.address_id = a.address_id
    JOIN city ci
    ON a.city_id = ci.city_id
    JOIN country co
    ON ci.country_id = co.country_id
    WHERE co.country = 'Canada';

   
7d)

    SELECT f.title
    FROM film f
    JOIN film_category fc
    ON f.film_id = fc.film_id
    JOIN category c
    ON fc.category_id = c.category_id
    WHERE c.name = 'Family';

7e)

    SELECT f.title, COUNT(r.rental_id)
    FROM rental r
    JOIN inventory i
    ON r.inventory_id = i.inventory_id
    JOIN film f
    ON i.film_id = f.film_id
    GROUP BY f.title
    ORDER BY COUNT(r.rental_id) DESC;

7f) 

    SELECT s.store_id, SUM(p.amount)
    FROM payment p
    JOIN staff s
    ON p.staff_id = s.staff_id
    JOIN store st
    ON s.store_id = st.store_id
    GROUP BY st.store_id;
  	
7g) 

    SELECT store.store_id, city.city, country.country
    FROM store 
    JOIN address
    ON store.address_id = address.address_id
    JOIN city
    ON address.city_id = city.city_id
    JOIN country 
    ON city.country_id = country.country_id;

7h)

    SELECT c.name, sum(p.amount)
    FROM category c
    JOIN film_category fc
    USING(category_id)

    JOIN inventory i
    USING(film_id)

    JOIN rental r 
    USING(inventory_id)

    JOIN payment p
    USING(rental_id)

    GROUP BY c.category_id
    ORDER BY sum(p.amount) DESC
    LIMIT 5;
  	
8a)

    CREATE VIEW top_5_genres AS
    SELECT c.name, sum(p.amount)
    FROM category c
    JOIN film_category fc
    USING(category_id)

    JOIN inventory i
    USING(film_id)

    JOIN rental r 
    USING(inventory_id)

    JOIN payment p
    USING(rental_id)

    GROUP BY c.category_id
    ORDER BY sum(p.amount) DESC
    LIMIT 5;
  	
8b)

    SELECT * FROM top_5_genres;

8c)

    DROP VIEW top_5_genres;


