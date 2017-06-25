---
layout: post
title: Running Spark Cluster Mode on Windows Prompt
---

## Starting a Master (in terminal 1)

1. Navigate to c:\spark\bin directory
2. Start the standalone master by running the command as below (Keep in mind the master url e.g. spark://IP:PORT)

```
spark-class org.apache.spark.deploy.master.Master
```

{:start="3"}
3. Check the web UI of spark standalong cluster at http://localhost:8080/ 

## Starting a Worker (in terminal 2)

1. Navigate to c:\spark\bin directory 
2. Start a slave by running the command as below (Use the master url e.g. spark://192.168.0.5:7077)

```
spark-class org.apache.spark.deploy.worker.Worker spark://192.168.0.5:7077
#spark-class org.apache.spark.deploy.worker.Worker --cores 2 --memory 4g spark://192.168.0.5:7077
```

## Running Shell on Cluster Mode

Run the command as below in the terminal

```
pyspark --master spark://192.168.0.5:7077
```

## Submitting a Job on Cluster Mode

1. Navigate to the python file in the terminal

```
cd c:/users/yohan/documents/spark
```

{:start="2"}
2. Submit the job specifying the master url

```
spark-submit --master spark://192.168.0.5:7077 --executor-memory test.py
```

{:start="3"}
3. Kill the client as below

```
spark-class org.apache.spark.deploy.Client kill
```

See more info:
* <a href="https://blog.knoldus.com/2015/04/14/setup-a-apache-spark-cluster-in-your-single-standalone-machine/">Set up a apache spark cluster in your single standalone machine</a>
* <a href="http://spark.apache.org/docs/latest/spark-standalone.html">Spark Standalone Mode</a>



