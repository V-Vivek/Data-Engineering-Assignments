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
- 

16. Difference between "Internal Table" and "External Table" and Mention when to choose “Internal Table” and “External Table” in Hive?
- 

17. Where does the data of a Hive table get stored?
- 

18. Is it possible to change the default location of a managed table?
- 

19. What is a metastore in Hive? What is the default database provided by Apache Hive for metastore?
- 

20. Why does Hive not store metadata information in HDFS?
- 

21. What is a partition in Hive? And Why do we perform partitioning in Hive?
- 

22. What is the difference between dynamic partitioning and static partitioning?
- 

23. How do you check if a particular partition exists?
- 

24. How can you stop a partition form being queried?
- 

25. Why do we need buckets? How Hive distributes the rows into buckets?
- 

26. In Hive, how can you enable buckets?
- 

27. How does bucketing help in the faster execution of queries?
- 

28. How to optimise Hive Performance? Explain in very detail.
- 

29. What is the use of Hcatalog?
- 

30. Explain about the different types of join in Hive.
- 

31. Is it possible to create a Cartesian join between 2 tables, using Hive?
- 

32. Explain the SMB Join in Hive?
- 

33. What is the difference between order by and sort by which one we should use?
- 

34. What is the usefulness of the DISTRIBUTED BY clause in Hive?
- 

35. How does data transfer happen from HDFS to Hive?
- 

36. Wherever (Different Directory) I run the hive query, it creates a new metastore_db, please explain the reason for it?
- 

37. What will happen in case you have not issued the command: ‘SET hive.enforce.bucketing=true;’ before bucketing a table in Hive?
- 

38. Can a table be renamed in Hive?
- 

39. Write a query to insert a new column(new_col INT) into a hive table at a position before an existing column (x_col)
- 

40. What is serde operation in HIVE?
- 

41. Explain how Hive Deserializes and serialises the data?
- 

42. Write the name of the built-in serde in hive.
- 

43. What is the need of custom Serde?
- 

44. Can you write the name of a complex data type(collection data types) in Hive?
- 

45. Can hive queries be executed from script files? How?
- 

46. What are the default record and field delimiter used for hive text files?
- 

47. How do you list all databases in Hive whose name starts with s?
- 

48. What is the difference between LIKE and RLIKE operators in Hive?
- 

49. How to change the column data type in Hive?
- 

50. How will you convert the string ’51.2’ to a float value in the particular column?
- 

51. What will be the result when you cast ‘abc’ (string) as INT?
- 

52. What does the following query do?
```
INSERT OVERWRITE TABLE employees
PARTITION (country, state)
SELECT ..., se.cnty, se.st
FROM staged_employees se;
```
- 

53. Write a query where you can overwrite data in a new table from the existing table.
- 

54. What is the maximum size of a string data type supported by Hive? Explain how Hive supports binary formats.
- 

55. What File Formats and Applications Does Hive Support?
- 

56. How do ORC format tables help Hive to enhance its performance?
- 

57. How can Hive avoid mapreduce while processing the query?
- 

58. What is view and indexing in hive?
- 

59. Can the name of a view be the same as the name of a hive table?
- 

60. What types of costs are associated in creating indexes on hive tables?
- 

61. Give the command to see the indexes on a table.
- 

62. Explain the process to access subdirectories recursively in Hive queries.
- 

63. If you run a select * query in Hive, why doesn't it run MapReduce?
- 

64. What are the uses of Hive Explode?
- 

65. What is the available mechanism for connecting applications when we run Hive as a server?
- 

66. Can the default location of a managed table be changed in Hive?
- 

67. What is the Hive ObjectInspector function?
- 

68. What is UDF in Hive?
- 

69. Write a query to extract data from hdfs to hive.
- 

70. What is TextInputFormat and SequenceFileInputFormat in hive.
- 

71. How can you prevent a large job from running for a long time in a hive?
- 

72. When do we use explode in Hive?
- 

73. Can Hive process any type of data formats? Why? Explain in very detail
- 

74. Whenever we run a Hive query, a new metastore_db is created. Why?
- 

75. Can we change the data type of a column in a hive table? Write a complete query.
- 

76. While loading data into a hive table using the LOAD DATA clause, how do you specify it is a hdfs file and not a local file ?
- 

77. What is the precedence order in Hive configuration?
- 

78. Which interface is used for accessing the Hive metastore?
- 

79. Is it possible to compress json in the Hive external table ?
- 

80. What is the difference between local and remote metastores?
- 

81. What is the purpose of archiving tables in Hive?
- 

82. What is DBPROPERTY in Hive?
- 

83. Differentiate between local mode and MapReduce mode in Hive.
- 
