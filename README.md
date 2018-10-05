
# Join Statements

## Introduction

In this section, you will learn about several types of Join statements.

## Objectives

You will be able to:

- Compare and contrast the various types of joins
- Understand the structure of Join statements, and the role of foreign and primary keys in them

## CRM Schema

In almost all cases, rather then just working with a single table we will typically need data from multiple tables. Doing this requires the use of **joins ** using shared columns from the two tables. For example, here's a diagram of a mock Customer Relationship Management (CRM) database.
<img src='Database-Schema.png' width=550>

## Connecting to the Database


```python
import sqlite3
import pandas as pd
```


```python
conn = sqlite3.connect('data.sqlite', detect_types=sqlite3.PARSE_COLNAMES)
cur = conn.cursor()
```

## Displaying product details along with order details
Let's say we need to generate some report that includes details about products from orders. To do that, we would need to take data from multiple tables in a single statement.


```python
cur.execute("""select * from orderdetails
                        join products
                        on orderdetails.productCode = products.productCode
                        limit 10;
                       """)
df = pd.DataFrame(cur.fetchall()) #Take results and create dataframe
df. columns = [i[0] for i in cur.description]
df.head()
```

# Compared to the individual tables:

### orderdetails


```python
cur.execute("""select * from orderdetails limit 10;""")
df = pd.DataFrame(cur.fetchall()) #Take results and create dataframe
df. columns = [i[0] for i in cur.description]
df.head()
```

### products


```python
cur.execute("""select * from products limit 10;""")
df = pd.DataFrame(cur.fetchall()) #Take results and create dataframe
df. columns = [i[0] for i in cur.description]
df.head()
```

# the using clause
A more concise way to join the tables if the column name is identical is the using clauase.


```python
cur.execute("""select * from orderdetails
                        join products
                        using(productCode)
                        limit 10;
                       """)
df = pd.DataFrame(cur.fetchall()) #Take results and create dataframe
df. columns = [i[0] for i in cur.description]
df.head()
```

# Aliasing
Alternatively, you can also alias tables by giving them an alternative shorthand name directly after them. Here we use the aliases 'o' and 'p' for orderdetails and products respectively.


```python
cur.execute("""select * from orderdetails o
                        join products p
                        on o.productCode = p.productCode
                        limit 10;
                       """)
df = pd.DataFrame(cur.fetchall()) #Take results and create dataframe
df. columns = [i[0] for i in cur.description]
df.head()
```

# Left Joins

Above, we have only been doing **inner joins** which is the intersection of the two tables. There are many other types of joins, displayed below. Of these, sqlite does not support outer joins, but it is good to be aware of as more powerful versions of sql such as postgresql support these additional functions.

<img src='venn.png' width=650>

For example, the statement  
  
`select * from products left join orderdetails; `  

would return all products, even those that hadn't been ordered. 
We can imagine that all products in inventory should have a description in the product table, but perhaps not every product is represented in the orderdetails table. 


```python
cur.execute("""select * from products
                        left join orderdetails
                        using(productCode);
                       """)
df = pd.DataFrame(cur.fetchall()) #Take results and create dataframe
df. columns = [i[0] for i in cur.description]
print(len(df))
print(len(df[df.orderNumber.isnull()]))
df[df.orderNumber.isnull()].head()
```

As you can see, its rare, but there is one product that has yet to be ordered

# Summary

Great, you now know about join statements, how to use them and when to use which type of join statement! Let's move over to some practice!
