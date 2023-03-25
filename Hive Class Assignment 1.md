## 1. Download vechile sales data
https://github.com/shashank-mishra219/Hive-Class/blob/main/sales_order_data.csv

## 2. Store raw data into HDFS location

- Syntax
```
hadoop fs -put /local_path /hdfs_destination
```
- Command
```
hadoop fs -put sales_order_data.csv /data
```
![image](https://user-images.githubusercontent.com/117569148/227565503-9c0a7521-fbe9-4184-baab-0c501c120e5d.png)


## 3. Create a internal hive table "sales_order_csv" which will store csv data sales_order_csv .. make sure to skip header row while creating table
```
CREATE TABLE sales_order_csv
(
  ordernumber INT,
  quantityordered INT,
  priceeach FLOAT,
  orderlinenumber INT,
  sales FLOAT,
  status STRING,
  qtr_id INT,
  month_id INT,
  year_id INT,
  productline STRING,
  msrp INT,
  productcode STRING,
  phone STRING,
  city STRING,
  state STRING,
  postalcode STRING,
  country STRING,
  teritory STRING,
  contactlastname STRING,
  contactfirstname STRING,
  dealsize STRING
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
TBLPROPERTIES("skip.header.line.count"="1");
```

## 4. Load data from hdfs path into "sales_order_csv" 
```
LOAD DATA INPATH '/data/sales_order_data.csv' INTO TABLE sales_order_csv;
```

## 5. Create an internal hive table which will store data in ORC format "sales_order_orc"
```
CREATE TABLE sales_order_orc
(
  ordernumber INT,
  quantityordered INT,
  priceeach FLOAT,
  orderlinenumber INT,
  sales FLOAT,
  status STRING,
  qtr_id INT,
  month_id INT,
  year_id INT,
  productline STRING,
  msrp INT,
  productcode STRING,
  phone STRING,
  city STRING,
  state STRING,
  postalcode STRING,
  country STRING,
  teritory STRING,
  contactlastname STRING,
  contactfirstname STRING,
  dealsize STRING
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
STORED AS ORC;
```

## 6. Load data from "sales_order_csv" into "sales_order_orc"
```
INSERT OVERWRITE TABLE sales_order_orc SELECT * FROM sales_order_csv;
```

## Perform below menioned queries on "sales_order_orc" table :

- a. Calculate total sales per year
```
SELECT year_id AS year, ROUND(SUM(sales), 2) AS total_sales
FROM sales_order_orc
GROUP BY year_id;
```

- b. Find a product for which maximum orders were placed
```
SELECT productcode, SUM(quantityordered) AS total_orders 
FROM sales_order_orc
GROUP BY (productcode)
ORDER BY total_orders DESC
LIMIT 1;
```

- c. Calculate the total sales for each quarter
```
SELECT year_id AS year, qtr_id AS quarter, ROUND(SUM(sales), 2) AS total_sales
FROM sales_order_orc
GROUP BY year_id, qtr_id
ORDER BY year_id, qtr_id;
```

- d. In which quarter sales was minimum
```
SELECT year_id AS year, qtr_id AS quarter, ROUND(SUM(sales), 2) AS total_sales
FROM sales_order_orc
GROUP BY year_id, qtr_id
ORDER BY total_sales ASC
LIMIT 1;
```

- e. In which country sales was maximum and in which country sales was minimum
```
SELECT country, ROUND(SUM(sales), 2) AS max_sales
FROM sales_order_orc
GROUP BY country
ORDER BY max_sales DESC
LIMIT 1;

SELECT country, ROUND(SUM(sales), 2) AS min_sales
FROM sales_order_orc
GROUP BY country
ORDER BY min_sales
LIMIT 1;
```

- f. Calculate quartelry sales for each city
```
SELECT year_id AS year, qtr_id AS quarter, city, ROUND(SUM(sales), 2) AS total_sales
FROM sales_order_orc
GROUP BY year_id, qtr_id, city;
```

- h. Find a month for each year in which maximum number of quantities were sold
```
SELECT year_id AS year, month_id AS month, SUM(quantityordered) AS max_quantity_ordered
FROM sales_order_orc AS o
GROUP BY year_id, month_id
HAVING SUM(quantityordered) = (SELECT MAX(quantityordered) FROM sales_order_orc AS i WHERE i.year_id = o.year_id GROUP BY i.year_id)
```
