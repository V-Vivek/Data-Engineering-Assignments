# Hive Interview Questions

1. What is the definition of Hive? What is the present version of Hive?
- Hive is a Data Warehousing framework built on top of Hadoop. 
- It uses a SQL-like language called HiveQL to analyze and process data.
- The current version of Hive is 4.0.0-aplha-2

2. Is Hive suitable to be used for OLTP systems? Why?
- Hive is not typically used for OLTP (Online Transaction Processing) systems. 
- OLTP systems require low latency, high concurrency, and real-time transaction processing, which are not the primary strengths of Hive.
- Hive is designed for batch processing and is optimized for performing complex queries on large volumes of data.

3. How is HIVE different from RDBMS? Does hive support ACID transactions. If not then give the proper reason.
- Hive is optimized for batch processing of big data, while RDBMS is designed for transactional processing of smaller datasets.
- Hive does not support real-time transactions, whereas RDBMS is optimized for OLTP applications that require real-time transaction processing.
- Hive is a schema-on-read system, which means that the data schema can be flexible and can evolve as new data is added to the system. RDBMS uses a fixed schema model that must be defined upfront and can be challenging to change.
- Hive does not support full ACID transactions by default.

4. Explain the hive architecture and the different components of a Hive architecture?
### Hive Clients
- Hive clients are the tools that users use to interact with Hive. Hive provides several client interfaces, including:

a. Thrift
- Thrift is a lightweight, cross-platform remote procedure call (RPC) framework that is used by Hive to provide a client interface for various programming languages such as Java, Python, C++, and others.

b. JDBC
- JDBC (Java Database Connectivity) is a standard Java API that enables Java programs to interact with databases. 
- Hive provides a JDBC driver that allows Java programs to interact with Hive.

c. ODBC
- ODBC (Open Database Connectivity) is a standard interface for accessing data from a variety of database management systems. 
- Hive provides an ODBC driver that allows applications that support ODBC to interact with Hive.

### Hive Services
- Hive consists of several services that work together to process user queries. The main Hive services are:

#### HiveServer2
- HiveServer2 is the successor of HiveServer1.
- HiveServer1 does not handle concurrent requests from more than one client due to which it was replaced by HiveServer2.
- HiveServer2 is the main service that provides a Thrift interface for clients to interact with Hive.
- It receives queries from clients, compiles them, optimizes them and executes them on the Hadoop cluster.

#### Hive Driver
- The Hive driver receives the HiveQL statements submitted by the user through the command shell. 
- It creates the session handles for the query and sends the query to the compiler.

#### Hive Compiler
- Hive compiler parses the query. 
- It performs semantic analysis and type-checking on the different query blocks and query expressions by using the metadata stored in metastore.
- It generates an execution plan.
- The execution plan created by the compiler is the ***DAG(Directed Acyclic Graph)***, where each stage is a map/reduce job, operation on HDFS, a metadata operation.

#### Optimizer
- Optimizer performs the transformation operations on the execution plan and splits the task to improve efficiency and scalability.

#### Execution Engine
- Execution engine, after the compilation and optimization steps, executes the execution plan created by the compiler in order of their dependencies using Hadoop.

#### Metastore
- Metastore is a central repository that stores the metadata information about the structure of tables and partitions, including column and column type information.
- It also stores information of serializer and deserializer, required for the read/write operation and HDFS files where data is stored. 
- This metastore is generally a relational database.
- Metastore provides a Thrift interface for querying and manipulating Hive metadata.

5. Mention what Hive query processor does? And Mention what are the components of a Hive query processor?
- The Hive query processor is responsible for interpreting HiveQL queries and generating a query plan that can be executed on Hadoop.
- The query processor analyzes the query, determines the tables involved, applies any necessary filters or joins and creates a physical execution plan for the query.

- The components of a Hive query processor are as follows:
- ***Parser:*** The parser component of the query processor is responsible for parsing the HiveQL query and breaking it down into a logical representation that can be processed further. The parser checks for syntax errors and ensures that the query conforms to the HiveQL grammar.

- ***Semantic Analyzer:*** The semantic analyzer component of the query processor performs semantic analysis on the query and validates it against the metadata stored in the metastore. The semantic analyzer checks for errors like missing tables or columns, ensures that the query conforms to the data types of the columns, and performs any necessary type conversions.

- ***Query Optimizer:*** The query optimizer component of the query processor takes the logical query plan generated by the semantic analyzer and optimizes it to improve query performance. The optimizer determines the most efficient way to execute the query by selecting the optimal join order, applying filter pushdown, and choosing the most appropriate file format and compression algorithm.

- ***Execution Engine:*** The execution engine component of the query processor is responsible for executing the physical query plan generated by the optimizer. The execution engine coordinates the execution of map-reduce jobs and other operations necessary to execute the query on Hadoop.

6. What are the three different modes in which we can operate Hive?
- ***Local Mode:*** In local mode, Hive runs in a single JVM without using Hadoop. This mode is typically used for development and testing purposes when the data size is small and does not require the distributed processing capabilities of Hadoop.

- ***MapReduce Mode:*** In MapReduce mode, Hive runs on top of Hadoop and uses the Hadoop MapReduce framework to execute queries. In this mode, Hive translates queries into MapReduce jobs that are distributed across the Hadoop cluster, allowing for parallel processing of large datasets.

- ***Spark Mode:*** In Spark mode, Hive runs on top of Apache Spark and uses the Spark execution engine to execute queries. In this mode, Hive translates queries into Spark jobs that can be executed in memory, providing faster query processing times than MapReduce mode.

7. Features and Limitations of Hive.
- ***Features:*** Scalability, Schema-on-read, SQL-like interface, Integration with Hadoop ecosystem, etc.
- ***Limitations:*** Latency, Lack of real-time updates, Complexity, etc.

8. How to create a Database in HIVE?
```
CREATE DATABASE mydb;
```

9. How to create a table in HIVE?
```
CREATE TABLE mytable
(
  col1 INT,
  col2 STRING,
  col3 DECIMAL
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ',';
```

10. What do you mean by describe and describe extended and describe formatted with respect to database and table
- ***DESCRIBE:*** It is used to display basic metadata about a table
- ***DESCRIBE EXTENDED:*** It is used to display additional metadata about a table such as the location of the table data in HDFS, the input format, output format, serialization format, and storage properties.
- ***DESCRIBE FORMATTED:*** It is used to display even more detailed metadata about a table such as the table parameters, partition information, table statistics, and column statistics.

11. How to skip header rows from a table in Hive?
- We can skip header rows from a table by setting the table property ```skip.header.line.count``` to the number of header rows that you want to skip.
- For e.g. to skip 2 header rows:
```
SET TBLPROPERTIES('skip.header.line.count'='2')
```

12. What is a hive operator? What are the different types of hive operators?
- In Hive, an operator is a symbol or keyword that is used to perform a specific operation on one or more values or expressions. 
- Operators are used in Hive queries to manipulate and transform data.
- There are several types of operators in Hive:

- ***Arithmetic Operators:*** Arithmetic operators are used to perform arithmetic operations on numeric data. The supported arithmetic operators in Hive are +, -, *, /, and % (modulus).

- ***Comparison Operators:*** Comparison operators are used to compare values and return a Boolean result. The supported comparison operators in Hive are =, !=, <, >, <=, and >=.

- ***Logical Operators:*** Logical operators are used to combine Boolean expressions and return a Boolean result. The supported logical operators in Hive are AND, OR, and NOT.

- ***Bitwise Operators:*** Bitwise operators are used to perform bitwise operations on integer data. The supported bitwise operators in Hive are & (bitwise AND), | (bitwise OR), ^ (bitwise XOR), and ~ (bitwise NOT).

- ***String Operators:*** String operators are used to manipulate string data. The supported string operators in Hive are CONCAT, || (concatenation), and LIKE (pattern matching).

- ***Conditional Operators:*** Conditional operators are used to perform conditional operations on data. The supported conditional operators in Hive are CASE, WHEN, THEN, ELSE, and END.

- ***Miscellaneous Operators:*** There are several miscellaneous operators in Hive, such as the CAST operator (used to convert data types), the IS NULL operator (used to check for null values), and the IN operator (used to test for membership in a set of values).

13. Explain about the Hive Built-In Functions
- Hive comes with a wide range of built-in functions that can be used to manipulate and transform data in Hive queries. 
- These functions can be classified into several categories based on their functionality:

- ***Mathematical Functions:*** These functions perform mathematical operations on numeric data. Examples of mathematical functions in Hive include ABS, EXP, LOG, POWER, ROUND, SIGN, SQRT, and TRUNC.

- ***String Functions:*** These functions perform operations on string data. Examples of string functions in Hive include CONCAT, LENGTH, LOWER, UPPER, SUBSTR, TRIM, REPLACE, and REGEXP_REPLACE.

- ***Date/Time Functions:*** These functions perform operations on date and time data. Examples of date/time functions in Hive include CURRENT_DATE, CURRENT_TIMESTAMP, YEAR, MONTH, DAY, HOUR, MINUTE, SECOND, and DATE_FORMAT.

- ***Conditional Functions:*** These functions evaluate a condition and return a value based on whether the condition is true or false. Examples of conditional functions in Hive include CASE, WHEN, IF, and COALESCE.

- ***Aggregate Functions:*** These functions perform calculations on groups of rows and return a single result. Examples of aggregate functions in Hive include SUM, AVG, MIN, MAX, COUNT, and GROUP_CONCAT.

- ***Collection Functions:*** These functions operate on collections of values, such as arrays and maps. Examples of collection functions in Hive include ARRAY, MAP, SIZE, SORT_ARRAY, EXPLODE, and POSEXPL.

- ***Type Conversion Functions:*** These functions convert values from one data type to another. Examples of type conversion functions in Hive include CAST, TO_DATE, TO_UNIX_TIMESTAMP, and FROM_UNIXTIME.

14. Write hive DDL and DML commands.
### DDL Commands:
- ***CREATE DATABASE:*** Creates a new database in Hive.
```
CREATE DATABASE database_name;
```

- ***CREATE TABLE:*** Creates a new table in Hive.
```
CREATE TABLE table_name 
(
  column_name1 data_type1,
  column_name2 data_type2,
  ...
  column_nameN data_typeN
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ',';
```

### DML Commands:
- ***SELECT:*** Retrieves data from one or more tables in Hive.
```
SELECT column_name1, column_name2, ... FROM table_name WHERE condition;
```
- ***INSERT:*** Inserts data into a table in Hive.
```
INSERT INTO table_name (column_name1, column_name2, ...) VALUES (value1, value2, ...);
```

15. Explain about SORT BY, ORDER BY, DISTRIBUTE BY and CLUSTER BY in Hive.
- ***SORT BY:*** This clause is used to sort the output of a Hive query based on one or more columns. SORT BY sorts the data after the reducer phase, which means that all the data is first collected by the reducers and then sorted. SORT BY does not guarantee any specific ordering of the data within the reducers.
```
SELECT name, age FROM employees SORT BY age DESC;
```

- ***ORDER BY:*** This clause is used to sort the output of a Hive query based on one or more columns. ORDER BY sorts the data before the reducer phase, which means that the data is first sorted by the mappers and then sent to the reducers. ORDER BY guarantees a specific ordering of the data within the reducers.
```
SELECT name, age FROM employees ORDER BY age DESC;
```

- ***DISTRIBUTE BY:*** This clause is used to distribute the data across different reducers based on the value of one or more columns. DISTRIBUTE BY determines which reducer each record will go to based on the result of a hash function applied to the column(s) specified in the clause. DISTRIBUTE BY does not guarantee any specific ordering of the data within the reducers.
```
SELECT name, age FROM employees DISTRIBUTE BY age;
```

- ***CLUSTER BY:*** This clause is similar to DISTRIBUTE BY, but it also sorts the data within each reducer based on the value of the column(s) specified in the clause. CLUSTER BY is a combination of DISTRIBUTE BY and SORT BY, and it guarantees a specific ordering of the data within the reducers.
```
SELECT name, age FROM employees CLUSTER BY age DESC;
```

16. Difference between "Internal Table" and "External Table" and Mention when to choose “Internal Table” and “External Table” in Hive?
### ***Internal Table:*** 
- An internal table is a table where the data and metadata are managed by Hive. 
- This means that when you create an internal table, Hive creates a directory in HDFS and stores the data files within that directory.
- Internal tables are tightly coupled with Hive and cannot be accessed or modified by other tools outside of Hive.
- You should choose internal tables when you want to have full control over the data and metadata.Since internal tables are tightly coupled with Hive, you can use all of Hive's features and capabilities to manage the data. Additionally, internal tables provide better performance than external tables since they don't need to read the data from an external source.
```
CREATE TABLE employee (id INT, name STRING, salary FLOAT) STORED AS ORC;
```

### ***External Table:*** 
- An external table is a table where the data is managed by an external tool, and Hive only manages the metadata. 
- This means that when you create an external table, Hive only creates a pointer to the data files stored in HDFS or in an external storage system. 
- External tables are loosely coupled with Hive and can be accessed and modified by other tools outside of Hive.
- You should choose external tables when you want to share data between different tools or systems. Since external tables are loosely coupled with Hive, you can access and modify the data using other tools such as Spark or Pig. Additionally, external tables are useful when you want to store data in a storage system that is not managed by Hive, such as S3 or HBase.
```
CREATE EXTERNAL TABLE employee_external (id INT, name STRING, salary FLOAT) STORED AS ORC LOCATION '/user/hive/employee';
```

17. Where does the data of a Hive table get stored?
- If you create an internal table, Hive creates a directory in HDFS and stores the data files within that directory. 
- If you create an external table, Hive creates a pointer to the data files stored in HDFS or in an external storage system.

18. Is it possible to change the default location of a managed table?
- Yes, it is possible to change the default location of a managed table in Hive.
- We can override the default location by specifying a different location when you create the table.
```
CREATE TABLE my_table (col1 INT, col2 STRING)
LOCATION '/custom/path/to/my_table';
```

19. What is a metastore in Hive? What is the default database provided by Apache Hive for metastore?
- In Hive, the metastore is a central repository that stores metadata about the tables, databases, columns, partitions, and other objects created in Hive. 
- The metastore is used by Hive to track information about the data stored in Hadoop, including the schema of the data, its location and the format of the files. 
- The metastore is typically implemented using RDBMS such as MySQL or PostgreSQL.
- The default database provided by Apache Hive for metastore is Derby.

20. Why does Hive not store metadata information in HDFS?
- Hive does not store metadata information in HDFS because HDFS is designed for storing large files and is optimized for data access and retrieval, rather than efficient metadata management. 
- Storing metadata in HDFS would require a lot of disk I/O operations and could slow down the performance of Hive queries.
- Storing metadata in an RDBMS also provides other advantages, such as transactional consistency and scalability, which are important for large-scale deployments of Hive

21. What is a partition in Hive? And Why do we perform partitioning in Hive?
- In Hive, a partition is a way of dividing a large table into smaller, more manageable parts based on a particular column or set of columns. 
- Each partition contains a subset of the data in the table, with all rows in a partition sharing the same value for the partition column(s).
- Partitioning in Hive is done for several reasons:
- ***Improved query performance:*** By dividing the data into smaller partitions, Hive can skip over entire partitions that are not relevant to a query, reducing the amount of data that needs to be processed.
- ***Easier management:*** Partitioning makes it easier to manage large tables, as you can add, drop, or modify partitions without affecting the entire table.
- ***Better data organization:*** Partitioning allows you to organize your data based on specific criteria, such as time or location, which can make it easier to analyze and understand the data.

22. What is the difference between dynamic partitioning and static partitioning?
### Dynamic Partitioning
- It allows Hive to create partitions automatically based on the values in the data.
- In dynamic partitioning, the partition columns are specified in the INSERT statement itself.
- The dynamic partitioning process requires a large amount of data shuffling and can be resource-intensive.
- Dynamic partitioning is more flexible and easier to use, as it allows for partitions to be created on the fly.

### Static partitioning 
- It requires users to predefine the partitions and their values before loading data into the table.
- In static partitioning, the partition columns are specified in the CREATE TABLE statement itself.
- The static partitioning process is more efficient and less resource-intensive than dynamic partitioning, as the partitioning is done at the time of data loading, and there is no need for data shuffling.
- Static partitioning is suitable for tables with a fixed set of partitions that are known in advance.

23. How do you check if a particular partition exists?
- To check if a particular partition exists in a Hive table, you can use the SHOW PARTITIONS command and the partition filter condition. 
- Here is the syntax for checking if a partition exists:
```
SHOW PARTITIONS table_name [PARTITION(partition_column=value)];
```

24. How can you stop a partition form being queried?
- In Hive, you can prevent a specific partition from being queried by using the ALTER TABLE command to mark it as "offline". 
```
ALTER TABLE table_name PARTITION (partition_column=value) SET OFFLINE;
```

25. Why do we need buckets? How Hive distributes the rows into buckets?
- Bucketing is a way of dividing a large dataset into smaller, more manageable files based on a hash function applied to a chosen column.
- The primary reason to use buckets is to optimize queries that use GROUP BY or JOIN operations. 
- When you bucket a table, Hive will write the rows into separate files based on the hash function applied to the bucketing column. 
- This means that rows that share the same value for the bucketing column will end up in the same bucket file.
- To distribute the rows into buckets, Hive uses a hash function to assign each row to a bucket based on the value of the bucketing column. 
- The hash function ensures that rows with the same value for the bucketing column are assigned to the same bucket

26. In Hive, how can you enable buckets?
- To enable buckets in Hive, you need to create a table with the CLUSTERED BY clause, which specify the bucketing column. 
- You also need to specify the number of buckets you want to create using the INTO n BUCKETS clause, where n is the number of buckets.
```
CREATE TABLE my_table 
(
  id INT,
  name STRING,
  age INT,
  address STRING
)
CLUSTERED BY (id)
INTO 10 BUCKETS;
```

27. How does bucketing help in the faster execution of queries?
- Bucketing can help in faster execution of queries in Hive because it allows for more efficient data retrieval and processing.
- When you execute a query on a bucketed table, Hive can use the bucketing information to optimize the query processing. 
- For example, if you're querying on the bucketing column, Hive can skip reading data from the other buckets that don't contain the relevant data, resulting in faster query execution.
- Additionally, bucketing can also help with data skew, which occurs when certain values in a column are overrepresented in the data.

28. How to optimise Hive Performance? Explain in very detail.
- ***Partitioning:*** Partitioning is a technique used to divide large tables into smaller, more manageable pieces based on the values in one or more columns. By partitioning a table, Hive can skip scanning the irrelevant partitions during query execution, which can significantly reduce query response time. In addition, partitioning can also help to balance data skew and improve data compression.

- ***Bucketing:*** Bucketing is a technique used to organize data into a set of files based on the value of one or more columns. It can be used to improve query performance by reducing the amount of data that needs to be read during query execution. Bucketing is particularly useful when you frequently query on a specific column, as it can allow Hive to skip reading the irrelevant data in other buckets.

- ***Use appropriate file formats:*** Hive supports a variety of file formats, each with its own advantages and disadvantages in terms of performance, compression, and storage. For example, ORC and Parquet are columnar storage formats that can provide significant performance improvements and storage savings for queries that only need to access a subset of columns. Similarly, Gzip and Snappy are popular compression codecs that can help to reduce file size and improve query performance by reducing I/O time.

- ***Data compression:*** Data compression is an effective way to reduce the size of large data sets and improve query performance by reducing I/O time. In Hive, you can compress data using codecs like Gzip, Snappy, and LZO. However, compression can also introduce a CPU overhead during data access, so it's important to find the right balance between compression and decompression time.

29. What is the use of Hcatalog?
- HCatalog is a table and storage management layer for Hadoop that enables users to share data between Apache Hive, Apache Pig, and MapReduce. 
- It is essentially a metadata management service that allows users to store and share metadata about data stored in Hadoop.
- HCatalog provides a common schema for all data stored in Hadoop, regardless of the format or structure, making it easier for different tools and platforms to work with the same data. 
- This also makes it easier for users to access and analyze data from different sources using a single interface.

30. Explain about the different types of join in Hive.
- ***Inner Join:*** An inner join returns only the matching records from both tables. It combines rows from two or more tables where the join condition is true. The resulting table contains only the rows that match in both tables.

- ***Left Outer Join:*** A left outer join returns all the records from the left table and matching records from the right table. If there are no matching records in the right table, the result will contain null values.

- ***Right Outer Join:*** A right outer join returns all the records from the right table and matching records from the left table. If there are no matching records in the left table, the result will contain null values.

- ***Full Outer Join:*** A full outer join returns all the records from both tables. If there are no matching records in one table, the result will contain null values for that table.

31. Is it possible to create a Cartesian join between 2 tables, using Hive?
- es, it is possible to create a Cartesian join between two tables in Hive using the CROSS JOIN or CROSS JOIN ALL clause. 
- The CROSS JOIN clause returns all possible combinations of rows between two tables, which can result in a very large output.

32. Explain the SMB Join in Hive?
- Sort Merge Bucket (SMB) join is an optimization technique used in Hive to improve the performance of join operations. 
- It is a type of join that uses bucketing and sorting to speed up the join process.
- In SMB join, the tables being joined are first bucketed and sorted based on the join key. 
- This allows Hive to perform the join operation without having to shuffle or sort the data again. 
- The join is then performed on the sorted and bucketed data, which improves performance by reducing the amount of data that needs to be read and processed.

33. What is the difference between order by and sort by which one we should use?
- ORDER BY is a clause that is used to sort the entire result set based on one or more columns in ascending or descending order. It guarantees a total order of the result set. When ORDER BY is used, the data is first sorted and then returned to the user. This means that the entire data set needs to be processed before any results can be returned. This can be resource-intensive and slow down the query processing time, particularly for large datasets.

- On the other hand, SORT BY is used to sort the data within each reducer. It does not guarantee a total order of the result set. SORT BY is less resource-intensive than ORDER BY because it does not require the entire data set to be processed before any results can be returned. However, this can result in different partitions being returned in different orders.

- In general, if you need to sort the entire data set in a specific order, ORDER BY is the better choice. However, if you only need to sort the data within each reducer, SORT BY can be faster and more efficient. It is important to note that SORT BY can only be used with a single reducer, so it may not be suitable for large datasets.

34. What is the usefulness of the DISTRIBUTED BY clause in Hive?
- The DISTRIBUTED BY clause in Hive is used to specify how the data should be distributed across the nodes in a Hadoop cluster. 
- This clause is used in conjunction with the CLUSTER BY clause to optimize the performance of queries that involve grouping and aggregation.
- By using the DISTRIBUTED BY clause, Hive can distribute the data in a way that is optimized for the specific query being executed. 
- This can improve the performance of the query by reducing the amount of data that needs to be transferred between nodes and by allowing parallel processing of the data.

35. How does data transfer happen from HDFS to Hive?
- To transfer data from HDFS to Hive, Hive uses a mechanism called a "SerDe" (Serializer/Deserializer) to read and write data in a specific format. 
- The data transfer from HDFS to Hive happens in a distributed manner, with different parts of the data being read by different nodes in the cluster. This allows Hive to process large amounts of data efficiently and in parallel.
- In addition to using SerDes, Hive also supports other data transfer mechanisms such as Apache Thrift and Apache Avro. These mechanisms provide additional flexibility in how data is transferred between Hive and other tools in the Hadoop ecosystem.

36. Wherever (Different Directory) I run the hive query, it creates a new metastore_db, please explain the reason for it?
- Each time you run a Hive query from a different directory, a new metastore_db directory is created in that directory. 
- This is because the embedded Derby database is a file-based database, and it stores its data in files in the directory where it is located.

37. What will happen in case you have not issued the command: ‘SET hive.enforce.bucketing=true;’ before bucketing a table in Hive?
- When you haven't issued the command ```SET hive.enforce.bucketing=true;``` before bucketing a table in Hive, the bucketing will still happen but it will not be enforced. 
- This means that the data may not be evenly distributed across the buckets, and queries that rely on bucketing may not work as efficiently.

38. Can a table be renamed in Hive?
- Yes, a table can be renamed in Hive using the ALTER TABLE statement with the RENAME TO clause. 
```
ALTER TABLE old_table_name RENAME TO new_table_name;
```

39. Write a query to insert a new column(new_col INT) into a hive table at a position before an existing column (x_col)
```
ALTER TABLE h_table
CHANGE COLUMN new_col INT
BEFORE x_col;
```

40. What is serde operation in HIVE?
- In Hive, SerDe (Serializer/Deserializer) stands for a set of built-in or external libraries that helps in the serialization and deserialization of the data in Hive tables. 
- It is used to convert the structured data stored in HDFS or other storage systems into a format that Hive can use and vice versa. 
- SerDe enables Hive to read and write data in various formats such as JSON, CSV, ORC, and Parquet.

41. Explain how Hive Deserializes and serialises the data?
- In Hive, SerDe stands for Serializer/Deserializer. It is a way to parse data between Hive and data storage formats like CSV, JSON, and TSV. SerDe in Hive can deserialize or serialize the data into the storage formats during reading and writing operations.

- When a query is executed in Hive, the SerDe comes into action and converts the input data from the storage format to a format that can be processed by Hive. The deserialization process is done by the SerDe in the InputFormat, which is responsible for reading the data from the storage format and creating a list of objects for processing.

- Similarly, during the output process, the SerDe in the OutputFormat serializes the processed data and converts it back to the storage format for storage.

- The SerDe operations in Hive are configurable, and it allows us to use different SerDe implementations for different data formats. Additionally, users can also develop custom SerDe libraries for different data formats that are not supported by Hive by default.

42. Write the name of the built-in serde in hive.
- The built-in serde in Hive is called LazySimpleSerDe.

43. What is the need of custom Serde?
- LazySimpleSerDe, can handle only simple text data in a delimited format. 
- However, there may be situations where the data is stored in a different format, such as JSON, Avro, or ORC, and the default SerDe cannot handle it. In such cases, a custom SerDe can be written to handle the specific data format.

44. Can you write the name of a complex data type(collection data types) in Hive?
- Array
- Map
- Struct
- Union

45. Can hive queries be executed from script files? How?
- Yes, Hive queries can be executed from script files. We can create a script file with the extension ".hql" and write the Hive queries in it.
- We can use the Hive CLI command source followed by the path of the script file to execute the Hive queries in the script.
```
source /path/to/script.hql;
```

46. What are the default record and field delimiter used for hive text files?
- The default record delimiter used for Hive text files is the newline character (\n), and the default field delimiter is the tab character (\t).

47. How do you list all databases in Hive whose name starts with s?
```
SHOW DATABASES LIKE 's*';
```

48. What is the difference between LIKE and RLIKE operators in Hive?
- ***LIKE operator***: It is used to match a string pattern with a specified string, where the pattern may include % and _ wildcards.
```
SELECT * FROM my_table WHERE name LIKE 'J%';
```

- ***RLIKE operator***: It is used to match a string pattern using a regular expression.
```
SELECT * FROM my_table WHERE name RLIKE '.*apple.*';
```

49. How to change the column data type in Hive?
```
ALTER TABLE table_name
CHANGE column_name column_name new_data_type;
```

50. How will you convert the string ’51.2’ to a float value in the particular column?
```
SELECT CAST(column_name AS FLOAT) FROM table_name;
```

  51. What will be the result when you cast ‘abc’ (string) as INT?
- It will result in a NULL value as 'abc' cannot be converted to an integer value.

52. What does the following query do?
```
INSERT OVERWRITE TABLE employees
PARTITION (country, state)
SELECT ..., se.cnty, se.st
FROM staged_employees se;
```
- It inserts data into the "employees" table by selecting data from another table "staged_employees". It also partitions the data by country and state columns.

53. Write a query where you can overwrite data in a new table from the existing table.
```
INSERT OVERWRITE TABLE new_table
SELECT *
FROM existing_table;
```

54. What is the maximum size of a string data type supported by Hive? Explain how Hive supports binary formats.
- In Hive, the maximum size of a string data type is 2 GB.
- Hive supports binary formats through the use of SerDes. SerDes are responsible for de-serializing binary data from HDFS into Hive tables and serializing data back from Hive tables into binary data stored in HDFS.

55. What File Formats and Applications Does Hive Support?
- Hive supports various file formats, including text files, sequence files, ORC files, and Parquet files.
- Hive also supports various applications such as Apache Tez, Apache Spark, and Apache HBase.

56. How do ORC format tables help Hive to enhance its performance?
- ORC (Optimized Row Columnar) format tables in Hive help to enhance its performance in several ways:

- ***Compression:*** ORC format tables support highly efficient compression techniques such as Zlib, Snappy, and LZO. This reduces the storage space required by the tables, resulting in faster I/O operations.

- ***Encoding:*** ORC format tables support advanced encoding techniques such as Dictionary Encoding, Run Length Encoding, and Bit Packing. This encoding technique reduces the data size of the table and improves query performance.

- ***Column Pruning:*** ORC format tables store column statistics, which allows Hive to prune unnecessary columns during query execution. This reduces the amount of data that needs to be read from disk, resulting in faster query execution.

- ***Predicate Pushdown:*** ORC format tables support predicate pushdown, which means that the filtering of records can be pushed down to the storage layer. This reduces the amount of data that needs to be processed, resulting in faster query execution.

- ***Vectorization:*** ORC format tables support vectorization, which allows Hive to perform operations on multiple rows of data at once, resulting in faster query execution.

57. How can Hive avoid mapreduce while processing the query?
- To avoid MapReduce while processing the query we can set the ```hive.exec.mode.local.auto=true```.

58. What is view and indexing in hive?
- A view is a virtual table that does not store data on its own but instead displays the result of a query on other tables. A view is created using a SELECT statement, and its output is treated as a regular table.

- An index is a structure that is created on one or more columns of a table to speed up the query processing. An index allows faster data retrieval, as it stores the pointer to the actual data location on disk.

59. Can the name of a view be the same as the name of a hive table?
- Hive allows views and tables to have the same name as long as they are in different databases.

60. What types of costs are associated in creating indexes on hive tables?
- ***Creation cost:*** This is the cost of creating the index structure, which involves reading the data from the table and building the index. The time taken for creating the index structure depends on the size of the table and the number of columns on which the index is created.

- ***Maintenance cost:*** This is the cost associated with updating the index whenever new data is added to or deleted from the table. The more frequently the table is updated, the higher the maintenance cost.

- ***Storage cost:*** The storage cost for indexes is usually small compared to the table data, but it can still be significant for large tables with many indexes.

- ***Query processing cost:*** Query processing cost may be reduced when indexes are used, as they can speed up the query processing by reducing the amount of data that needs to be scanned.

61. Give the command to see the indexes on a table.
```
SHOW INDEXES ON table_name;
```

62. Explain the process to access subdirectories recursively in Hive queries.
- To access subdirectories recursively we can use the INPUT__FILE__NAME virtual column. 
- This virtual column provides the full path of the input file being processed by the mapper. 
- By extracting the directory path from this column, we can access the subdirectories recursively in Hive queries.
- Here's an example query to access subdirectories recursively in Hive:
```
SELECT *
FROM my_table
WHERE input__file__name LIKE '/path/to/directory/%'
```
- The % character is used as a wildcard to match any subdirectory under the specified directory.

63. If you run a select * query in Hive, why doesn't it run MapReduce?
- When a user runs a SELECT * query in Hive, it does not run MapReduce because Hive stores metadata about the data in its metastore, which includes the schema of the table and the location of the data in HDFS. When the query is executed, Hive uses this metadata to read the data directly from HDFS without the need for a MapReduce job. 

64. What are the uses of Hive Explode?
- explode() is a built-in function used to transform elements in a collection into individual rows. 
- The function takes a collection (such as an array or map) as input and generates a new row for each element in the collection.
- Here are some use cases for explode() in Hive:

- ***Flattening arrays:*** If a table column contains an array of values, explode() can be used to split the array into individual rows. This is useful when you want to analyze the individual elements of an array.

- ***Exploding maps:*** If a table column contains a map of key-value pairs, explode() can be used to split the map into individual rows. This is useful when you want to analyze the individual key-value pairs in a map.

- ***Joining tables:*** explode() can be used to join two tables based on a common column that contains a collection. For example, if one table has a column that contains an array of IDs and another table has a column that contains a single ID, explode() can be used to join the two tables by matching each ID in the array with the single ID.

- ***Cross-joining:*** explode() can be used to perform a cross-join between a table and a collection. For example, if a table has a column that contains an array of values and you want to cross-join the table with each element in the array, you can use explode() to generate the necessary rows.

65. What is the available mechanism for connecting applications when we run Hive as a server?
- ***JDBC (Java Database Connectivity):*** This is a standard Java API that allows Java programs to access databases, including Hive. JDBC provides a set of interfaces and classes for database connectivity and allows users to execute SQL queries and retrieve data from a database.

- ***ODBC (Open Database Connectivity):*** This is a standard interface for connecting applications to databases. ODBC provides a set of functions for database connectivity and allows applications to execute SQL queries and retrieve data from a database. Hive provides an ODBC driver that allows applications to connect to Hive using ODBC.

66. Can the default location of a managed table be changed in Hive?
- es, the default location of a managed table in Hive can be changed. It can be done by specifying the LOCATION clause while creating a table or by altering the table location using the ALTER TABLE command.
```
CREATE TABLE my_table (col1 INT, col2 STRING)
LOCATION '/my/custom/location';
```

67. What is the Hive ObjectInspector function?
- In Hive, the ObjectInspector function is used to inspect the data types of objects in the Hive runtime environment. It is a built-in function that is used by Hive to serialize and deserialize data between the MapReduce framework and the Hive runtime environment.

68. What is UDF in Hive?
- In Hive, a UDF is a function that can be defined by the user to manipulate and process data during the query execution.
- Hive allows users to define their own UDFs to perform custom operations that are not available in the built-in functions. 
- UDFs can be written in various programming languages like Java, Python, and Scala.

69. Write a query to extract data from hdfs to hive.
```
LOAD DATA INPATH '/path/to/hdfs/file' OVERWRITE INTO TABLE table_name;
```

70. What is TextInputFormat and SequenceFileInputFormat in hive.
- ***TextInputFormat*** is the default file input format used in Hive, which reads input files as plain text files and represents each line in the file as a separate record.

- ***SequenceFileInputFormat*** is a binary file input format that allows users to store complex data structures in a compressed binary format. It reads data from files stored in Hadoop's SequenceFile format, which can be used to store large amounts of structured data in a compact and efficient way.

71. How can you prevent a large job from running for a long time in a hive?
- By setting the MapReduce jobs to execute in strict mode ```set hive.mapred.mode=strict;```
- The strict mode ensures that the queries on partitioned tables cannot execute without defining a WHERE clause.
- The number of reducers should be set appropriately based on the data size and the available resources. Setting too few reducers can cause the job to run for a long time, while setting too many can result in unnecessary overhead.

72. When do we use explode in Hive?
- explode() function is used to transform a collection of items in a single column of a table into multiple rows. 
- It takes an array or a map column as input and returns a new row for each element in the array or map. 
- This is useful when dealing with complex data types such as arrays and maps, which may contain multiple values that need to be processed individually.

73. Can Hive process any type of data formats? Why? Explain in very detail
- Hive can process a variety of data formats, including text files, ORC files, Parquet files, and Avro files, among others. 
- This is possible because Hive uses a pluggable storage handler interface, which allows users to define their own input/output formats and SerDes for custom data formats.
- Hive supports text files with various delimiters, including tab-separated, comma-separated, and other user-defined delimiters. 
- It can also handle binary data formats such as ORC, Parquet, and Avro, which offer higher performance and compression compared to text files.

74. Whenever we run a Hive query, a new metastore_db is created. Why?
- The metastore in Hive is a repository that stores the metadata of all the tables, views, partitions, and databases in Hive. 
- It also stores the mapping between HDFS data and Hive tables. Whenever we run a Hive query, it accesses the metastore to retrieve the metadata of the tables being used in the query.
- However, a new metastore_db should not be created every time a Hive query is run. 
- The metastore_db is created only once when we initialize Hive for the first time. After that, Hive reuses the same metastore_db for subsequent queries.
- If a new metastore_db is being created every time a Hive query is run, it could be due to a misconfiguration in Hive or a permission issue.

75. Can we change the data type of a column in a hive table? Write a complete query.
- Yes, we can change the data type of a column in a Hive table using the ALTER TABLE command. Here is an example query:
```
ALTER TABLE my_table
CHANGE COLUMN column_name column_new_datatype
```

76. While loading data into a hive table using the LOAD DATA clause, how do you specify it is a hdfs file and not a local file ?
- To specify that the data being loaded using the LOAD DATA clause is from HDFS and not a local file, we need to specify the full HDFS path in the location parameter of the command.
```
LOAD DATA INPATH '/user/data/' INTO TABLE sample_table;
```

77. What is the precedence order in Hive configuration?
- The order of precedence in Hive configuration is as follows:
1. Configuration property value set explicitly in the query or script.
2. Configuration property value set for the Hive session.
3. Configuration property value set in hive-site.xml configuration file.
4. Configuration property value set in the Hadoop configuration file (core-site.xml, hdfs-site.xml, mapred-site.xml, etc.).
- If a property is not found in any of the above levels, then the default value is used.

78. Which interface is used for accessing the Hive metastore?
- The Hive Metastore can be accessed using the Thrift service API provided by the Hive Metastore Server. 
- The Hive CLI and HiveServer2 are two other commonly used interfaces to interact with the Hive metastore.

79. Is it possible to compress json in the Hive external table ?
- Yes, it is possible to compress JSON data in Hive external tables. You can use compression codecs such as Gzip, Bzip2, or Snappy to compress the data in JSON format.
- To compress JSON data, you can specify the compression codec while creating the external table.

80. What is the difference between local and remote metastores?
- ***Local Metastore:*** In this case, the metastore is created and managed locally on the same machine as the Hive service. This is the default configuration for Hive, and it is suitable for small to medium-sized deployments. However, a local metastore can become a bottleneck if multiple Hive services are accessing it simultaneously, or if the number of tables and partitions in the system grows very large.

- ***Remote Metastore:*** A remote metastore is a separate database service that is dedicated to storing and managing Hive metadata. The advantage of using a remote metastore is that it can handle a large number of tables and partitions more efficiently, and it can be shared by multiple Hive services running on different machines. A remote metastore also provides better fault tolerance and scalability, as the metadata is stored separately from the Hive service.

81. What is the purpose of archiving tables in Hive?
- Archiving tables in Hive means moving a table or partition and its associated data from its current location to a new location, while still retaining the ability to query the data. 
- The main purpose of archiving tables is to free up space in the current location for new data or to optimize the query performance.
- In addition, archiving tables can also be useful for backup and disaster recovery purposes. By maintaining archived copies of data, an organization can quickly recover data in the event of a system failure or other data loss event.

82. What is DBPROPERTY in Hive?
- DBPROPERTY is a Hive function that allows you to retrieve metadata information about the current database. It returns the value of the specified property for the current database.

83. Differentiate between local mode and MapReduce mode in Hive.
- The main differences between local mode and MapReduce mode in Hive are:

- ***Execution:*** In local mode, the query runs on the local file system of the machine where Hive is installed, whereas in MapReduce mode, the query is executed as a MapReduce job on the Hadoop cluster.

- ***Scalability:*** Local mode is suitable for testing small amounts of data or running ad-hoc queries, whereas MapReduce mode is used for processing large amounts of data and executing complex queries.

- ***Performance:*** MapReduce mode can provide better performance than local mode, especially for large data sets, because it takes advantage of the distributed processing power of the Hadoop cluster.

- ***Dependencies:*** In local mode, the Hive query does not have any dependencies on the Hadoop cluster, whereas in MapReduce mode, the Hive query requires a Hadoop cluster to be set up and configured properly.
