# Questions link
https://drive.google.com/file/d/1eT8ck9JJHFPvq0zsWEkrtbtQxXdSjqSB/view
# SQL Questions Set 1

### Q1 - Solution
```
SELECT *
FROM CITY
WHERE (COUNTRYCODE = 'USA' AND POPULATION > 100000);
```

### Q2 - Solution
```
SELECT name
FROM City
WHERE countrycode = 'USA' AND population > 120000;
```

### Q3 - Solution
```
SELECT *
FROM City;
```

### Q4 - Solution
```
SELECT *
FROM City
WHERE ID = 1661;
```

### Q5 - Solution
```
SELECT *
FROM City
WHERE countrycode = 'JPN';
```

### Q6 - Solution
```
SELECT name
FROM City
WHERE countrycode = 'JPN';
```

### Q7 - Solution
```
SELECT city, state
FROM Station;
```

### Q8 - Solution
```
SELECT DISTINCT city
FROM station
WHERE id%2 = 0;
```

### Q9 - Solution
```
SELECT COUNT(City) - COUNT(DISTINCT City)
FROM Station;
```

### Q10 - Solution
```
SELECT City, LENGTH(City)
FROM STATION
ORDER BY LENGTH(City), City
LIMIT 1;

SELECT City, LENGTH(City)
FROM STATION
ORDER BY LENGTH(City) DESC, City
LIMIT 1;
```

### Q11 - Solution
```
SELECT DISTINCT City
FROM Station
WHERE LEFT(City, 1) IN ('a', 'e', 'i', 'o', 'u');
```

### Q12 - Solution
```
SELECT DISTINCT City
FROM Station
WHERE RIGHT(City, 1) IN ('a', 'e', 'i', 'o', 'u');
```

### Q13 - Solution
```
SELECT DISTINCT City
FROM Station
WHERE LEFT(City, 1) NOT IN ('a', 'e', 'i', 'o', 'u');
```

### Q14 - Solution
```
SELECT DISTINCT City
FROM Station
WHERE RIGHT(City, 1) NOT IN ('a', 'e', 'i', 'o', 'u');
```

### Q15 - Solution
```
SELECT DISTINCT City
FROM Station
WHERE LEFT(City, 1) NOT IN ('a', 'e', 'i', 'o', 'u') OR RIGHT(City, 1) NOT IN ('a', 'e', 'i', 'o', 'u');
```

### Q16 - Solution
```
SELECT DISTINCT City
FROM Station
WHERE LEFT(City, 1) NOT IN ('a', 'e', 'i', 'o', 'u') AND RIGHT(City, 1) NOT IN ('a', 'e', 'i', 'o', 'u');
```

### Q17 - Solution
```
SELECT DISTINCT s1.product_id, product_name
FROM Sales s1 JOIN Product p
ON s1.product_id = p.product_id
WHERE NOT EXISTS (SELECT DISTINCT s2.product_id FROM Sales s2 WHERE ((s1.product_id = s2.product_id) AND s2.sale_date NOT BETWEEN '2019-01-01' AND '2019-03-31'));
```

### Q18 - Solution
```
SELECT DISTINCT author_id AS id
FROM views
WHERE author_id = viewer_id
ORDER BY id;
```

### Q19 - Solution
```
SELECT
ROUND (100.0 * (SELECT COUNT(*) FROM Delivery WHERE order_date = customer_pref_delivery_date) / (SELECT COUNT(*) FROM Delivery), 2)
AS immediate_percentage;
```

### Q20 - Solution
```
WITH T1 AS
(
  SELECT ad_id,
  SUM(CASE WHEN action = 'Clicked' THEN 1 ELSE 0 END) AS click_count,
  SUM(CASE WHEN action = 'Viewed' THEN 1 ELSE 0 END) AS view_count
  FROM Ads
  GROUP BY 1
)
SELECT ad_id,
CASE
  WHEN click_count != 0 AND view_count != 0 THEN ROUND(100.0 * (click_count / (click_count + view_count)), 2)
  ELSE 0
END AS ctr
FROM T1
ORDER BY 2 DESC
```

Q21 - Solution
```
WITH T1 AS
(
  SELECT team_id, COUNT(*) AS team_size
  FROM Employee
  GROUP BY 1
)
SELECT employee_id, team_size
FROM Employee e JOIN T1
ON e.team_id = T1.team_id;
```

Q22 - Solution
```
SELECT country_name,
CASE
  WHEN AVG(weather_state) < 16 THEN "Cold"
  WHEN AVG(weather_state) > 24 THEN "Hot"
  ELSE "Warm"
END AS weather_type
FROM Weather w JOIN Countries c
ON w.country_id = c.country_id
WHERE day BETWEEN "2019-11-01" AND "2019-11-30"
GROUP BY 1;
```

Q23 - Solution
```
SELECT p.product_id, ROUND(SUM(price * units) / SUM(units), 2) AS average_price
FROM Prices p JOIN UnitsSold us
ON p.product_id = us.product_id AND (us.purchase_date BETWEEN p.start_date AND p.end_date)
GROUP BY 1;
```

Q24 - Solution
```
WITH T1 AS 
(
  SELECT player_id, event_date AS first_login,
  ROW_NUMBER() OVER(PARTITION BY player_id ORDER BY event_date) AS rn
  FROM Activity
  
)
SELECT player_id, first_login
FROM T1
WHERE rn = 1;
```

Q25 - Solution
```
WITH T1 AS 
(
  SELECT player_id, device_id,
  ROW_NUMBER() OVER(PARTITION BY player_id ORDER BY event_date) AS rn
  FROM Activity
  
)
SELECT player_id, device_id
FROM T1
WHERE rn = 1;
```

Q26 - Solution
```
SELECT product_name, SUM(unit) AS unit
FROM products p JOIN Orders o
ON p.product_id = o.product_id
WHERE order_date BETWEEN "2020-02-01" AND "2020-02-29"
GROUP BY 1
HAVING SUM(unit) > 99;
```

Q27 - Solution
```
SELECT *
FROM Users
WHERE mail REGEXP "^[a-zA-Z][a-zA-Z0-9_\\.-]*(@leetcode.com)$";
```

Q28 - Solution
```
WITH T1 AS
(
  SELECT o.customer_id, c.name, MONTH(order_date), SUM(price * quantity) AS spent
  FROM Orders o JOIN Product p ON o.product_id = p.product_id JOIN Customers c ON o.customer_id = c.customer_id
  WHERE MONTH(order_date) = 6 OR MONTH(order_date) = 7
  GROUP BY 1, 2, 3
  HAVING SUM(price * quantity) > 99
)
SELECT customer_id, name
FROM T1
GROUP BY 1
HAVING COUNT(*) = 2;
```

Q29 - Solution
```
SELECT title
FROM TVProgram t JOIN Content c
ON t.content_id = c.content_id AND Kids_content = "Y" AND content_type = "Movies" AND (program_date BETWEEN "2020-06-01" AND "2020-06-30");
```

Q30 - Solution
```
SELECT q.id, q.year,
CASE WHEN npv IS NULL THEN 0 ELSE npv END AS npv
FROM Queries q LEFT JOIN NPV
ON q.ID = NPV.id AND q.year = NPV.year;
```

Q31 - Solution
```
SELECT q.id, q.year,
CASE WHEN npv IS NULL THEN 0 ELSE npv END AS npv
FROM Queries q LEFT JOIN NPV
ON q.ID = NPV.id AND q.year = NPV.year;
```

Q32 - Solution
```
SELECT unique_id, name
FROM Employees e LEFT JOIN EmployeeUNI uni
ON uni.id = e.id;
```

Q33 - Solution
```
SELECT name, 
CASE WHEN SUM(distance) IS NULL THEN 0 ELSE SUM(distance) END AS travelled_distance
FROM Users u LEFT JOIN Rides r
ON u.id = r.user_id
GROUP BY 1
ORDER BY 2 DESC, 1;
```

Q34 - Solution
```
SELECT product_name, SUM(unit) AS unit
FROM products p JOIN Orders o
ON p.product_id = o.product_id
WHERE order_date BETWEEN "2020-02-01" AND "2020-02-29"
GROUP BY 1
HAVING SUM(unit) > 99;
```

Q35 - Solution
```
(
  SELECT name AS results
  FROM MovieRating mr JOIN Movies m ON mr.movie_id = m.movie_id JOIN Users u ON mr.user_id = u.user_id
  GROUP BY 1
  ORDER BY COUNT(*) DESC, 1
  LIMIT 1
)
UNION
(
  SELECT title AS results
  FROM MovieRating mr JOIN Movies m ON mr.movie_id = m.movie_id JOIN Users u ON mr.user_id = u.user_id
  WHERE created_at BETWEEN "2020-02-01" AND "2020-02-29"
  GROUP BY 1
  ORDER BY AVG(rating) DESC, 1
  LIMIT 1
)
```

Q36 - Solution
```
SELECT name, 
CASE WHEN SUM(distance) IS NULL THEN 0 ELSE SUM(distance) END AS travelled_distance
FROM Users u LEFT JOIN Rides r
ON u.id = r.user_id
GROUP BY 1
ORDER BY 2 DESC, 1;
```

Q37 - Solution
```
SELECT unique_id, name
FROM Employees e LEFT JOIN EmployeeUNI uni
ON uni.id = e.id;
```

Q38 - Solution
```
SELECT s.id, s.name
FROM Students s LEFT JOIN Departments d
ON s.department_id = d.id
WHERE d.name IS NULL;
```

Q39 - Solution
```
SELECT from_id AS person1, to_id AS person2, COUNT(*) AS call_count, SUM(duration) AS total_duration
FROM
(
  (
    SELECT from_id, to_id, duration
    FROM Calls
    WHERE from_id < to_id
  ) 
  UNION ALL
  (
    SELECT to_id AS from_id, from_id AS to_id, duration
    FROM Calls
    WHERE from_id > to_id
  )
) AS T1
GROUP BY 1, 2;
```

Q40 - Solution
```
SELECT p.product_id, ROUND(SUM(price * units) / SUM(units), 2) AS average_price
FROM Prices p JOIN UnitsSold us
ON p.product_id = us.product_id AND (us.purchase_date BETWEEN p.start_date AND p.end_date)
GROUP BY 1;
```

Q41 - Solution
```
SELECT name AS warehouse_name, SUM(units * Width * Length * Height) AS volume
FROM Warehouse w JOIN Products p
ON w.product_id = p.product_id
GROUP BY 1;
```

Q42 - Solution
```
WITH T1 AS
(
  SELECT sale_date, sold_num
  FROM Sales
  WHERE fruit = "apples"
), 
T2 AS
(
  SELECT sale_date, sold_num
  FROM Sales
  WHERE fruit = "oranges"
)
SELECT T1.sale_date, T1.sold_num - T2.sold_num AS diff
FROM T1 JOIN T2
USING (sale_date)
ORDER BY 1;
```

Q43 - Solution
```

```

Q44 - Solution
```

```

Q45 - Solution
```

```

Q46 - Solution
```

```

Q47 - Solution
```

```

Q48 - Solution
```

```

Q49 - Solution
```

```

Q50 - Solution
```

```
