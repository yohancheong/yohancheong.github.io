---
layout: post
title: Query Performance SQL vs Spark
---

1. Install [AdventureWorks2014 database](https://msftdbprodsamples.codeplex.com/releases/view/125550) in local sql server
2. Create the slow running script to compare query performance
3. Come up with how to measure the execution time for querying
4. Query the script in SSMS & Export the reulst as the file .csv

```sql
SET STATISTICS TIME ON

SELECT	TOP 10000000 
		th.* 
FROM	Production.TransactionHistory th
JOIN	Production.TransactionHistoryArchive tha ON th.Quantity = tha.Quantity

SET STATISTICS TIME OFF
```
```
(10000000 row(s) affected)

 SQL Server Execution Times:
   *CPU time = 3813 ms,  elapsed time = 99440 ms.*
```

{:start="5"}
5. Query the script in pyspark (standalone) by submitting a job & Export the result as the file .csv

```python
from pyspark import SparkConf, SparkContext, SQLContext
import time

conf = SparkConf().setMaster("local").setAppName("TestingSqlQuery")
sc = SparkContext(conf = conf)

query = """(   
SELECT	TOP 10000000 
		th.* 
FROM	Production.TransactionHistory th
JOIN	Production.TransactionHistoryArchive tha ON th.Quantity = tha.Quantity
) as Alias"""

sqlContext = SQLContext(sc)

start = time.time()
df = sqlContext.read.format("jdbc").options(
  url="jdbc:sqlserver://localhost;databasename=AdventureWorks2014;integratedSecurity=true;", 
  driver="com.microsoft.sqlserver.jdbc.SQLServerDriver",  
  dbtable=query,
  numPartitions=4).load()

df.write.csv('testing-sql-query-result.csv')

end = time.time()
print str(end-start) + ' seconds'
```
```
PS C:\users\yohan\documents\spark> spark-submit testing-sql-query.py
17/05/22 22:06:11 WARN NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
*59.2020001411 seconds*
```