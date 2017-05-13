---
layout: post
title: Connecting Azure SQL through PySpark
---

1. [Download sqljdbc](https://www.microsoft.com/en-us/download/details.aspx?id=11774) 
2. Extract files in a directory c:/spark
3. Copy & paste c:/spark/sqljdbc_6.0/enu/auth/sqljdb_auth.dll to c:/windows/system32
4. Create test-sql.py as below

```python
from pyspark import SparkContext
from pyspark.sql import SQLContext, Row
from pyspark import SparkConf, SparkContext

conf = SparkConf().setMaster("local").setAppName("My App")
sc = SparkContext(conf = conf)

query = "(SELECT Id from Matter) as mt"

sqlContext = SQLContext(sc)

df = sqlContext.read.format("jdbc").options(
  url="jdbc:sqlserver://domain.database.windows.net;" +
         "databaseName=collaborate-7;user=[USER_ID]];password=[Password]];Integrated Security=False",
  driver="com.microsoft.sqlserver.jdbc.SQLServerDriver",  
  dbtable=query).load()

df.show() 
```

{:start="5"}
5. Run cmd with driver as *spark-submit --driver-class-path c:/spark/sqljdbc_6.0/enu/jre8/sqljdbc42.jar test-sql.py*
6. Output

```python
<class 'pyspark.sql.dataframe.DataFrame'>
+---+
| Id|
+---+
| 12|
| 13|
|  1|
|  7|
|  2|
|  8|
|  9|
| 10|
| 11|
|  3|
+---+
only showing top 10 rows
```

*See more [spark-pyspark-to-extract-from-sql-server](https://community.hortonworks.com/articles/59205/spark-pyspark-to-extract-from-sql-server.html)*