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

CREATE TABLE IF NOT EXISTS coloc.events (
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
