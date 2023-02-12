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
  WHEN click_count IS NOT NULL AND view_count IS NOT NULL THEN ROUND(100.0 * (click_count / (click_count + view_count)), 2)
  ELSE 0
END AS ctr
FROM T1
ORDER BY 2 DESC
```
