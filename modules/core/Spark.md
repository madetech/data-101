# PySpark: The Basics

![PySpark logo](https://miro.medium.com/max/800/1*VNdaFCkls0gyJR0ddP1PCQ.png "PySpark logo")

## What is PySpark?

Apache Spark + Python = PySpark

Apache Spark is a multi-language engine for executing data engineering, data science, and machine learning on single-node machines or clusters. Apache Spark is written primarily in the Scala programming language. PySpark is an interface for Apache Spark in Python, which is used for real-time, large-scale data processing.

## Exercise
Take a look at [Python and PySpark's installation instructions](https://github.com/madetech/data-101/blob/main/modules/core/Python.md "Python and PySpark's Installation Instructions").

To run this excercise you should be able to use your local Python environment with PySpark enabled.

You could also set up a notebook on a Databricks workspace or use [Databricks Connect](https://docs.databricks.com/dev-tools/databricks-connect.html).

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
  .csv("relative path to 2015-summary.csv file")
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

Adding the code above will show us the [explain plan](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.DataFrame.explain.html "explain plan"), so you should see something like this:

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

6) Below is an example of querying using SQL against a DataFrame:

```sql
sqlWay = spark.sql("""
SELECT DEST_COUNTRY_NAME, count(1)
FROM flight_data_2015
GROUP BY DEST_COUNTRY_NAME
""")
```

Using `sqlWay.explain()` will also show how Spark will execute the query. How could you do this using just DataFrame code?

```python
dataFrameWay = flightData2015\
  .groupBy("DEST_COUNTRY_NAME")\
  .count()

dataFrameWay.explain()
```

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

`.show()` prints out the table and `.printSchema()` the schema.

8) As you can see in the table below, we have renamed the column count to destination_total and then sorted it by descending order:

![Top 5 destinations table](https://i.ibb.co/9NhNKQH/countries-table.png "Top 5 destinations table")

Finally, the DataFrame way to do this exercise and see the table of countries above:

```python
from pyspark.sql.functions import desc

flightData2015\
  .groupBy("DEST_COUNTRY_NAME")\
  .sum("count")\
  .withColumnRenamed("sum(count)", "destination_total")\
  .sort(desc("destination_total"))\
  .limit(5)\
  .show()
```

## Common functions

### select and selectExp

**select** and **selectExpr** allow you to manipulate columns in your dataframe and are the equivalent of SQL queries on table data.  

```sql
SELECT * from myTable
SELECT columnName from myTable
SELECT columnName, someOtherCol as otherCol FROM myTable
```
The easiest way is to just pass column names as strings.

```python
df.select("someColumn").show(2)
```

You can also refer to collumns in a number of different ways

```python
from pyspark.sql.functions import expr, col, column
df.select(
    expr("someColumn"),
    col("someColumn"),
    column("someColumn"),
    expr("someColumn as some_other_name")
).show(2)
```

Shorthand version of **df.select(expr("columnName as some_other_name"))**

```python
df.selectExpr("some_column as some_other_name")
```

### lit

Sometimes we need to pass explicit values into Spark that are just a value (rather then a new column). They could be constant value or something we need to compare to later on. To do this we use literals.

```python
from pyspark.sql.functions import lit
df.select(expr("*"), lit(3.14).alias("Pi")).show(2)
```

In SQL -

```SQL
SELECT *, 3.14 as Pi FROM myTable LIMIT 2
```

### withColumn

Used to add new columns to a DataFrame

```python
df.withColumn("numberOne", lit(1)).show(2)

df.withColumn("withinCountry", expr("ORIGIN_COUNTRY_NAME" == "DEST_COUNTRY_NAME")).show(2)

```

In SQL -

```sql
SELECT *, 1 as numberOne from dfTable LIMIT 2
```

### withColumnRenamed

Rename a column, the first string argument is the current name of the column, the second argument is the new name.

```python
df.withColumnRenamed("DEST_COUNTRY_NAME","dest")
```

## References and Further Reading

1. [Spark: The Definitive Guide Book](https://www.oreilly.com/library/view/spark-the-definitive/9781491912201/ "Spark: The Definitive Guide") by Bill Chambers and Matei Zaharia (Chapter 2)
2. [PySpark Documentation](https://spark.apache.org/docs/latest/api/python/ "PySpark Documentation")
3. [Explore best practices for Spark performance optimization](https://developer.ibm.com/blogs/spark-performance-optimization-guidelines/ "Explore best practices for Spark performance optimization") IBM blog post
