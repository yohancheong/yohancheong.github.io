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

kcosmetics_availability = es_rdd.map(lambda item: ("key",{
    'id': item[0] , ## _id from products/kcosmetics
    'availability': item[1]['availability']
}))

# write the results to "titanic/value_counts"
kcosmetics_availability.saveAsNewAPIHadoopFile(
    path='-', 
    outputFormatClass="org.elasticsearch.hadoop.mr.EsOutputFormat",
    keyClass="org.apache.hadoop.io.NullWritable", 
    valueClass="org.elasticsearch.hadoop.mr.LinkedMapWritable", 
    conf={ 
        "es.index.auto.create": "true", # auto creating index as inserted
        "es.mapping.id": "id",          # auto mapping id as index id
        "es.resource" : "products/kcosmetics_stocks" })
```

{:start="5"}
5. Create the c:/spark/external_jars directory to use external jar, and add the aforementioned jar file to the directory
6. Add the following lines in c:/spark/config/spark-defaults.config to use the jars

```python
spark.driver.extraClassPath			c:/spark/external_jars/*
spark.executor.extraClassPath		c:/spark/external_jars/*
```

{:start="7"}
7. Run **spark-submit script.py** to read data from elastic index i.e. products/kcosmetics

See more information [here](https://www.elastic.co/guide/en/elasticsearch/hadoop/current/spark.html)