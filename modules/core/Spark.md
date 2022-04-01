# PySpark: The Basics

![PySpark logo](https://miro.medium.com/max/800/1*VNdaFCkls0gyJR0ddP1PCQ.png "PySpark logo")

## What is PySpark?

Spark is a tool for managing and coordinating the execution of tasks on data across a cluster of computers. Apache Spark is written in the Scala programming language. PySpark is an interface for Apache Spark in Python. It not only allows you to write Spark applications using Python APIs, but also provides the PySpark shell for interactively analyzing your data in a distributed environment.

## Exercise
To run this excercise you should be able to use your local Python environment with PySpark enabled.

You could also set up a notebook on a Databricks workspace or use [Databricks Connect](https://docs.databricks.com/dev-tools/databricks-connect.html).

Please download flight-data from the [Spark-The-Definitive-Guide repo](https://github.com/databricks/Spark-The-Definitive-Guide/tree/master/data/flight-data) and put it somewhere that is accessible in your PySpark environment.

For examples and guidance on using PySpark, please refer to the documentation [here](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.SparkSession.html).

1) To begin please read in the csv file '2015-summary.csv using the DataFrameReader with option 'inferSchema' and 'header' set to true

2) Return the first 4 rows of the table as a list by using the .take() command

3) A key part part of pyspark is transformation of data. Return a new dataframe sorted by the column 'count' (the sort command will be useful here and .explain() which will showcase the dataframe lineage) Run .take() command again after the sort to see how this has changed what the top rows are. 

4) A partition in spark is an atomic chunk of data (logical division of data) stored on a node in the cluster. spark.sql.shuffle.partitions configures the number of partitions that are used when shuffling data for joins or aggregations. By default when we perform a shuffle Spark outputs 200 shuffle partitions, experiment with reducing these partitions with 'spark.conf.set'. Depending on the number it may impact runtime. 

5) Next we are going to look at how to run SQL queries against dataframe. There is no performance difference between writing SQL queries or writing DataFrame code. Create a temporary table using the 'createOrReplaceTempView' function.

6) Below is an example of querying using SQl against a dataframe:

```
sqlWay = spark.sql("""
SELECT DEST_COUNTRY_NAME, count(1)
FROM flight_data_2015
GROUP BY DEST_COUNTRY_NAME
""")
```

how could you do this using just DataFrame code? Using sqlWay.explain() will also show how Spark will execute the query. 

7) Now if I wanted to find the top 5 destinations according to this CSV file then I would execute:

```
maxSql = spark.sql("""
SELECT DEST_COUNTRY_NAME, sum(count) as destination_total
FROM flight_data_2015
GROUP BY DEST_COUNTRY_NAME
ORDER BY sum(count) DESC
LIMIT 5
""")

maxSql.show()
maxSql.printSchema()
```

.show() prints out the table and .printSchema() the schema. Do this same operation using DataFrame code. 

8) Rename the column count to destination_total and then sort this by descending order

## Resources

1. Spark: The Definitive Guide by Bill Chambers and Matei Zaharia
2. PySpark documentation https://spark.apache.org/docs/latest/api/python/
