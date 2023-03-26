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














Hive Practical questions:

Hive Join operations

Create a  table named CUSTOMERS(ID | NAME | AGE | ADDRESS   | SALARY)
Create a Second  table ORDER(OID | DATE | CUSTOMER_ID | AMOUNT
)

Now perform different joins operations on top of these tables
(Inner JOIN, LEFT OUTER JOIN ,RIGHT OUTER JOIN ,FULL OUTER JOIN)

BUILD A DATA PIPELINE WITH HIVE

Download a data from the given location - 
https://archive.ics.uci.edu/ml/machine-learning-databases/00360/

1. Create a hive table as per given schema in your dataset 
2. try to place a data into table location
3. Perform a select operation . 
4. Fetch the result of the select operation in your local as a csv file . 
5. Perform group by operation . 
7. Perform filter operation at least 5 kinds of filter examples . 
8. show and example of regex operation
9. alter table operation 
10 . drop table operation
12 . order by operation . 
13 . where clause operations you have to perform . 
14 . sorting operation you have to perform . 
15 . distinct operation you have to perform . 
16 . like an operation you have to perform . 
17 . union operation you have to perform . 
18 . table view operation you have to perform . 






hive operation with python

Create a python application that connects to the Hive database for extracting data, creating sub tables for data processing, drops temporary tables.fetch rows to python itself into a list of tuples and mimic the join or filter operations
