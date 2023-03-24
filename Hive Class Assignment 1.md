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

```

5. Create an internal hive table which will store data in ORC format "sales_order_orc"

6. Load data from "sales_order_csv" into "sales_order_orc"


Perform below menioned queries on "sales_order_orc" table :

a. Calculatye total sales per year
b. Find a product for which maximum orders were placed
c. Calculate the total sales for each quarter
d. In which quarter sales was minimum
e. In which country sales was maximum and in which country sales was minimum
f. Calculate quartelry sales for each city
h. Find a month for each year in which maximum number of quantities were sold
