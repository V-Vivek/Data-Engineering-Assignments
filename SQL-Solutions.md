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
