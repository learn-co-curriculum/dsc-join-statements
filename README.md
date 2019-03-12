
# Join Statements

## Introduction

In this section, you will learn about several types of join statements.  Joins are the primary mechanism for combining data from multiple tables. In order to due this, you define the common attribute(s) between tables in order for them to be combined.

## Objectives  

You will be able to:  
 
* Compare and contrast the various types of joins
* Discuss the syntax and structure of join statements
* Discuss the role of foreign and primary keys in them

## CRM Schema

In almost all industry cases, rather then just working with a single table you will generally need data from multiple tables. Doing this requires the use of **joins** using shared columns from the two tables. For example, here's a diagram of a mock customer relation management database.
<img src='Database-Schema.png' width=550>

## Connecting to the Database

As usual, you'll start by connecting to the database.


```python
import sqlite3
import pandas as pd
```


```python
conn = sqlite3.connect('data.sqlite', detect_types=sqlite3.PARSE_COLNAMES)
cur = conn.cursor()
```

## Displaying product details along with order details
Let's say you need to generate some report that includes details about products from orders. To do that, we would need to take data from multiple tables in a single statement.


```python
cur.execute("""select * from orderdetails
                        join products
                        on orderdetails.productCode = products.productCode
                        limit 10;
                       """)
df = pd.DataFrame(cur.fetchall()) #Take results and create dataframe
df.columns = [i[0] for i in cur.description]
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>orderNumber</th>
      <th>productCode</th>
      <th>quantityOrdered</th>
      <th>priceEach</th>
      <th>orderLineNumber</th>
      <th>productCode</th>
      <th>productName</th>
      <th>productLine</th>
      <th>productScale</th>
      <th>productVendor</th>
      <th>productDescription</th>
      <th>quantityInStock</th>
      <th>buyPrice</th>
      <th>MSRP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10100</td>
      <td>S18_1749</td>
      <td>30</td>
      <td>136.00</td>
      <td>3</td>
      <td>S18_1749</td>
      <td>1917 Grand Touring Sedan</td>
      <td>Vintage Cars</td>
      <td>1:18</td>
      <td>Welly Diecast Productions</td>
      <td>This 1:18 scale replica of the 1917 Grand Tour...</td>
      <td>2724</td>
      <td>86.70</td>
      <td>170.00</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10100</td>
      <td>S18_2248</td>
      <td>50</td>
      <td>55.09</td>
      <td>2</td>
      <td>S18_2248</td>
      <td>1911 Ford Town Car</td>
      <td>Vintage Cars</td>
      <td>1:18</td>
      <td>Motor City Art Classics</td>
      <td>Features opening hood, opening doors, opening ...</td>
      <td>540</td>
      <td>33.30</td>
      <td>60.54</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10100</td>
      <td>S18_4409</td>
      <td>22</td>
      <td>75.46</td>
      <td>4</td>
      <td>S18_4409</td>
      <td>1932 Alfa Romeo 8C2300 Spider Sport</td>
      <td>Vintage Cars</td>
      <td>1:18</td>
      <td>Exoto Designs</td>
      <td>This 1:18 scale precision die cast replica fea...</td>
      <td>6553</td>
      <td>43.26</td>
      <td>92.03</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10100</td>
      <td>S24_3969</td>
      <td>49</td>
      <td>35.29</td>
      <td>1</td>
      <td>S24_3969</td>
      <td>1936 Mercedes Benz 500k Roadster</td>
      <td>Vintage Cars</td>
      <td>1:24</td>
      <td>Red Start Diecast</td>
      <td>This model features grille-mounted chrome horn...</td>
      <td>2081</td>
      <td>21.75</td>
      <td>41.03</td>
    </tr>
    <tr>
      <th>4</th>
      <td>10101</td>
      <td>S18_2325</td>
      <td>25</td>
      <td>108.06</td>
      <td>4</td>
      <td>S18_2325</td>
      <td>1932 Model A Ford J-Coupe</td>
      <td>Vintage Cars</td>
      <td>1:18</td>
      <td>Autoart Studio Design</td>
      <td>This model features grille-mounted chrome horn...</td>
      <td>9354</td>
      <td>58.48</td>
      <td>127.13</td>
    </tr>
  </tbody>
</table>
</div>



## Compared to the individual tables:

### orderdetails


```python
cur.execute("""select * from orderdetails limit 10;""")
df = pd.DataFrame(cur.fetchall()) #Take results and create dataframe
df.columns = [i[0] for i in cur.description]
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>orderNumber</th>
      <th>productCode</th>
      <th>quantityOrdered</th>
      <th>priceEach</th>
      <th>orderLineNumber</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10100</td>
      <td>S18_1749</td>
      <td>30</td>
      <td>136.00</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10100</td>
      <td>S18_2248</td>
      <td>50</td>
      <td>55.09</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10100</td>
      <td>S18_4409</td>
      <td>22</td>
      <td>75.46</td>
      <td>4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10100</td>
      <td>S24_3969</td>
      <td>49</td>
      <td>35.29</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>10101</td>
      <td>S18_2325</td>
      <td>25</td>
      <td>108.06</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>



### products


```python
cur.execute("""select * from products limit 10;""")
df = pd.DataFrame(cur.fetchall()) #Take results and create dataframe
df.columns = [i[0] for i in cur.description]
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>productCode</th>
      <th>productName</th>
      <th>productLine</th>
      <th>productScale</th>
      <th>productVendor</th>
      <th>productDescription</th>
      <th>quantityInStock</th>
      <th>buyPrice</th>
      <th>MSRP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>S10_1678</td>
      <td>1969 Harley Davidson Ultimate Chopper</td>
      <td>Motorcycles</td>
      <td>1:10</td>
      <td>Min Lin Diecast</td>
      <td>This replica features working kickstand, front...</td>
      <td>7933</td>
      <td>48.81</td>
      <td>95.70</td>
    </tr>
    <tr>
      <th>1</th>
      <td>S10_1949</td>
      <td>1952 Alpine Renault 1300</td>
      <td>Classic Cars</td>
      <td>1:10</td>
      <td>Classic Metal Creations</td>
      <td>Turnable front wheels; steering function; deta...</td>
      <td>7305</td>
      <td>98.58</td>
      <td>214.30</td>
    </tr>
    <tr>
      <th>2</th>
      <td>S10_2016</td>
      <td>1996 Moto Guzzi 1100i</td>
      <td>Motorcycles</td>
      <td>1:10</td>
      <td>Highway 66 Mini Classics</td>
      <td>Official Moto Guzzi logos and insignias, saddl...</td>
      <td>6625</td>
      <td>68.99</td>
      <td>118.94</td>
    </tr>
    <tr>
      <th>3</th>
      <td>S10_4698</td>
      <td>2003 Harley-Davidson Eagle Drag Bike</td>
      <td>Motorcycles</td>
      <td>1:10</td>
      <td>Red Start Diecast</td>
      <td>Model features, official Harley Davidson logos...</td>
      <td>5582</td>
      <td>91.02</td>
      <td>193.66</td>
    </tr>
    <tr>
      <th>4</th>
      <td>S10_4757</td>
      <td>1972 Alfa Romeo GTA</td>
      <td>Classic Cars</td>
      <td>1:10</td>
      <td>Motor City Art Classics</td>
      <td>Features include: Turnable front wheels; steer...</td>
      <td>3252</td>
      <td>85.68</td>
      <td>136.00</td>
    </tr>
  </tbody>
</table>
</div>



## the using clause
A more concise way to join the tables if the column name is identical is the using clause. Rather then saying on `tableA.column = tableB.column` we can simply say `using(column)`. Again, this only works if the column is identically named for both tables.


```python
cur.execute("""select * from orderdetails
                        join products
                        using(productCode)
                        limit 10;
                       """)
df = pd.DataFrame(cur.fetchall()) #Take results and create dataframe
df.columns = [i[0] for i in cur.description]
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>orderNumber</th>
      <th>productCode</th>
      <th>quantityOrdered</th>
      <th>priceEach</th>
      <th>orderLineNumber</th>
      <th>productName</th>
      <th>productLine</th>
      <th>productScale</th>
      <th>productVendor</th>
      <th>productDescription</th>
      <th>quantityInStock</th>
      <th>buyPrice</th>
      <th>MSRP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10100</td>
      <td>S18_1749</td>
      <td>30</td>
      <td>136.00</td>
      <td>3</td>
      <td>1917 Grand Touring Sedan</td>
      <td>Vintage Cars</td>
      <td>1:18</td>
      <td>Welly Diecast Productions</td>
      <td>This 1:18 scale replica of the 1917 Grand Tour...</td>
      <td>2724</td>
      <td>86.70</td>
      <td>170.00</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10100</td>
      <td>S18_2248</td>
      <td>50</td>
      <td>55.09</td>
      <td>2</td>
      <td>1911 Ford Town Car</td>
      <td>Vintage Cars</td>
      <td>1:18</td>
      <td>Motor City Art Classics</td>
      <td>Features opening hood, opening doors, opening ...</td>
      <td>540</td>
      <td>33.30</td>
      <td>60.54</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10100</td>
      <td>S18_4409</td>
      <td>22</td>
      <td>75.46</td>
      <td>4</td>
      <td>1932 Alfa Romeo 8C2300 Spider Sport</td>
      <td>Vintage Cars</td>
      <td>1:18</td>
      <td>Exoto Designs</td>
      <td>This 1:18 scale precision die cast replica fea...</td>
      <td>6553</td>
      <td>43.26</td>
      <td>92.03</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10100</td>
      <td>S24_3969</td>
      <td>49</td>
      <td>35.29</td>
      <td>1</td>
      <td>1936 Mercedes Benz 500k Roadster</td>
      <td>Vintage Cars</td>
      <td>1:24</td>
      <td>Red Start Diecast</td>
      <td>This model features grille-mounted chrome horn...</td>
      <td>2081</td>
      <td>21.75</td>
      <td>41.03</td>
    </tr>
    <tr>
      <th>4</th>
      <td>10101</td>
      <td>S18_2325</td>
      <td>25</td>
      <td>108.06</td>
      <td>4</td>
      <td>1932 Model A Ford J-Coupe</td>
      <td>Vintage Cars</td>
      <td>1:18</td>
      <td>Autoart Studio Design</td>
      <td>This model features grille-mounted chrome horn...</td>
      <td>9354</td>
      <td>58.48</td>
      <td>127.13</td>
    </tr>
  </tbody>
</table>
</div>



## Aliasing
Alternatively, you can also alias tables by giving them an alternative shorthand name directly after them. Here we use the aliases 'o' and 'p' for orderdetails and products respectively.


```python
cur.execute("""select * from orderdetails o
                        join products p
                        on o.productCode = p.productCode
                        limit 10;
                       """)
df = pd.DataFrame(cur.fetchall()) #Take results and create dataframe
df.columns = [i[0] for i in cur.description]
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>orderNumber</th>
      <th>productCode</th>
      <th>quantityOrdered</th>
      <th>priceEach</th>
      <th>orderLineNumber</th>
      <th>productCode</th>
      <th>productName</th>
      <th>productLine</th>
      <th>productScale</th>
      <th>productVendor</th>
      <th>productDescription</th>
      <th>quantityInStock</th>
      <th>buyPrice</th>
      <th>MSRP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10100</td>
      <td>S18_1749</td>
      <td>30</td>
      <td>136.00</td>
      <td>3</td>
      <td>S18_1749</td>
      <td>1917 Grand Touring Sedan</td>
      <td>Vintage Cars</td>
      <td>1:18</td>
      <td>Welly Diecast Productions</td>
      <td>This 1:18 scale replica of the 1917 Grand Tour...</td>
      <td>2724</td>
      <td>86.70</td>
      <td>170.00</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10100</td>
      <td>S18_2248</td>
      <td>50</td>
      <td>55.09</td>
      <td>2</td>
      <td>S18_2248</td>
      <td>1911 Ford Town Car</td>
      <td>Vintage Cars</td>
      <td>1:18</td>
      <td>Motor City Art Classics</td>
      <td>Features opening hood, opening doors, opening ...</td>
      <td>540</td>
      <td>33.30</td>
      <td>60.54</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10100</td>
      <td>S18_4409</td>
      <td>22</td>
      <td>75.46</td>
      <td>4</td>
      <td>S18_4409</td>
      <td>1932 Alfa Romeo 8C2300 Spider Sport</td>
      <td>Vintage Cars</td>
      <td>1:18</td>
      <td>Exoto Designs</td>
      <td>This 1:18 scale precision die cast replica fea...</td>
      <td>6553</td>
      <td>43.26</td>
      <td>92.03</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10100</td>
      <td>S24_3969</td>
      <td>49</td>
      <td>35.29</td>
      <td>1</td>
      <td>S24_3969</td>
      <td>1936 Mercedes Benz 500k Roadster</td>
      <td>Vintage Cars</td>
      <td>1:24</td>
      <td>Red Start Diecast</td>
      <td>This model features grille-mounted chrome horn...</td>
      <td>2081</td>
      <td>21.75</td>
      <td>41.03</td>
    </tr>
    <tr>
      <th>4</th>
      <td>10101</td>
      <td>S18_2325</td>
      <td>25</td>
      <td>108.06</td>
      <td>4</td>
      <td>S18_2325</td>
      <td>1932 Model A Ford J-Coupe</td>
      <td>Vintage Cars</td>
      <td>1:18</td>
      <td>Autoart Studio Design</td>
      <td>This model features grille-mounted chrome horn...</td>
      <td>9354</td>
      <td>58.48</td>
      <td>127.13</td>
    </tr>
  </tbody>
</table>
</div>



## Left Joins

By default, a join is an inner join, or the intersection between two tables. In other words, the join between orders and products is only for productCodes that are in both the orderdetails and products tables. If a product had yet to be ordered (and wasn't in the orderdetails table) then it would also not be in the result of the join.

There are many other types of joins, displayed below. Of these, sqlite does not support outer joins, but it is good to be aware of as more powerful versions of sql such as PostgreSQL support these additional functions.

<img src='venn.png' width=650>

For example, the statement  
  
`select * from products left join orderdetails; `  

would return all products, even those that hadn't been ordered. 
You can imagine that all products in inventory should have a description in the product table, but perhaps not every product is represented in the orderdetails table. 


```python
cur.execute("""select * from products
                        left join orderdetails
                        using(productCode);
                       """)
df = pd.DataFrame(cur.fetchall()) #Take results and create dataframe
df.columns = [i[0] for i in cur.description]
print(len(df))
print(len(df[df.orderNumber.isnull()]))
df[df.orderNumber.isnull()].head()
```

    2997
    1





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>productCode</th>
      <th>productName</th>
      <th>productLine</th>
      <th>productScale</th>
      <th>productVendor</th>
      <th>productDescription</th>
      <th>quantityInStock</th>
      <th>buyPrice</th>
      <th>MSRP</th>
      <th>orderNumber</th>
      <th>quantityOrdered</th>
      <th>priceEach</th>
      <th>orderLineNumber</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1122</th>
      <td>S18_3233</td>
      <td>1985 Toyota Supra</td>
      <td>Classic Cars</td>
      <td>1:18</td>
      <td>Highway 66 Mini Classics</td>
      <td>This model features soft rubber tires, working...</td>
      <td>7733</td>
      <td>57.01</td>
      <td>107.57</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
    </tr>
  </tbody>
</table>
</div>



As you can see, its rare, but there is one product that has yet to be ordered

## Primary Versus Foreign Keys

Another important consideration when performing joins is to think more about the key or column you are joining on. As you'll see in upcoming lessons, this can lead to interesting behavior if the join value is not unique in one or both of the tables. In all of the above examples, you joined two tables using the **primary key**. The primary key(s) of a table are those column(s) which uniquely identify a row. You'll also see this designated in our schema diagram with the asterisk (*).
<img src='Database-Schema.png' width=550>

You can also join tables using **foreign keys** which are not the primary key for that particular table, but rather another table. For example, employeeNumber is the primary key for the employees table and corresponds to the salesRepEmployeeNumber of the customers table. In the customers table, salesRepEmployeeNumber is only a foreign key, and is unlikely to be a unique identifier, as it is likely that an employee serves multiple customers. As such, in the resulting view, employeeNumber would no longer be a unique field.


```python
cur.execute("""select * from customers c
                        join employees e
                        on c.salesRepEmployeeNumber = e.employeeNumber
                        order by employeeNumber;
                       """)
df = pd.DataFrame(cur.fetchall()) #Take results and create dataframe
df.columns = [i[0] for i in cur.description]
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>customerNumber</th>
      <th>customerName</th>
      <th>contactLastName</th>
      <th>contactFirstName</th>
      <th>phone</th>
      <th>addressLine1</th>
      <th>addressLine2</th>
      <th>city</th>
      <th>state</th>
      <th>postalCode</th>
      <th>...</th>
      <th>salesRepEmployeeNumber</th>
      <th>creditLimit</th>
      <th>employeeNumber</th>
      <th>lastName</th>
      <th>firstName</th>
      <th>extension</th>
      <th>email</th>
      <th>officeCode</th>
      <th>reportsTo</th>
      <th>jobTitle</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>124</td>
      <td>Mini Gifts Distributors Ltd.</td>
      <td>Nelson</td>
      <td>Susan</td>
      <td>4155551450</td>
      <td>5677 Strong St.</td>
      <td></td>
      <td>San Rafael</td>
      <td>CA</td>
      <td>97562</td>
      <td>...</td>
      <td>1165</td>
      <td>210500.00</td>
      <td>1165</td>
      <td>Jennings</td>
      <td>Leslie</td>
      <td>x3291</td>
      <td>ljennings@classicmodelcars.com</td>
      <td>1</td>
      <td>1143</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>1</th>
      <td>129</td>
      <td>Mini Wheels Co.</td>
      <td>Murphy</td>
      <td>Julie</td>
      <td>6505555787</td>
      <td>5557 North Pendale Street</td>
      <td></td>
      <td>San Francisco</td>
      <td>CA</td>
      <td>94217</td>
      <td>...</td>
      <td>1165</td>
      <td>64600.00</td>
      <td>1165</td>
      <td>Jennings</td>
      <td>Leslie</td>
      <td>x3291</td>
      <td>ljennings@classicmodelcars.com</td>
      <td>1</td>
      <td>1143</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>2</th>
      <td>161</td>
      <td>Technics Stores Inc.</td>
      <td>Hashimoto</td>
      <td>Juri</td>
      <td>6505556809</td>
      <td>9408 Furth Circle</td>
      <td></td>
      <td>Burlingame</td>
      <td>CA</td>
      <td>94217</td>
      <td>...</td>
      <td>1165</td>
      <td>84600.00</td>
      <td>1165</td>
      <td>Jennings</td>
      <td>Leslie</td>
      <td>x3291</td>
      <td>ljennings@classicmodelcars.com</td>
      <td>1</td>
      <td>1143</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>3</th>
      <td>321</td>
      <td>Corporate Gift Ideas Co.</td>
      <td>Brown</td>
      <td>Julie</td>
      <td>6505551386</td>
      <td>7734 Strong St.</td>
      <td></td>
      <td>San Francisco</td>
      <td>CA</td>
      <td>94217</td>
      <td>...</td>
      <td>1165</td>
      <td>105000.00</td>
      <td>1165</td>
      <td>Jennings</td>
      <td>Leslie</td>
      <td>x3291</td>
      <td>ljennings@classicmodelcars.com</td>
      <td>1</td>
      <td>1143</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>4</th>
      <td>450</td>
      <td>The Sharp Gifts Warehouse</td>
      <td>Frick</td>
      <td>Sue</td>
      <td>4085553659</td>
      <td>3086 Ingle Ln.</td>
      <td></td>
      <td>San Jose</td>
      <td>CA</td>
      <td>94217</td>
      <td>...</td>
      <td>1165</td>
      <td>77600.00</td>
      <td>1165</td>
      <td>Jennings</td>
      <td>Leslie</td>
      <td>x3291</td>
      <td>ljennings@classicmodelcars.com</td>
      <td>1</td>
      <td>1143</td>
      <td>Sales Rep</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 21 columns</p>
</div>



Notice that this also returned both columns: salesRepEmployeeNumber and employeeNumber.

## Summary

In this lesson you investigated joins including the on and using clause, aliasing table names, left joins and primary and foreign keys.
