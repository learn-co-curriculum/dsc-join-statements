# Join Statements

## Introduction

In this section, you will learn about several types of `JOIN` statements.  Joins are the primary mechanism for combining data from multiple tables. In order to do this, you define the common attribute(s) between tables in order for them to be combined.

## Objectives  

You will be able to:  
 
* Write SQL queries that make use of various types of joins
* Compare and contrast the various types of joins
* Discuss how primary and foreign keys are used in SQL
* Decide and perform whichever type of join is best for retrieving desired data


## CRM ERD

In almost all industry cases, rather than just working with a single table you will generally need data from multiple tables. Doing this requires the use of **joins** using shared columns from the two tables. For example, here's a diagram of a mock customer relationship management (CRM) database.
<img src='https://curriculum-content.s3.amazonaws.com/data-science/images/Database-Schema.png' width=550>

## Connecting to the Database

As usual, you'll start by connecting to the database.

> In the cell below, type the code to import `sqlite` and `pandas` with the standard alias. Then in the next cell create a connection to the database `data.sqlite` and asign it to a variable:

<details>
    <summary style="cursor: pointer; display: inline">
        <b><u>CLICK to Reveal Code</u></b>
    </summary>
    <pre><code language="python">import sqlite3
import pandas as pd

conn = sqlite3.connect('data.sqlite')
    </code></pre>
</details>


```python
# replace this comment with the code to import the libraries
```


```python
# replace this comment with the code to create a connection to the database data.database
```


```python
import sqlite3
import pandas as pd
conn = sqlite3.connect('data.sqlite')
```

## Displaying Product Details Along with Order Details

Let's say you need to generate a report that includes details about products from orders. To do that, we would need to take data from multiple tables in a single statement. To do this we will use `JOIN`.

> In the cell below, type the query to select all records from `orderdetails` and `products` and join them using thier common key `productCode` and display the first 10.

<details>
    <summary style="cursor: pointer; display: inline">
        <b><u>CLICK to Reveal Code</u></b>
    </summary>
    <pre><code language="python">q = """
SELECT *
  FROM orderdetails
       JOIN products
       ON orderdetails.productCode = products.productCode
       LIMIT 10;
"""
pd.read_sql(q, conn)
    </code></pre>
</details>


```python
# replace None with the query to join orderdetails and proucts on productCode
query = None
pd.read_sql(query, conn)
```

---
#### Expected Output
<pre><code>a DataFrame with 10 rows and 14 columns
</code></pre>
<details>
    <summary style="cursor: pointer; display: inline">
        <b><u>Click to Expand Complete Output</u></b>
    </summary>
    <img src="https://curriculum-content.s3.amazonaws.com/data-science/images/join_1.png">
</details>

---

## Compared to the Individual Tables:

### `orderdetails` Table:

> In the cell below, type the code to select all records from `orderdetails` and display the first `10`

<details>
    <summary style="cursor: pointer; display: inline">
        <b><u>CLICK to Reveal Code</u></b>
    </summary>
    <pre><code class="language-python">query = """
SELECT *
  FROM orderdetails LIMIT 10;
"""
pd.read_sql(query, conn)
    </code></pre>
</details>


```python
# replace None with the query to display the first 10 records in orderdetails
query = None
pd.read_sql(query, conn)
```

---
#### Expected Output
<pre><code>the first 10 records in orderdetails
</code></pre>
<details>
    <summary style="cursor: pointer; display: inline">
        <b><u>Click to Expand Complete Output</u></b>
    </summary>
    <img src="https://curriculum-content.s3.amazonaws.com/data-science/images/orderdetails.png">
</details>

---

### `products` Table:

> In the cell below, type the code to select all records from `products` and display the first `10`

<details>
    <summary style="cursor: pointer; display: inline">
        <b><u>CLICK to Reveal Code</u></b>
    </summary>
    <pre><code class="language-python">query = """
SELECT *
  FROM products LIMIT 10;
"""
pd.read_sql(query, conn)
    </code></pre>
</details>


```python
# replace None with the query to display the first 10 records in products
query = None
pd.read_sql(query, conn)
```

---
#### Expected Output
<pre><code>the first 10 records in products
</code></pre>
<details>
    <summary style="cursor: pointer; display: inline">
        <b><u>Click to Expand Complete Output</u></b>
    </summary>
    <img src="https://curriculum-content.s3.amazonaws.com/data-science/images/products.png">
</details>

---

## The `USING` clause
A more concise way to join the tables, if the column name is identical, is the `USING` clause. Rather then saying `ON tableA.column = tableB.column` we can simply say `USING(column)`. Again, this only works if the column is **identically named** for both tables.

> In the cell below, type the query to select all records in `orderdetails` and `products` and join them on `productCode` with the `USING()` clause, and return the first 10 records:

<details>
    <summary style="cursor: pointer; display: inline">
        <b><u>CLICK to Reveal Code</u></b>
    </summary>
    <pre><code language="python">query = """
SELECT *
  FROM orderdetails
       JOIN products
       USING(productCode)
       LIMIT 10;
"""
pd.read_sql(query, conn)
    </code></pre>
</details>



```python
# replace None with the query to join orderdetails and proucts on productCode with the using() clause
query = None
pd.read_sql(query, conn)
```

---
#### Expected Output
<pre><code>a DataFrame with 10 rows and 14 columns
</code></pre>
<details>
    <summary style="cursor: pointer; display: inline">
        <b><u>Click to Expand Complete Output</u></b>
    </summary>
    <img src="https://curriculum-content.s3.amazonaws.com/data-science/images/join_1.png">
</details>

---

## More Aliasing 

You can also assign tables an **alias** by entering an alternative shorthand name. This is slightly different than the previous lesson where we introduced aliases for column names, since now we are aliasing *tables*.

When aliasing columns the goal is usually to improve readability by giving something a more specific or easier-to-read name. For example, `name AS employee_name`, `AVG(AVG) AS average_batting_average`, or `COUNT(*) AS num_products`.

When aliasing tables the goal is usually to shorten the name, in order to shorten the overall query. So typically you'll see examples that alias a longer table name to a one-character or two-character shorthand. For example, `orderdetails AS od` or `products AS p`. (It is also possible to use aliases to clarify what exactly is in a table, like how aliases are used for columns, just less common.)

The following query produces the same result as the previous ones, using aliases `od` and `p` for `orderdetails` and `products`, respectively:

> In the following cell, type the following code to demonstrate the use of aliasing:

<details>
    <summary style="cursor: pointer; display: inline">
        <b><u>CLICK to Reveal Code</u></b>
    </summary>
    <pre><code language="python">query = """
SELECT *
  FROM orderdetails AS od
       JOIN products AS p
       ON od.productCode = p.productCode
       LIMIT 10;
"""
    </code></pre>
</details>


```python
# replace None with the query to demonstrate aliasing
query = None
pd.read_sql(query, conn)
```

---
#### Expected Output
<pre><code>a DataFrame with 10 rows and 14 columns
</code></pre>
<details>
    <summary style="cursor: pointer; display: inline">
        <b><u>Click to Expand Complete Output</u></b>
    </summary>
    <img src="https://curriculum-content.s3.amazonaws.com/data-science/images/join_1.png">
</details>

---

Note that just like with column aliases, the `AS` keyword is optional in SQLite. So, instead of `FROM orderdetails AS od` you could write `FROM orderdetails od` with the same outcome.

It is somewhat more common to see `AS` used with column aliases and skipped with table aliases, but again, you'll want to check the syntax rules of your particular type of SQL as well as style guidelines from your employer to know which syntax to use in a professional setting.

## `LEFT JOIN`s

By default a `JOIN` is an `INNER JOIN`, or the intersection between two tables. In other words, the `JOIN` between orders and products is only for `productCodes` that are in both the `orderdetails` and `products` tables. If a product had yet to be ordered (and wasn't in the `orderdetails` table) then it would also not be in the result of the `JOIN`.

The `LEFT JOIN` keyword returns all records from the left table (table1), and the matched records from the right table (table2). The result is NULL from the right side if there is no match.

There are many other types of joins, displayed below. Of these, SQLite does not support outer joins, but it is good to be aware of as more powerful versions of SQL such as PostgreSQL support these additional functions.

<img src='https://curriculum-content.s3.amazonaws.com/data-science/images/venn.png' width="700">

For example, the statement  
  
`SELECT * FROM products LEFT JOIN orderdetails `  

would return all products, even those that hadn't been ordered. 
You can imagine that all products in inventory should have a description in the product table, but perhaps not every product is represented in the orderdetails table. 

> In the cell below, type the query to select all records from `products` and join them with all records in `orderdetails` on `productcode` using `LEFT JOIN`, then execute the query and store it in a dataframe named `df`:

<details>
    <summary style="cursor: pointer; display: inline">
        <b><u>CLICK to Reveal Code</u></b>
    </summary>
    <pre><code language="python">query = """
SELECT *
  FROM products
       LEFT JOIN orderdetails
       USING(productCode);
"""

df = pd.read_sql(query, conn)
    </code></pre>
</details>



```python
# replace this comment with the code to create the specified query

# replace this comment with the code to execute the query and store it in a dataframe named df

print("Number of records returned:", len(df))
print("Number of records where order details are null:", len(df[df.orderNumber.isnull()]))

```

---
#### Expected Output
<pre><code>Number of records returned: 2997
Number of records where order details are null: 1
</code></pre>

---

Let's take a look at the one record that has null values in the order details:


```python
# run this cell with no changes to view the one record with null values
df[df.orderNumber.isnull()]
```

---
#### Expected Output
<pre><code>a dataframe with one row and 14 columns
</code></pre>
<details>
    <summary style="cursor: pointer; display: inline">
        <b><u>Click to Expand Complete Output</u></b>
    </summary>
    <img src="https://curriculum-content.s3.amazonaws.com/data-science/images/left_join.png">
</details>

---

As you can see, it's a rare occurrence, but there is one product that has yet to be ordered.

## Primary Versus Foreign Keys

Another important consideration when performing joins is to think more about the key or column you are joining on. As you'll see in upcoming lessons, this can lead to interesting behavior if the join value is not unique in one or both of the tables. In all of the above examples, you joined two tables using the **primary key**. The primary key(s) of a table are those column(s) which uniquely identify a row. You'll also see this designated in our schema diagram with the asterisk (*).
<img src='https://curriculum-content.s3.amazonaws.com/data-science/images/Database-Schema.png' width=550>

You can also join tables using **foreign keys** which are not the primary key for that particular table, but rather another table. For example, `employeeNumber` is the primary key for the employees table and corresponds to the `salesRepEmployeeNumber` of the customers table. In the customers table, `salesRepEmployeeNumber` is only a foreign key, and is unlikely to be a unique identifier, as it is likely that an employee serves multiple customers. As such, in the resulting view `employeeNumber` would no longer be a unique field.

> In the cell below, type the query to join `customers` using the alias `c` with `employees` using the alias `e` on the foreign keys `salesTepEmoloyeeNumber` and `employeeNumber` and order the result by `employeeNumber`, then type the code to execute the query:

<details>
    <summary style="cursor: pointer; display: inline">
        <b><u>CLICK to Reveal Code</u></b>
    </summary>
    <pre><code language="python">query = """
SELECT *
  FROM customers AS c
       JOIN employees AS e
       ON c.salesRepEmployeeNumber = e.employeeNumber
       ORDER By employeeNumber;
"""
pd.read_sql(query, conn)
    </code></pre>
</details>


```python
# replace None with the query to select the desired records

# replace this comment with the code to execute the query
```

---
#### Expected Output
<pre><code>a section of the df DataFrame with 100 rows and 21 columns
</code></pre>
<details>
    <summary style="cursor: pointer; display: inline">
        <b><u>Click to Expand Complete Output</u></b>
    </summary>
    <img src="https://curriculum-content.s3.amazonaws.com/data-science/images/join_foreign_key.png">
</details>

---

Notice that this also returned both columns: `salesRepEmployeeNumber` and `employeeNumber`. These columns contain identical values so you would probably actually only want to select one or the other.

## Summary

In this lesson, you investigated joins. This included implementing the `ON` and `USING` clauses, aliasing table names, implementing `LEFT JOIN`, and using primary vs. foreign keys.
