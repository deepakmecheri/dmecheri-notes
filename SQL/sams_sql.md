[Source](https://learning.oreilly.com/library/view/sams-teach-yourself/9780135182925/)

[Live SQL](https://livesql.oracle.com/)

[Challenge answers](https://forta.com/books/0135182794/challenges/)

# Lesson 1. Understanding SQL

## Database Basics

A **database** is a collection of data stored in some organised fashion. The term database does not refer to the database software. The software is called **Database Management System (DBMS)**.

Inside a database there will be multiple **tables** having unique names. Each table will have a **schema** specific to itself which describes the table layout and properties.

A table consists of one or more **columns** of data. Each column will be constrained to have values of a particular **datatype**. Each record will correspond to a **row** in the database.

Every row in a table should have some column (or set of columns) that uniquely identifies it called the **primary key**. Without a primary key, updating or deleting specific rows in a table becomes extremely difficult as there is no guaranteed safe way to refer to just the rows to be affected. It is good practice to always include primary keys in your tables.

Any column in a table can be defined as the primary key, as long as it meets the following conditions:

- No two rows can have the same primary key value.
-  Every row must have a value in the primary key column(s). (So, no NULL values.)
-  Values in primary key columns should never be modified or updated.
-  Primary key values should never be reused. (If a row is deleted from the table, its primary key may not be assigned to any new rows in the future.)

# Lesson 2. Retrieving Data

We use `SELECT` statement to retrieve one or more columns of data from a table.

```sql
-- Retrieve individual columns
SELECT prod_name
FROM Products;
```
```sql
-- Retrieve multiple columns
SELECT prod_id, prod_name, prod_price
FROM Products;
```
```sql
-- Retrieve all columns
SELECT *
FROM Products;
```
```sql
-- Retrieve distinct values only
SELECT DISTINCT vend_id
FROM Products;
-- Retrieve unique combinations of multiple columns
SELECT DISTINCT vend_id, prod_id 
FROM products;
```
```sql
-- Limit results
SELECT prod_name
FROM Products
WHERE ROWNUM <=5;
```
## Challenges
**Q**.  Write a SQL statement to retrieve all customer IDs (`cust_id`) from the  `Customers`  table.

**Ans**: 
```sql
select cust_id
from customers;
```

**Q**. The  `OrderItems`  table contains every item ordered (and some were ordered multiple times). Write a SQL statement to retrieve a list of the products (`prod_id`) ordered (not every order, just a unique list of products). Here’s a hint: you should end up with seven unique rows displayed.

**Ans**:
```sql
select distinct prod_id
from orderitems;
```
**Q**. Write a SQL statement that retrieves all columns from the  `Customers`  table and an alternate  `SELECT`  that retrieves just the customer ID. Use comments to comment out one  `SELECT`  so as to be able to run the other. (And, of course, test both statements.)

**Ans**:
```sql
select *
-- select cust_id
from customers;
```

# Lesson 3. Sorting Retrieved Data

To explicitly sort data retrieved using a `SELECT` statement, you use the `ORDER BY` clause.

```sql
-- sort by product name
SELECT prod_name
FROM Products
ORDER BY prod_name;
```
>Note: When specifying an `ORDER BY` clause, be sure that it is the last clause in your `SELECT` statement. If it is not the last clause, an error will be generated.

```sql
-- sort by product price, then by product name
SELECT prod_id, prod_price, prod_name
FROM Products
ORDER BY prod_price, prod_name;
```
```sql
-- sorting by colummn position
-- equivalent to above code block
SELECT prod_id, prod_price, prod_name
FROM Products
ORDER BY 2, 3;
```
```sql
-- specify sort direction
SELECT prod_id, prod_price, prod_name
FROM Products
ORDER BY prod_price DESC;
```
```sql
-- sort direction with multiple columns
SELECT prod_id, prod_price, prod_name
FROM Products
ORDER BY prod_price DESC, prod_name;
```

## Challenges

**Q**. Write a SQL statement to retrieve all customer names (`cust_names`) from the  `Customers`  table, and display the results sorted from  `Z`  to  `A`.

**Ans**:
```sql
select cust_name
from customers
order by cust_name desc;
```

**Q**. Write a SQL statement to retrieve customer ID (`cust_id`) and order number (`order_num`) from the  `Orders`  table, and sort the results first by customer ID and then by order date in reverse chronological order.

**Ans**:
```sql
select cust_id, order_num
from orders
order by cust_id, order_date;
```

**Q**. Our fictitious store obviously prefers to sell more expensive items, and lots of them. Write a SQL statement to display the quantity and price (`item_price`) from the  `OrderItems`  table, sorted with the highest quantity and highest price first.

**Ans**:
```sql
select * 
from orderitems
order by quantity desc, item_price desc;
```

# Lesson 4. Filtering Data

Within a `SELECT` statement, data is filtered using the `WHERE` clause. The `WHERE` clause is specified right after the table name (`FROM` clause).

```sql
SELECT prod_name, prod_price
FROM Products
WHERE prod_price = 3.49;
```
```sql
SELECT prod_name, prod_price
FROM products
WHERE prod_price BETWEEN 5 AND 10;
```

> Note: While using `ORDER BY` and `WHERE` clauses together, make sure `ORDER BY` comes after `WHERE`.

When a table is created the default values of columns will be `NULL`. We can query these `NULL` columns using `IS NULL`.
```sql
SELECT prod_name
FROM Products
WHERE prod_price IS NULL;
```

## Challenges
**Q**. Write a SQL statement to retrieve the product ID (`prod_id`) and name (`prod_name`) from the `Products` table, returning only products with a price of `9.49`.

**Ans**. 
```sql
SELECT prod_id, prod_name
FROM products
WHERE prod_price = 9.49;
```

**Q**. Write a SQL statement to retrieve the product ID (`prod_id`) and name (`prod_name`) from the Products table, returning only products with a price of `9` or more.

**Ans**.
```sql
SELECT prod_id, prod_name
FROM products
WHERE prod_price >= 9;
```

**Q**. Write a SQL statement that retrieves the unique list of order numbers (`order_num`) from the `OrderItems` table, which contain `100` or more of any item.

**Ans**.
```sql
SELECT DISTINCT order_num
FROM orderitems
WHERE quantity >= 100;
```

**Q**. Write a SQL statement that returns the product name (`prod_name`) and price (`prod_price`) from `Products` for all products priced between `3` and `6`. Oh, and sort the results by price. 

**Ans**.
```sql
SELECT prod_name, prod_price 
FROM products
WHERE prod_price BETWEEN 3 AND 6
ORDER BY prod_price;
```

# Lesson 5. Advanced Data Filtering

Use `AND` and `OR` to combine logical conditions

```sql
SELECT prod_id, prod_price, prod_name
FROM Products
WHERE vend_id = 'DLL01' AND prod_price <= 4;
```
```sql
SELECT prod_id, prod_price, prod_name
FROM Products
WHERE vend_id = 'DLL01' OR vend_id = 'BRS01';
```

SQL (like most languages) will evaluate `AND`s before `OR`s. Make sure to add parentheses in your expressions to make the order of evaluation explicit.
```sql
SELECT prod_name, prod_price
FROM Products
WHERE (vend_id = 'DLL01' OR vend_id = 'BRS01')
      AND prod_price >= 10;
```

The `IN` operator can be used to specify a range of conditions, any of which can be matched.
```sql
SELECT prod_name, prod_price
FROM Products
WHERE vend_id  IN ('DLL01','BRS01')
ORDER BY prod_name;
```

The `NOT` operator when used will negate any condition that it precedes.
```sql
SELECT prod_name
FROM Products
WHERE NOT vend_id = 'DLL01'
ORDER BY prod_name;
```

## Challenges

**Q**. Write a SQL statement to retrieve the vendor name (`vend_name`) from the `Vendors` table, returning only vendors in California.

**Ans**.
```sql
SELECT *
FROM vendors
WHERE vend_country = 'USA'
    AND vend_state = 'CA';
```

**Q**. Write a SQL statement to find all orders where at least `100` of items `BR01`, `BR02`, or `BR03` were ordered. You’ll want to return order number (`order_num`), product ID (`prod_id`), and quantity for the `OrderItems` table, filtering by both the product ID and quantity.

**Ans**.
```sql
SELECT DISTINCT order_num
FROM orderitems
WHERE prod_id in ('BR01', 'BR02', 'BR03')
    AND quantity >= 100;
```

**Q**. Write a SQL statement that returns the product name (`prod_name`) and price (`prod_price`) from `Products` for all products priced between `3` and `6`. Use an `AND`, and sort the results by price.

**Ans**.
```sql
SELECT prod_name, prod_price
FROM products
WHERE prod_price >= 3
    AND prod_price <= 6
ORDER BY prod_price;
```

**Q**. What is wrong with the following SQL statement?
```sql
SELECT vend_name
FROM Vendors
ORDER BY vend_name
WHERE vend_country = 'USA' AND vend_state = 'CA';
```
**Ans**. `ORDER BY` should go after `WHERE`


# Lesson 6. Using Wildcard Filtering

Wildcard searches can be made with the help of `LIKE` operator and wildcards. Some of the most commonly used wildcards are

`%` Matches zero or more characters

`_` Matches a single character

`[]` Specify a set of characters, any one of which must match a character in the specified position

```sql
SELECT cust_contact
FROM Customers
WHERE cust_contact LIKE '[JM]%'
ORDER BY cust_contact;
```

The `[]` wildcard can be negated using `^` before the characters
```sql
SELECT cust_contact
FROM Customers
WHERE cust_contact LIKE '[^JM]%'
ORDER BY cust_contact;
```
