# Scenario Based questions:

## Will the reducer work or not if you use “Limit 1” in any HiveQL query?
- The LIMIT clause is used to limit the no. of rows in a select statement and it will not affect the functionality of reducer.

## Suppose I have installed Apache Hive on top of my Hadoop cluster using default metastore configuration. Then, what will happen if we have multiple clients trying to access Hive at the same time? 
- The default configuration uses a single instance of the Hive Server, which can become a bottleneck if multiple clients are trying to access simultaneously.
- Hence, if multiple clients try to access the Hive at the same time, they will get an error as it cannot accept concurrent requests.

## Suppose, I create a table that contains details of all the transactions done by the customers: 
```
CREATE TABLE transaction_details
(
  cust_id INT, 
  amount FLOAT, 
  month STRING, 
  country STRING
) 
ROW FORMAT DELIMITED 
FIELDS TERMINATED BY ',';
```
Now, after inserting 50,000 records in this table, I want to know the total revenue generated for each month. But, Hive is taking too much time in processing this query. How will you solve this problem and list the steps that I will be taking in order to do so?
- To solve this problem we can use partitioning of table
- We will create a new table schema with 'month' as partition column
```
CREATE TABLE transaction_details_partitioned
(
  cust_id INT, 
  amount FLOAT, 
  country STRING
)
PARTITIONED BY (month STRING)
ROW FORMAT DELIMITED 
FIELDS TERMINATED BY ',';
```
- We will use dynamic partitioning and hence we need to enable the relevant properties.
```
SET hive.exec.dynamic.partition = true;
SET hive.exec.dynamic.partition.mode = nonstrict;
```
- Then we will insert data from original table into partitioned table
```
INSERT OVERWRITE TABLE transaction_details_partitioned PARTITION (month) 
SELECT cust_id, amount, country, month FROM transaction_details;
```


## How can you add a new partition for the month December in the above partitioned table?
```
ALTER TABLE transaction_details_partitioned ADD PARTITION (month='Dec');
```

## I am inserting data into a table based on partitions dynamically. But, I received an error – FAILED ERROR IN SEMANTIC ANALYSIS: Dynamic partition strict mode requires at least one static partition column. How will you remove this error?
- We need to enble below properties to eliminate the error
```
SET hive.exec.dynamic.partition = true;
SET hive.exec.dynamic.partition.mode = nonstrict;
```

## Suppose, I have a CSV file – ‘sample.csv’ present in ‘/temp’ directory with the following entries: id first_name last_name email gender ip_address How will you consume this CSV file into the Hive warehouse using built-in SerDe?
- By using the SerDe for CSV data type
```
CREATE EXTERNAL TABLE my_table
(
  id INT, 
  first_name STRING, 
  last_name STRING, 
  email STRING,
  gender STRING, 
  ip_address STRING
)
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.OpenCSVSerde'
WITH SERDEPROPERTIES
(
  "separatorChar" = ",",
  "quoteChar" = "\"",
  "escapeChar" = "\\"
)
STORED AS TEXTFILE
LOCATION '/temp';
```

## Suppose, I have a lot of small CSV files present in the input directory in HDFS and I want to create a single Hive table corresponding to these files. The data in these files are in the format: {id, name, e-mail, country}. Now, as we know, Hadoop performance degrades when we use lots of small files.
### So, how will you solve this problem where we want to create a single Hive table for lots of small files without degrading the performance of the system?
- We can use ```STORED AS SEQUENCEFILE``` to improve the performance of the system
- Table Schema
```
CREATE TABLE my_table
(
  id INT,
  name STRING,
  email STRING,
  country STRING
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
STORED AS SEQUENCEFILE;
```
- Loading the data
```
LOAD DATA INPATH '/input/' INTO TABLE my_table;
```

## The following statement failed to execute. What can be the cause?
```
LOAD DATA LOCAL INPATH ‘Home/country/state/’
OVERWRITE INTO TABLE address;
```
- The path specified in the above query is invalid. As it is pointing to a directory & not to a specific file.
- Corrected query
```
LOAD DATA LOCAL INPATH 'Home/country/state/address.csv'
OVERWRITE INTO TABLE address;
```

## Is it possible to add 100 nodes when we already have 100 nodes in Hive? If yes, how?
- Install Hadoop & Hive in new nodes
- Configure them to connect to existing Hadoop cluster
- Join the new nodes to the cluster
  
  
# Hive Practical questions:

## Hive Join operations
### Create a  table named CUSTOMERS(ID | NAME | AGE | ADDRESS   | SALARY)
```
CREATE TABLE customers 
(
   id INT,
   name STRING,
   age INT,
   address STRING,
   salary FLOAT
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
STORED AS ORC;
```

### Create a Second  table ORDER(OID | DATE | CUSTOMER_ID | AMOUNT)
```
CREATE TABLE orders (
   oid INT,
   `date` DATE,
   customer_id INT,
   amount FLOAT
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
STORED AS ORC;
```

## Now perform different joins operations on top of these tables
- Inner JOIN
```
SELECT id, name, age, address, salary, oid, `date`, amount
FROM customers c JOIN orders o
ON c.id = o.customer_id;
```

- LEFT OUTER JOIN
```
SELECT id, name, age, address, salary, oid, `date`, amount
FROM customers c LEFT JOIN orders o
ON c.id = o.customer_id;
```

- RIGHT OUTER JOIN
```
SELECT id, name, age, address, salary, oid, `date`, amount
FROM customers c RIGHT JOIN orders o
ON c.id = o.customer_id;
```

- FULL OUTER JOIN
```
SELECT id, name, age, address, salary, oid, `date`, amount
FROM customers c FULL OUTER JOIN orders o
ON c.id = o.customer_id;
```


## BUILD A DATA PIPELINE WITH HIVE

Download a data from the given location - 
https://archive.ics.uci.edu/ml/machine-learning-databases/00360/

1. Create a hive table as per given schema in your dataset 
```
CREATE TABLE air_quality_uci 
(
  `Date` STRING,
  Time STRING,
  CO_GT STRING,
  PT08_S1_CO INT,
  NMHC_GT INT,
  C6H6_GT STRING,
  PT08_S2_NMHC INT,
  NOx_GT INT,
  PT08_S3_NOx INT,
  NO2_GT INT,
  PT08_S4_NO2 INT,
  PT08_S5_O3 INT,
  T STRING,
  RH STRING,
  AH STRING
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ';'
TBLPROPERTIES("skip.header.line.count"="1");
```

2. try to place a data into table location
```
LOAD DATA LOCAL INPATH 'file:///opt/AirQualityUCI.csv' INTO TABLE air_quality_uci;
```

3. Perform a select operation
```
SELECT * FROM air_quality_uci LIMIT 10;
```
![image](https://user-images.githubusercontent.com/117569148/228114845-e9602267-ed3c-420f-b2dd-a5ab6cba0305.png)

4. Fetch the result of the select operation in your local as a csv file
```
INSERT OVERWRITE LOCAL DIRECTORY '/vivek/my_exported_data.csv'
ROW FORMAT DELIMITED 
FIELDS TERMINATED BY ',' 
SELECT * FROM air_quality_uci 
LIMIT 10;
```
![image](https://user-images.githubusercontent.com/117569148/228114734-324b021b-8778-464f-9fec-224232e4d9d7.png)

5. Perform group by operation
```
SELECT `date`, ROUND(AVG(T), 2)
FROM air_quality_uci
GROUP BY `date`
LIMIT 10;
```

6. Perform filter operation at least 5 kinds of filter examples
- Example #1
```
SELECT *
FROM air_quality_uci
WHERE t < 20
LIMIT 10;
```
![image](https://user-images.githubusercontent.com/117569148/228409841-6b1d28a9-1bb5-4277-bf0b-fa92e456f5fc.png)

- Example #2
```
SELECT *
FROM air_quality_uci
WHERE t = 20
LIMIT 10;
```
![image](https://user-images.githubusercontent.com/117569148/228410082-1affc60a-d844-4c46-b6c3-ca47e1ca9372.png)

- Example #3
```
SELECT *
FROM air_quality_uci
WHERE co_gt BETWEEN 0.5 AND 2.5
LIMIT 10;
```
![image](https://user-images.githubusercontent.com/117569148/228410459-66351c4e-80e9-4e22-b19d-8020d9749b55.png)

- Example #4
```
SELECT *
FROM air_quality_uci
WHERE `date` LIKE "1%"
LIMIT 10;
```
![image](https://user-images.githubusercontent.com/117569148/228410666-466300ac-5e7e-46b7-9e63-cf10f1176224.png)

- Example #5
```
SELECT *
FROM air_quality_uci
WHERE rh IS NOT NULL
LIMIT 10;
```
![image](https://user-images.githubusercontent.com/117569148/228410965-3606916c-170a-41f6-b8a7-23fd23285162.png)


7. show and example of regex operation
8. alter table operation 
9. drop table operation
10. order by operation
11. where clause operations you have to perform 
12. sorting operation you have to perform
13. distinct operation you have to perform
14. like an operation you have to perform
15. union operation you have to perform
16. table view operation you have to perform


```
INSERT OVERWRITE TABLE air_quality_uci
SELECT 
  `Date`,
  Time,
  regexp_replace(CO_GT, ',', '.') as CO_GT,
  PT08_S1_CO,
  NMHC_GT,
  regexp_replace(C6H6_GT, ',', '.') as C6H6_GT,
  PT08_S2_NMHC,
  NOx_GT,
  PT08_S3_NOx,
  NO2_GT,
  PT08_S4_NO2,
  PT08_S5_O3,
  regexp_replace(T, ',', '.') as T,
  regexp_replace(RH, ',', '.') as RH,
  regexp_replace(AH, ',', '.') as AH
FROM air_quality_uci;
```



hive operation with python

Create a python application that connects to the Hive database for extracting data, creating sub tables for data processing, drops temporary tables.fetch rows to python itself into a list of tuples and mimic the join or filter operations
