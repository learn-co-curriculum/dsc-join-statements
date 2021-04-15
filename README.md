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
<img src='images/Database-Schema.png' width=550>

## Connecting to the Database

As usual, you'll start by connecting to the database.


```python
import sqlite3
import pandas as pd
```


```python
conn = sqlite3.connect('data.sqlite')
```

## Displaying Product Details Along with Order Details

Let's say you need to generate some report that includes details about products from orders. To do that, we would need to take data from multiple tables in a single statement. 


```python
q = """
SELECT *
FROM orderdetails
JOIN products
    ON orderdetails.productCode = products.productCode
LIMIT 10
;
"""
pd.read_sql(q, conn)
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
    <tr>
      <th>5</th>
      <td>10101</td>
      <td>S18_2795</td>
      <td>26</td>
      <td>167.06</td>
      <td>1</td>
      <td>S18_2795</td>
      <td>1928 Mercedes-Benz SSK</td>
      <td>Vintage Cars</td>
      <td>1:18</td>
      <td>Gearbox Collectibles</td>
      <td>This 1:18 replica features grille-mounted chro...</td>
      <td>548</td>
      <td>72.56</td>
      <td>168.75</td>
    </tr>
    <tr>
      <th>6</th>
      <td>10101</td>
      <td>S24_1937</td>
      <td>45</td>
      <td>32.53</td>
      <td>3</td>
      <td>S24_1937</td>
      <td>1939 Chevrolet Deluxe Coupe</td>
      <td>Vintage Cars</td>
      <td>1:24</td>
      <td>Motor City Art Classics</td>
      <td>This 1:24 scale die-cast replica of the 1939 C...</td>
      <td>7332</td>
      <td>22.57</td>
      <td>33.19</td>
    </tr>
    <tr>
      <th>7</th>
      <td>10101</td>
      <td>S24_2022</td>
      <td>46</td>
      <td>44.35</td>
      <td>2</td>
      <td>S24_2022</td>
      <td>1938 Cadillac V-16 Presidential Limousine</td>
      <td>Vintage Cars</td>
      <td>1:24</td>
      <td>Classic Metal Creations</td>
      <td>This 1:24 scale precision die cast replica of ...</td>
      <td>2847</td>
      <td>20.61</td>
      <td>44.80</td>
    </tr>
    <tr>
      <th>8</th>
      <td>10102</td>
      <td>S18_1342</td>
      <td>39</td>
      <td>95.55</td>
      <td>2</td>
      <td>S18_1342</td>
      <td>1937 Lincoln Berline</td>
      <td>Vintage Cars</td>
      <td>1:18</td>
      <td>Motor City Art Classics</td>
      <td>Features opening engine cover, doors, trunk, a...</td>
      <td>8693</td>
      <td>60.62</td>
      <td>102.74</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10102</td>
      <td>S18_1367</td>
      <td>41</td>
      <td>43.13</td>
      <td>1</td>
      <td>S18_1367</td>
      <td>1936 Mercedes-Benz 500K Special Roadster</td>
      <td>Vintage Cars</td>
      <td>1:18</td>
      <td>Studio M Art Models</td>
      <td>This 1:18 scale replica is constructed of heav...</td>
      <td>8635</td>
      <td>24.26</td>
      <td>53.91</td>
    </tr>
  </tbody>
</table>
</div>



## Compared to the Individual Tables:

### `orderdetails` Table:


```python
pd.read_sql("""SELECT * FROM orderdetails LIMIT 10;""", conn)
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
    <tr>
      <th>5</th>
      <td>10101</td>
      <td>S18_2795</td>
      <td>26</td>
      <td>167.06</td>
      <td>1</td>
    </tr>
    <tr>
      <th>6</th>
      <td>10101</td>
      <td>S24_1937</td>
      <td>45</td>
      <td>32.53</td>
      <td>3</td>
    </tr>
    <tr>
      <th>7</th>
      <td>10101</td>
      <td>S24_2022</td>
      <td>46</td>
      <td>44.35</td>
      <td>2</td>
    </tr>
    <tr>
      <th>8</th>
      <td>10102</td>
      <td>S18_1342</td>
      <td>39</td>
      <td>95.55</td>
      <td>2</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10102</td>
      <td>S18_1367</td>
      <td>41</td>
      <td>43.13</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



### `products` Table:


```python
pd.read_sql("""SELECT * FROM products LIMIT 10;""", conn)
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
    <tr>
      <th>5</th>
      <td>S10_4962</td>
      <td>1962 LanciaA Delta 16V</td>
      <td>Classic Cars</td>
      <td>1:10</td>
      <td>Second Gear Diecast</td>
      <td>Features include: Turnable front wheels; steer...</td>
      <td>6791</td>
      <td>103.42</td>
      <td>147.74</td>
    </tr>
    <tr>
      <th>6</th>
      <td>S12_1099</td>
      <td>1968 Ford Mustang</td>
      <td>Classic Cars</td>
      <td>1:12</td>
      <td>Autoart Studio Design</td>
      <td>Hood, doors and trunk all open to reveal highl...</td>
      <td>68</td>
      <td>95.34</td>
      <td>194.57</td>
    </tr>
    <tr>
      <th>7</th>
      <td>S12_1108</td>
      <td>2001 Ferrari Enzo</td>
      <td>Classic Cars</td>
      <td>1:12</td>
      <td>Second Gear Diecast</td>
      <td>Turnable front wheels; steering function; deta...</td>
      <td>3619</td>
      <td>95.59</td>
      <td>207.80</td>
    </tr>
    <tr>
      <th>8</th>
      <td>S12_1666</td>
      <td>1958 Setra Bus</td>
      <td>Trucks and Buses</td>
      <td>1:12</td>
      <td>Welly Diecast Productions</td>
      <td>Model features 30 windows, skylights &amp; glare r...</td>
      <td>1579</td>
      <td>77.90</td>
      <td>136.67</td>
    </tr>
    <tr>
      <th>9</th>
      <td>S12_2823</td>
      <td>2002 Suzuki XREO</td>
      <td>Motorcycles</td>
      <td>1:12</td>
      <td>Unimax Art Galleries</td>
      <td>Official logos and insignias, saddle bags loca...</td>
      <td>9997</td>
      <td>66.27</td>
      <td>150.62</td>
    </tr>
  </tbody>
</table>
</div>



## The `USING` clause
A more concise way to join the tables, if the column name is identical, is the `USING` clause. Rather then saying on `tableA.column = tableB.column` we can simply say `USING(column)`. Again, this only works if the column is **identically named** for both tables.


```python
q = """
SELECT *
FROM orderdetails
JOIN products
    USING(productCode)
LIMIT 10
;
"""
pd.read_sql(q, conn)
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
    <tr>
      <th>5</th>
      <td>10101</td>
      <td>S18_2795</td>
      <td>26</td>
      <td>167.06</td>
      <td>1</td>
      <td>1928 Mercedes-Benz SSK</td>
      <td>Vintage Cars</td>
      <td>1:18</td>
      <td>Gearbox Collectibles</td>
      <td>This 1:18 replica features grille-mounted chro...</td>
      <td>548</td>
      <td>72.56</td>
      <td>168.75</td>
    </tr>
    <tr>
      <th>6</th>
      <td>10101</td>
      <td>S24_1937</td>
      <td>45</td>
      <td>32.53</td>
      <td>3</td>
      <td>1939 Chevrolet Deluxe Coupe</td>
      <td>Vintage Cars</td>
      <td>1:24</td>
      <td>Motor City Art Classics</td>
      <td>This 1:24 scale die-cast replica of the 1939 C...</td>
      <td>7332</td>
      <td>22.57</td>
      <td>33.19</td>
    </tr>
    <tr>
      <th>7</th>
      <td>10101</td>
      <td>S24_2022</td>
      <td>46</td>
      <td>44.35</td>
      <td>2</td>
      <td>1938 Cadillac V-16 Presidential Limousine</td>
      <td>Vintage Cars</td>
      <td>1:24</td>
      <td>Classic Metal Creations</td>
      <td>This 1:24 scale precision die cast replica of ...</td>
      <td>2847</td>
      <td>20.61</td>
      <td>44.80</td>
    </tr>
    <tr>
      <th>8</th>
      <td>10102</td>
      <td>S18_1342</td>
      <td>39</td>
      <td>95.55</td>
      <td>2</td>
      <td>1937 Lincoln Berline</td>
      <td>Vintage Cars</td>
      <td>1:18</td>
      <td>Motor City Art Classics</td>
      <td>Features opening engine cover, doors, trunk, a...</td>
      <td>8693</td>
      <td>60.62</td>
      <td>102.74</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10102</td>
      <td>S18_1367</td>
      <td>41</td>
      <td>43.13</td>
      <td>1</td>
      <td>1936 Mercedes-Benz 500K Special Roadster</td>
      <td>Vintage Cars</td>
      <td>1:18</td>
      <td>Studio M Art Models</td>
      <td>This 1:18 scale replica is constructed of heav...</td>
      <td>8635</td>
      <td>24.26</td>
      <td>53.91</td>
    </tr>
  </tbody>
</table>
</div>



## More Aliasing 

You can also assign tables an **alias** by entering an alternative shorthand name. This is slightly different than the previous lesson where we introduced aliases for column names, since now we are aliasing *tables*.

When aliasing columns the goal is usually to improve readability by giving something a more specific or easier-to-read name. For example, `name AS employee_name`, `AVG(AVG) AS average_batting_average`, or `COUNT(*) AS num_products`.

When aliasing tables the goal is usually to shorten the name, in order to shorten the overall query. So typically you'll see examples that alias a longer table name to a one-character or two-character shorthand. For example, `orderdetails AS od` or `products AS p`. (It is also possible to use aliases to clarify what exactly is in a table, like how aliases are used for columns, just less common.)

The following query produces the same result as the previous ones, using aliases `od` and `p` for `orderdetails` and `products`, respectively:


```python
q = """
SELECT *
FROM orderdetails AS od
JOIN products AS p
    ON od.productCode = p.productCode
LIMIT 10;
"""
pd.read_sql(q, conn)
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
    <tr>
      <th>5</th>
      <td>10101</td>
      <td>S18_2795</td>
      <td>26</td>
      <td>167.06</td>
      <td>1</td>
      <td>S18_2795</td>
      <td>1928 Mercedes-Benz SSK</td>
      <td>Vintage Cars</td>
      <td>1:18</td>
      <td>Gearbox Collectibles</td>
      <td>This 1:18 replica features grille-mounted chro...</td>
      <td>548</td>
      <td>72.56</td>
      <td>168.75</td>
    </tr>
    <tr>
      <th>6</th>
      <td>10101</td>
      <td>S24_1937</td>
      <td>45</td>
      <td>32.53</td>
      <td>3</td>
      <td>S24_1937</td>
      <td>1939 Chevrolet Deluxe Coupe</td>
      <td>Vintage Cars</td>
      <td>1:24</td>
      <td>Motor City Art Classics</td>
      <td>This 1:24 scale die-cast replica of the 1939 C...</td>
      <td>7332</td>
      <td>22.57</td>
      <td>33.19</td>
    </tr>
    <tr>
      <th>7</th>
      <td>10101</td>
      <td>S24_2022</td>
      <td>46</td>
      <td>44.35</td>
      <td>2</td>
      <td>S24_2022</td>
      <td>1938 Cadillac V-16 Presidential Limousine</td>
      <td>Vintage Cars</td>
      <td>1:24</td>
      <td>Classic Metal Creations</td>
      <td>This 1:24 scale precision die cast replica of ...</td>
      <td>2847</td>
      <td>20.61</td>
      <td>44.80</td>
    </tr>
    <tr>
      <th>8</th>
      <td>10102</td>
      <td>S18_1342</td>
      <td>39</td>
      <td>95.55</td>
      <td>2</td>
      <td>S18_1342</td>
      <td>1937 Lincoln Berline</td>
      <td>Vintage Cars</td>
      <td>1:18</td>
      <td>Motor City Art Classics</td>
      <td>Features opening engine cover, doors, trunk, a...</td>
      <td>8693</td>
      <td>60.62</td>
      <td>102.74</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10102</td>
      <td>S18_1367</td>
      <td>41</td>
      <td>43.13</td>
      <td>1</td>
      <td>S18_1367</td>
      <td>1936 Mercedes-Benz 500K Special Roadster</td>
      <td>Vintage Cars</td>
      <td>1:18</td>
      <td>Studio M Art Models</td>
      <td>This 1:18 scale replica is constructed of heav...</td>
      <td>8635</td>
      <td>24.26</td>
      <td>53.91</td>
    </tr>
  </tbody>
</table>
</div>



Note that just like with column aliases, the `AS` keyword is optional in SQLite. So, instead of `FROM orderdetails AS od` you could write `FROM orderdetails od` with the same outcome.

It is somewhat more common to see `AS` used with column aliases and skipped with table aliases, but again, you'll want to check the syntax rules of your particular type of SQL as well as style guidelines from your employer to know which syntax to use in a professional setting.

## `LEFT JOIN`s

By default a `JOIN` is an `INNER JOIN`, or the intersection between two tables. In other words, the `JOIN` between orders and products is only for `productCodes` that are in both the `orderdetails` and `products` tables. If a product had yet to be ordered (and wasn't in the `orderdetails` table) then it would also not be in the result of the `JOIN`.

The `LEFT JOIN` keyword returns all records from the left table (table1), and the matched records from the right table (table2). The result is NULL from the right side if there is no match.

There are many other types of joins, displayed below. Of these, SQLite does not support outer joins, but it is good to be aware of as more powerful versions of SQL such as PostgreSQL support these additional functions.

<img src='images/venn.png' width="700">

For example, the statement  
  
`SELECT * FROM products LEFT JOIN orderdetails `  

would return all products, even those that hadn't been ordered. 
You can imagine that all products in inventory should have a description in the product table, but perhaps not every product is represented in the orderdetails table. 


```python
q = """
SELECT *
FROM products
LEFT JOIN orderdetails
    USING(productCode)
;
"""
df = pd.read_sql(q, conn)

print("Number of records returned:", len(df))
print("Number of records where order details are null:", len(df[df.orderNumber.isnull()]))
```

    Number of records returned: 2997
    Number of records where order details are null: 1



```python
df[df.orderNumber.isnull()]
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
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



As you can see, it's a rare occurrence, but there is one product that has yet to be ordered.

## Primary Versus Foreign Keys

Another important consideration when performing joins is to think more about the key or column you are joining on. As you'll see in upcoming lessons, this can lead to interesting behavior if the join value is not unique in one or both of the tables. In all of the above examples, you joined two tables using the **primary key**. The primary key(s) of a table are those column(s) which uniquely identify a row. You'll also see this designated in our schema diagram with the asterisk (*).
<img src='images/Database-Schema.png' width=550>

You can also join tables using **foreign keys** which are not the primary key for that particular table, but rather another table. For example, `employeeNumber` is the primary key for the employees table and corresponds to the `salesRepEmployeeNumber` of the customers table. In the customers table, `salesRepEmployeeNumber` is only a foreign key, and is unlikely to be a unique identifier, as it is likely that an employee serves multiple customers. As such, in the resulting view `employeeNumber` would no longer be a unique field.


```python
q = """
SELECT *
FROM customers AS c
JOIN employees AS e
    ON c.salesRepEmployeeNumber = e.employeeNumber
ORDER By employeeNumber
;
"""
pd.read_sql(q, conn)
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
      <td>210500</td>
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
      <td>64600</td>
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
      <td>84600</td>
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
      <td>105000</td>
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
      <td>77600</td>
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
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>95</th>
      <td>298</td>
      <td>Vida Sport, Ltd</td>
      <td>Holz</td>
      <td>Mihael</td>
      <td>0897-034555</td>
      <td>Grenzacherweg 237</td>
      <td></td>
      <td>Genève</td>
      <td></td>
      <td>1203</td>
      <td>...</td>
      <td>1702</td>
      <td>141300</td>
      <td>1702</td>
      <td>Gerard</td>
      <td>Martin</td>
      <td>x2312</td>
      <td>mgerard@classicmodelcars.com</td>
      <td>4</td>
      <td>1102</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>96</th>
      <td>344</td>
      <td>CAF Imports</td>
      <td>Fernandez</td>
      <td>Jesus</td>
      <td>+34 913 728 555</td>
      <td>Merchants House</td>
      <td>27-30 Merchant's Quay</td>
      <td>Madrid</td>
      <td></td>
      <td>28023</td>
      <td>...</td>
      <td>1702</td>
      <td>59600</td>
      <td>1702</td>
      <td>Gerard</td>
      <td>Martin</td>
      <td>x2312</td>
      <td>mgerard@classicmodelcars.com</td>
      <td>4</td>
      <td>1102</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>97</th>
      <td>376</td>
      <td>Precious Collectables</td>
      <td>Urs</td>
      <td>Braun</td>
      <td>0452-076555</td>
      <td>Hauptstr. 29</td>
      <td></td>
      <td>Bern</td>
      <td></td>
      <td>3012</td>
      <td>...</td>
      <td>1702</td>
      <td>0</td>
      <td>1702</td>
      <td>Gerard</td>
      <td>Martin</td>
      <td>x2312</td>
      <td>mgerard@classicmodelcars.com</td>
      <td>4</td>
      <td>1102</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>98</th>
      <td>458</td>
      <td>Corrida Auto Replicas, Ltd</td>
      <td>Sommer</td>
      <td>Martín</td>
      <td>(91) 555 22 82</td>
      <td>C/ Araquil, 67</td>
      <td></td>
      <td>Madrid</td>
      <td></td>
      <td>28023</td>
      <td>...</td>
      <td>1702</td>
      <td>104600</td>
      <td>1702</td>
      <td>Gerard</td>
      <td>Martin</td>
      <td>x2312</td>
      <td>mgerard@classicmodelcars.com</td>
      <td>4</td>
      <td>1102</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>99</th>
      <td>484</td>
      <td>Iberia Gift Imports, Corp.</td>
      <td>Roel</td>
      <td>José Pedro</td>
      <td>(95) 555 82 82</td>
      <td>C/ Romero, 33</td>
      <td></td>
      <td>Sevilla</td>
      <td></td>
      <td>41101</td>
      <td>...</td>
      <td>1702</td>
      <td>65700</td>
      <td>1702</td>
      <td>Gerard</td>
      <td>Martin</td>
      <td>x2312</td>
      <td>mgerard@classicmodelcars.com</td>
      <td>4</td>
      <td>1102</td>
      <td>Sales Rep</td>
    </tr>
  </tbody>
</table>
<p>100 rows × 21 columns</p>
</div>



Notice that this also returned both columns: `salesRepEmployeeNumber` and `employeeNumber`. These columns contain identical values so you would probably actually only want to select one or the other.

## Summary

In this lesson, you investigated joins. This included implementing the `ON` and `USING` clauses, aliasing table names, implementing `LEFT JOIN`, and using primary vs. foreign keys.
