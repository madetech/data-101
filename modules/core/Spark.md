# PySpark: The Basics

![PySpark logo](https://miro.medium.com/max/800/1*VNdaFCkls0gyJR0ddP1PCQ.png "PySpark logo")

## What is PySpark?

Apache Spark + Python = PySpark

Spark is a tool for managing and coordinating the execution of tasks on data across a cluster of computers. Apache Spark is written primarily in the Scala programming language. PySpark is an interface for Apache Spark in Python, which is used for real-time, large-scale data processing. It not only allows you to write Spark applications using Python APIs, but also provides the PySpark shell for interactively analyzing your data in a distributed environment.

## Exercise
Take a look at [PySpark's installation instructions](https://spark.apache.org/docs/latest/api/python/getting_started/install.html "PySpark's Installation Instructions").

To run this excercise you should be able to use your local Python environment with PySpark enabled.

You could also set up a notebook on a Databricks workspace or use [Databricks Connect](https://docs.databricks.com/dev-tools/databricks-connect.html).

If you are stuck on setting up the project, refer to the instructions on [this](https://github.com/databricks/Spark-The-Definitive-Guide "this") page.

Please download **flight-data** from the [Spark-The-Definitive-Guide repo](https://github.com/databricks/Spark-The-Definitive-Guide/tree/master/data/flight-data) and put it somewhere that is accessible in your PySpark environment.

For examples and guidance on using PySpark, please refer to the documentation [here](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.SparkSession.html).

1) To begin, read in the csv file '2015-summary.csv using the DataFrameReader with option 'inferSchema' and 'header' set to true. A DataFrame is the most common Structured API and simply represents a table of data with rows and columns.

```python
from pyspark.context import SparkContext
from pyspark.sql.session import SparkSession
sc = SparkContext('local')
spark = SparkSession(sc)

flightData2015 = spark\
  .read\
  .option("inferSchema", "true")\
  .option("header", "true")\
  .csv("csv/2015-summary.csv")
```

2) Now, return the first 4 rows of the table as a list by using the `.take()` command

```python
x = flightData2015.take(4)
print(x)
```
You should see the following output:

    [Row(DEST_COUNTRY_NAME='United States', ORIGIN_COUNTRY_NAME='Romania', count=15), Row(DEST_COUNTRY_NAME='United States', ORIGIN_COUNTRY_NAME='Croatia', count=1), Row(DEST_COUNTRY_NAME='United States', ORIGIN_COUNTRY_NAME='Ireland', count=344), Row(DEST_COUNTRY_NAME='Egypt', ORIGIN_COUNTRY_NAME='United States', count=15)]

3) A key part of PySpark is transformation of data. Return a new DataFrame sorted by the column 'count' (the `sort` command will be useful here and `.explain()` which will showcase the DataFrame lineage).

```python
flightData2015.sort("count").explain()
```

Adding the code above will show us the explain plan, so you should see something like this:

    == Physical Plan ==
    *Sort [count#195 ASC NULLS FIRST], true, 0
    +- Exchange rangepartitioning(count#195 ASC NULLS FIRST, 200)
       +- *FileScan csv [DEST_COUNTRY_NAME#193,ORIGIN_COUNTRY_NAME#194,count#195] ...

Run `.take()` command again after the sort to see how this has changed what the top rows are.

```python
y = flightData2015.sort("count").take(2)
print(y)
```
The output you should be getting:

    [Row(DEST_COUNTRY_NAME='United States', ORIGIN_COUNTRY_NAME='Singapore', count=1), Row(DEST_COUNTRY_NAME='Moldova', ORIGIN_COUNTRY_NAME='United States', count=1)]

4) A partition in Spark is an atomic chunk of data (logical division of data) stored on a node in the cluster. `spark.sql.shuffle.partitions` configures the number of partitions that are used when shuffling data for joins or aggregations. By default, when we perform a shuffle Spark outputs 200 shuffle partitions, experiment with reducing these partitions with 'spark.conf.set'. Depending on the number it may impact runtime.

```python
spark.conf.set("spark.sql.shuffle.partitions", "5")
```

5) Next, we are going to look at how to run SQL queries against DataFrame. There is no performance difference between writing SQL queries or writing DataFrame code. Create a temporary table using the `createOrReplaceTempView` function.

```python
flightData2015.createOrReplaceTempView("flight_data_2015")
```

6) Below is an example of querying using SQL against a dataframe:

```sql
sqlWay = spark.sql("""
SELECT DEST_COUNTRY_NAME, count(1)
FROM flight_data_2015
GROUP BY DEST_COUNTRY_NAME
""")
```

How could you do this using just DataFrame code? Using `sqlWay.explain()` will also show how Spark will execute the query.

7) Now, if I wanted to find the top 5 destinations according to this CSV file then I would execute:

```sql
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

`.show()` prints out the table and `.printSchema()` the schema. Do this same operation using DataFrame code.

8) As you can see in the table below, we have renamed the column count to destination_total and then sorted it by descending order:

![Top 5 destinations table](https://i.ibb.co/9NhNKQH/countries-table.png "Top 5 destinations table")

## References and Further Reading

1. [Spark: The Definitive Guide Book](https://www.oreilly.com/library/view/spark-the-definitive/9781491912201/ "Spark: The Definitive Guide") by Bill Chambers and Matei Zaharia (Chapter 2)
2. [PySpark Documentation](https://spark.apache.org/docs/latest/api/python/ "PySpark Documentation")
3. [Explore best practices for Spark performance optimization](https://developer.ibm.com/blogs/spark-performance-optimization-guidelines/ "Explore best practices for Spark performance optimization") IBM blog post
