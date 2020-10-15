# astra_restau_share
An app to organize co-lunches


## Create Tables

```
CREATE TABLE IF NOT EXISTS restaurants_by_area (
  name text,
  phone text,
  area text,
  specialities text,
  PRIMARY KEY ((area), name)
 );
 
CREATE TABLE IF NOT EXISTS users_by_role (
  first_name text,
  phone text,
  role text,
  PRIMARY KEY (phone)
);

CREATE TABLE IF NOT EXISTS events (
  date date,
  user_phone text,
  user_first_name text,
  restaurant_name text,
  restaurant_area text,
  user_order text,
  amount float,
  paid boolean,
  PRIMARY KEY (user_phone, date)
);

CREATE TABLE IF NOT EXISTS comments_by_restaurants (
  restaurant_name text,
  restaurant_area text,
  user_phone text,
  user_first_name text,
  note float,
  comment text,
  PRIMARY KEY ((restaurant_name, restaurant_area), user_phone)
);

CREATE TABLE IF NOT EXISTS event_votes (
  restaurant_name text,
  restaurant_area text,
  date date,
  user_phone text,
  user_first_name text,
  PRIMARY KEY (date, restaurant_name, restaurant_area, user_phone)
);

CREATE TABLE IF NOT EXISTS restaurants_by_date (
  date date,
  restaurant_name text,
  restaurant_area text,
  votes int,
  PRIMARY KEY ((date), restaurant_name, restaurant_area)
); 
 ```
 
 ## Add data
 ### Basic data
 
 ```
 // INSERTS INTO restaurants_by_area
INSERT INTO restaurants_by_area (name, phone, area, specialities)
VALUES ('KFC', '03.03.03.03.03', 'Sud', 'Chicken');
INSERT INTO restaurants_by_area (name, phone, area, specialities)
VALUES ('KFC', '03.03.03.03.04', 'Nord', 'Chicken');
INSERT INTO restaurants_by_area (name, phone, area, specialities)
VALUES ('Punjab', '03.03.03.03.05', 'Centre', 'Indien');
INSERT INTO restaurants_by_area (name, phone, area, specialities)
VALUES ('Eden Burger', '03.03.03.03.06', 'Centre', 'Burger');

// INSERTS INTO users_by_role
INSERT INTO users_by_role (first_name, phone, role)
VALUES ('Yasmine', '03.02.01.02.03', 'manager');
INSERT INTO users_by_role (first_name, phone, role)
VALUES ('Aurélie', '03.02.01.02.04', 'co-worker');
INSERT INTO users_by_role (first_name, phone, role)
VALUES ('Océane', '03.02.01.02.05', 'co-worker');
INSERT INTO users_by_role (first_name, phone)
VALUES ('Quentin', '03.02.01.02.06');
```
## Placing orders
```
INSERT INTO events (date, restaurant_name, restaurant_area, user_phone, user_first_name, user_order, paid)
VALUES ('2020-10-01', 'Punjab', 'Centre', '03.02.01.02.03', 'Yasmine', 'Agneau au curry', false);
INSERT INTO events (date, restaurant_name, restaurant_area, user_phone, user_first_name, user_order, paid)
VALUES ('2020-10-01', 'Punjab', 'Centre', '03.02.01.02.05', 'Océane', 'Poulet Tika Masala', false);
INSERT INTO events (date, restaurant_name, restaurant_area, user_phone, user_first_name, user_order, paid)
VALUES ('2020-10-01', 'Punjab', 'Centre', '03.02.01.02.06', 'Quentin', 'Menu complet', false);
INSERT INTO events (date, restaurant_name, restaurant_area, user_phone, user_first_name, user_order, paid)
VALUES ('2020-10-01', 'Punjab', 'Centre', '03.02.01.02.04', 'Aurélie', 'Menu Vegan', false);

INSERT INTO events (date, restaurant_name, restaurant_area, user_phone, user_first_name, user_order, paid)
VALUES ('2020-10-08', 'Eden', 'Centre', '03.02.01.02.03', 'Yasmine', 'Cheese burger', false);
INSERT INTO events (date, restaurant_name, restaurant_area, user_phone, user_first_name, user_order, paid)
VALUES ('2020-10-08', 'Eden', 'Centre', '03.02.01.02.05', 'Océane', 'Bacon deluxe', false);
INSERT INTO events (date, restaurant_name, restaurant_area, user_phone, user_first_name, user_order, paid)
VALUES ('2020-10-08', 'Eden', 'Centre', '03.02.01.02.06', 'Quentin', 'Maxi Burger', false);
```

## Updating bills

```
UPDATE events SET amount = 14.50 WHERE user_phone = '03.02.01.02.03' AND date = '2020-10-01';
UPDATE events SET amount = 18 WHERE user_phone = '03.02.01.02.05' AND date = '2020-10-01';
UPDATE events SET amount = 20 WHERE user_phone = '03.02.01.02.06' AND date = '2020-10-01';
UPDATE events SET amount = 14.50 WHERE user_phone = '03.02.01.02.04' AND date = '2020-10-01';

UPDATE events SET amount = 12 WHERE user_phone = '03.02.01.02.03' AND date = '2020-10-08';
UPDATE events SET amount = 12 WHERE user_phone = '03.02.01.02.05' AND date = '2020-10-08';
UPDATE events SET amount = 22.50 WHERE user_phone = '03.02.01.02.06' AND date = '2020-10-08';
```

## Preparing invoices for a participant

```
SELECT user_first_name, SUM(amount) AS total_due FROM events WHERE user_phone = '03.02.01.02.06' AND paid = false ALLOW FILTERING;
```

## A user can evaluate a restaurant

### Add users evaluations
```
INSERT INTO comments_by_restaurants (restaurant_name , restaurant_area , user_phone , user_first_name , note, comment )
VALUES ('Punjab', 'Center', '03.02.01.02.06', 'Quentin', 4, 'Excellent, mais un peu juste sur la quantite');
INSERT INTO comments_by_restaurants (restaurant_name , restaurant_area , user_phone , user_first_name , note, comment )
VALUES ('Punjab', 'Center', '03.02.01.02.03', 'Yasmine', 5, 'Bien agréable ! Bonne présentation !');
INSERT INTO comments_by_restaurants (restaurant_name , restaurant_area , user_phone , user_first_name , note, comment )
VALUES ('Eden', 'Center', '03.02.01.02.05', 'Océane', 3, '');
```


### Show user's evaluation of a restaurant - can be used to display empty form.
```
SELECT * FROM comments_by_restaurants WHERE restaurant_name = 'Punjab' AND restaurant_area = 'Center' AND user_phone = '03.02.01.02.03';
SELECT * FROM comments_by_restaurants WHERE restaurant_name = 'Eden' AND restaurant_area = 'Center' AND user_phone = '03.02.01.02.03';
SELECT COUNT(*) AS answered FROM comments_by_restaurants WHERE restaurant_name = 'Eden' AND restaurant_area = 'Center' AND user_phone = '03.02.01.02.03';
```

### Show restaurants evaluations
```
SELECT * FROM comments_by_restaurants WHERE restaurant_name = 'Punjab' AND restaurant_name = 'Center';
// The average note of one restaurant;
SELECT restaurant_name, restaurant_name, AVG(note) AS rate FROM comments_by_restaurants WHERE restaurant_name = 'Punjab' AND restaurant_area = 'Center';
// The averages notes of all restaurant (descending order);
SELECT restaurant_name, restaurant_area, AVG(note) AS rate FROM comments_by_restaurants GROUP BY restaurant_name, restaurant_area;
```

## Proposition du restaurant

### Manager propose restaurant
```
// Manager give choice
INSERT INTO restaurants_by_date (date, restaurant_name, restaurant_area)
VALUES ('2020-10-08', 'KFC', 'Centre');
INSERT INTO restaurants_by_date (date, restaurant_name, restaurant_area, winner)
VALUES ('2020-10-08', 'Eden', 'Centre', false);
==
UPDATE votes SET votes = votes + 0
WHERE date = '2020-10-08' AND restaurant_name = 'Eden' AND restaurant_area = 'Centre' AND user_phone = '03.02.01.02.03';

UPDATE votes SET votes = votes + 0 WHERE date = '2020-10-08' AND restaurant_name = 'KFC' AND restaurant_area = 'Centre' AND user_phone = '03.02.01.02.03';

UPDATE votes SET votes = votes + 0 WHERE date = '2020-10-01' AND restaurant_name = 'Punjab' AND restaurant_area = 'Centre' AND user_phone = '03.02.01.02.03' AND vote_time = now();

// Manager impose one restaurant
INSERT INTO restaurants_by_date (date, restaurant_name, restaurant_area, winner)
VALUES ('2020-10-01', 'Punjab', 'Centre', true);


// Query to display propositions
SELECT * FROM restaurants_by_date WHERE date = '2020-10-08';
SELECT * FROM votes  WHERE date = '2020-10-08' AND votes = 0 ALLOW FILTERING ;

// To close vote
UPDATE votes SET votes = votes + 1 WHERE date = '2020-10-01' AND restaurant_name = 'Punjab' AND restaurant_area = 'Centre' AND user_phone = '03.02.01.02.03';

// to display all results
SELECT  date, restaurant_name, restaurant_area, MAX(votes) AS total_votes FROM votes
WHERE date = '2020-10-08' GROUP BY restaurant_name, restaurant_area;

// to display the winner
SELECT  date, restaurant_name, restaurant_area, MAX(votes) AS total_votes FROM votes WHERE date = '2020-10-08';


```

### Participants vote for a restaurant
```
INSERT INTO event_votes (restaurant_name, restaurant_area, date, user_phone, user_first_name)
VALUES ('Eden', 'Centre', '2020-10-08', '03.02.01.02.03', 'Yasmine');
INSERT INTO event_votes (restaurant_name, restaurant_area, date, user_phone, user_first_name)
VALUES ('Eden', 'Centre', '2020-10-08', '03.02.01.02.05', 'Océane');
INSERT INTO event_votes (restaurant_name, restaurant_area, date, user_phone, user_first_name)
VALUES ('KFC', 'Centre', '2020-10-08', '03.02.01.02.06', 'Quentin');
INSERT INTO event_votes (restaurant_name, restaurant_area, date, user_phone, user_first_name)
VALUES ('KFC', 'Centre', '2020-10-08', '03.02.01.02.08', 'Aflred');
INSERT INTO event_votes (restaurant_name, restaurant_area, date, user_phone, user_first_name)
VALUES ('KFC', 'Centre', '2020-10-08', '03.02.01.02.09', 'Béa');

UPDATE votes SET votes = votes + 1 WHERE date = '2020-10-08' AND restaurant_name = 'Eden' AND restaurant_area = 'Centre' AND user_phone = '03.02.01.02.05';
UPDATE votes SET votes = votes + 1 WHERE date = '2020-10-08' AND restaurant_name = 'Eden' AND restaurant_area = 'Centre' AND user_phone = '03.02.01.02.06';


```

### Manager displays choosen restaurant
```
// apply the votes
UPDATE restaurants_by_date SET votes = 4 WHERE date = '2020-10-08' AND restaurant_name = 'KFC' AND restaurant_area = 'Centre';

// get the winner from participants votes
SELECT COUNT(restaurant_name) AS total_votes, date, restaurant_name, restaurant_area FROM event_votes WHERE date = '2020-10-08' GROUP BY restaurant_name, restaurant_area LIMIT 1;

SELECT COUNT(restaurant_name) AS total_votes, date, restaurant_name, restaurant_area FROM event_votes WHERE date = '2020-10-08' AND restaurant_name = 'KFC' AND restaurant_area = 'Centre';


// set the winner
UPDATE restaurants_by_date SET winner = true
WHERE date = '2020-10-08' AND restaurant_name = 'Eden' AND restaurant_area = 'Centre';

// query to display screen to choose order
SELECT * FROM restaurants_by_date WHERE date = '2020-10-08' AND winner = true ALLOW FILTERING;
```



// resultats
SELECT date, restaurant_name, COUNT(restaurant_name) AS total_votes FROM event_votes WHERE date = '2020-10-08' GROUP BY restaurant_name;
```


CREATE TABLE IF NOT EXISTS votes (
  date date,
  restaurant_name text,
  restaurant_area text,
  user_phone text,
  votes counter,
  PRIMARY KEY ((date), restaurant_name, restaurant_area, user_phone)
);


CREATE TABLE IF NOT EXISTS restaurant_by_date (
  date date,
  restaurant_name text,
  restaurant_area text,
  user_phone text,
  votes counter,
  PRIMARY KEY ((date), restaurant_name, restaurant_area, user_phone)
  WITH
);

SELECT json date, restaurant_name, restaurant_area, count(1) AS votes FROM event_votes WHERE date = '2020-10-08' GROUP BY restaurant_name, restaurant_area;
