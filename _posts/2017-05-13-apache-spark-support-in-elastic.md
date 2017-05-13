---
layout: post
title: Apache Spark support in Elastic
---

1. Download [elasticsearch-hadoop](https://www.elastic.co/downloads/hadoop) adapter to read/write data in elastic
2. Extract the adaptor under spark directory as c:\spark\elasticsearch-hadoop-5.4.0
3. Open visual studio code as administrator
4. Create the script as below to read elastic index e.g. products/kcosmetics

```python
from pyspark import SparkConf, SparkContext

conf = SparkConf().setMaster("local").setAppName("elasticsearch-hadoop")
sc = SparkContext(conf = conf)

# read in ES index/type "products/kcosmetics"
es_rdd = sc.newAPIHadoopRDD(
    inputFormatClass="org.elasticsearch.hadoop.mr.EsInputFormat",
    keyClass="org.apache.hadoop.io.NullWritable", 
    valueClass="org.elasticsearch.hadoop.mr.LinkedMapWritable", 
    conf={ "es.resource" : "products/kcosmetics" })
print(es_rdd.first())
```

{:start="5"}
5. Run spark-submit --driver-class-path C:\spark\elasticsearch-hadoop-5.4.0\dist\elasticsearch-hadoop-5.4.0.jar script.py This will run the script with elasticsearch-hadoop adaptor to read/write data in elastic
