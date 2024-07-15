SQL Question

<img width="960" alt="image" src="https://github.com/naman-dwivedi1/DB_Practice/assets/174860133/4a4732cd-c527-4674-84e6-49fa70f6472f">

Solution

The SQL query aims to select a subset of image predictions from the unlabeled_image_predictions table. Here's a concise breakdown of what each part of the query does:

Common Table Expressions (CTEs):

posi: This CTE selects rows from unlabeled_image_predictions, assigns a row number (rno) based on descending order of score, and renames score as weak_label.

nega: This CTE selects rows similarly to posi, but assigns row numbers based on ascending order of score.


Main Query:

The main query selects image_id and weak_label from the combined results of two subqueries (one from posi and one from nega):

From posi, it selects rows where rno is divisible by 3 and less than or equal to 30,000.

From nega, it selects rows where rno is divisible by 3 and less than or equal to 30,000.


Union Operation:

The UNION operation combines the results of the above two subqueries, ensuring unique rows (removes duplicates).


Final Output:
The result is ordered by image_id and includes image_id and weak_label (rounded to the nearest whole number).


output screenshot

<img width="1433" alt="image" src="https://github.com/naman-dwivedi1/DB_Practice/assets/174860133/9fc1c3c8-f44c-471f-935b-cb6a06b6ddbe">
<img width="1440" alt="image" src="https://github.com/naman-dwivedi1/DB_Practice/assets/174860133/d7b5d220-eec7-4f9f-9ec2-495e2b686c1c">



ER - diagram

Question: Make the ER diagram for the following tables and relations

<img width="677" alt="image" src="https://github.com/naman-dwivedi1/DB_Practice/assets/174860133/4832b6a5-0335-47c1-895d-620e785efe5f">

Solution
<img width="1440" alt="image" src="https://github.com/naman-dwivedi1/DB_Practice/assets/174860133/15884927-1fb3-46d2-be86-3439418b8355">

Question 2:

# Problem Statement

## Database Schema

```sql
CREATE TABLE city
(
	id INT PRIMARY KEY,
	name VARCHAR(50) NOT NULL
);

CREATE TABLE hotel
(
	id INT PRIMARY KEY,
	city_id INT NOT NULL REFERENCES cityl,
	name VARCHAR(50) NOT NULL,
	day_price NUMERIC(8, 2) NOT NULL,
	photos JSONB DEFAULT '[]'
);


CREATE TABLE booking
(
	id int PRIMARY KEY,
	hotel_id INT NOT NULL REFERENCES hotel,
	booking_date DATE NOT NULL,
	start_date DATE NOT NULL,
	end_date DATE NOT NULL
);
```

## DESCRIPTION

Your task is to prepare a list of cities with the date of last reservation made in the city and a main photo (photos[0]) of the most popular (by number of bookings) hotel in this city.

Sort results in ascending order by city. If many hotels have the same amount of bookings sort them by ID (ascending order).
Remember that the query will also be run of different datasets.

Solution:

```
WITH BookingsPerHotel AS (
    SELECT
        h.city_id,
        h.id AS hotel_id,
        COUNT(b.id) AS bookings_count
    FROM
        hotel h
    LEFT JOIN
        booking b ON h.id = b.hotel_id
    GROUP BY
        h.city_id, h.id
),
MaxBookingsPerCity AS (
    SELECT
        city_id,
        MAX(bookings_count) AS max_bookings_count
    FROM
        BookingsPerHotel
    GROUP BY
        city_id
),
PopularHotels AS (
    SELECT
        bh.city_id,
        bh.hotel_id,
        h.photos ->> 0 AS hotel_photo
    FROM
        BookingsPerHotel bh
    JOIN
        MaxBookingsPerCity max_counts ON bh.city_id = max_counts.city_id
    JOIN
        hotel h ON bh.hotel_id = h.id
    WHERE
        bh.bookings_count = max_counts.max_bookings_count
)
SELECT
    c.name AS city_name,
    lb.last_booking_date,
    ph.hotel_id,
    ph.hotel_photo
FROM
    city c
LEFT JOIN (
    SELECT
        h.city_id,
        MAX(b.booking_date) AS last_booking_date
    FROM
        booking b
    JOIN
        hotel h ON b.hotel_id = h.id
    GROUP BY
        h.city_id
) lb ON c.id = lb.city_id
LEFT JOIN
    PopularHotels ph ON c.id = ph.city_id
ORDER BY
    c.name;
```

Output:
<img width="1373" alt="image" src="https://github.com/user-attachments/assets/8b68e8b8-72a6-4d29-a585-19932705329c">
<img width="1341" alt="image" src="https://github.com/user-attachments/assets/9424984b-21aa-4f74-a23e-a588cd872a8c">


