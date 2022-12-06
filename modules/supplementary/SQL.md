# **SQL**

 SQL stands for _`Structured Query Language`_. SQL is a popular data query language used for data retrieval and data manipulation, developed since the 1970s by IBM.
 It is used to store, retrieve and manipulate data in databases.

## Key Concepts 

 - Data can be queried and analyzed using SQL.  Relational Databases organize and store data into one or more tables that are related to each other (e.g.Orders and customers; Product and Product_details, etc.). This data can be queried and analyzed using SQL

 - There are different dialects of SQL.
 different RDBMS (Relational Database Management System) systems use different 'dialects' or 'flavours' of the query language. Some examples of  RDBMS are  MySQL, PorstgreSQL, Oracle DB, SQLite, etc. 

 ## Data Types
Most popular data types in SQL are: 
- Integer 
- Text 
- Date (“YYYY-MM-DD”)
- Char / Varchar
- Float 


## Syntax
SQL commands are typically written in capital letters and the Statements usually ended with a semicolon (;). below is a sample line of SQL code:

```sql
 SELECT * FROM Product;
 ```

The asterisk (*) indicates that we need all the information from the table.

We can also CREATE TABLE or INSERT some information into a table shown below:

```sql
INSERT INTO product(id, item, make, model)
VALUES(1, 'Laptop', 'Macbook' , 'Macbook_Pro');
```
## Commands
One of the objectives of writing SQL commands is to retrieve information stored in a database, hence we 'query' the database for the relevant information. 

Here are some codes: Example, If we need to figure out how many `unique` models we have of Laptops on our Product table:

We can write a code like this shown below:

```sql
SELECT DISTINCT model
FROM product;
```

The `SELECT` command essentially returns or displays the result of a query. 

Here are some popular SQL commands:

- SELECT - extracts and displays data from the database
- UPDATE - Updates data in the database
- DELETE - Deletes data from a database
- INSERT INTO - Inserts new data into the database
- CREATE DATABASE - Creates a new database
- ALTER DATABSE - Modifies the database
- CREATE TABLE - Creates a new table

## Aggregate Functions
Aggregate functions are calculations performed on multiple rows of a table. They are used to calculate basic statistics and answer questions like ‘What is the average number of Laptops?, What is average number of Laptops per location?’ , Total numbers etc. Some of the aggregate functions are:

- COUNT() — calculates the number of non-empty rows
- AVG() — calculates the mean of a numeric column
- MAX()/MIN() — returns largest or smallest value in a column
- SUM() — sums all the numeric values in a column
- ROUND() — rounds values in a column. You will need to specify to how many decimal points you want the value rounded

## SQL Clauses
These are built in functions made available for use by SQL. Clauses can help us filter and analyze data quickly. Typically when we have large amounts of data stored in databases, we can use clauses to query and get the required data efficiently.  

Common SQL clauses are:

* WHERE Clause
* ORDER BY clause
* HAVING Clause
* TOP Clause
* GROUP BY Clause

### WHERE Clause
The WHERE clause is used to restrict the number of records to be retrieved by the table. We could also use logical or comparison operators such as LIKE, LESS THAN (<), GREATER THAN (>), EQUAL (=), etc. with WHERE clause to fulfill certain conditions

```sql
SELECT item, make, model From Products WHERE id = 1;
```

### ORDER BY Clause
The ORDER BY clause is for sorting records. It is used to arrange the result set either in ascending (ASC) or descending (DESC) order. When we query using SELECT statement the result is not in an ordered form. Hence, the result rows can be sorted when we combine the SELECT statement with the ORDER BY clause.

```sql
SELECT item, make, model From Products  ORDER BY model ASC;
```
### GROUP BY Clause

The GROUP BY clause is used to group rows that have the same values in the result set. Like "find the number of customers in each country".


```sql
SELECT COUNT(CustomerID), Country FROM Customers
GROUP BY Country
ORDER BY COUNT(CustomerID) DESC;
```

```sql
SELECT ColumnName FROM Table WHERE condition GROUP BY ColumnName [ORDER BY ColumnName];
```

The GROUP BY statement is often used with aggregate functions like (COUNT(), MAX(), MIN(), SUM(), AVG()) to group the result-set by one or more columns.

### HAVING Clause

Actually, this clause was introduced to apply functions in the query with the WHERE clause. In SQL, the HAVING clause was added because the WHERE clause could not be applied with aggregate functions.

```sql
SELECT ColumnName FROM TableName WHERE condition GROUP BY ColumnName HAVING condition [ORDER BY ColumnName];
```
```sql
SELECT COUNT (bookId), LanguageId From Books GROUP BY LanguageId HAVING COUNT(bookId) >3
```

### TOP Clause (LIMIT)

The TOP clause is used to determine the number of rows to be shown in the result. This TOP clause is used with SELECT statement specially implemented on large tables with many records. MySQL supports LIMIT instead of TOP

```sql
SELECT TOP no ColumName(s) FROM TableName WHERE condition;
```

## Joins in SQL

Typically different related information are stored in different tables. To retrieve a joined up informaton on these tables, we will need to use `SQL JOINS` 

Lets say we have two tables called `Products` and `Product_Detail`. To be able to retrieve joined up values from both tables, we can implement SQL join using a common columnId to both tables.

Lets assume the common columnId on both tables is `Product_Id`.

Let's also assume that we need to retrieve `name` and `model` from the `Product table` and `price` and `stockLevel` from the `Product_detail table ` We can write a SQL code similar to the one shown below to achieve our objective:

```sql
SELECT Product.name, Product.model, Product_detail.price, Product_detail.stockLevel 
FROM Products
INNER JOIN Product_detail
ON Products.Product_Id = Product_detail.Product_Id
```

_Some examples of SQL Joins_


![image](https://user-images.githubusercontent.com/114578618/203140160-1e38df96-28b9-4818-9b33-8f089fb985e3.png)

- Inner Join - Returns rows that matches on both tables
- Left Join - Returns all rows from the Left table and rows that match from the right table
- Right Join - Returns all rows from the right table and rows that match from the left table
- Outer Join (Full Join) - All rows from either the left or right table that does not meet the join condition are included in the result set. Could potentially return vey large result set

## SQL Aliases

Alias allows you reduce the amount of code required for a query and makes your queries simpler to read and understand

Aliases are temporary names you give to a table or a column in a table and they only exists for the duration of that query.

Now let's try to shorten our SQL Join above using Aliases

 ```sql
SELECT P.name, P.model, Pd.price, Pd.stockLevel 
FROM Products AS P
INNER JOIN Product_detail AS Pd
ON P.Product_Id = Pd.Product_Id
 ```
