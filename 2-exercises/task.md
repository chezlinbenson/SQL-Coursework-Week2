# E-Commerce Database

In this homework, you are going to work with an ecommerce database. In this database, you have `products` that `consumers` can buy from different `suppliers`. Customers can create an `order` and several products can be added in one order.

## Submission

Below you will find a set of tasks for you to complete to set up a database for an e-commerce app.

To submit this homework write the correct commands for each question here:
```sql


```

When you have finished all of the questions - open a pull request with your answers to the `Databases-Homework` repository.

## Setup

To prepare your environment for this homework, open a terminal and create a new database called `cyf_ecommerce`:

```sql
createdb cyf_ecommerce
```

Import the file [`cyf_ecommerce.sql`](./cyf_ecommerce.sql) in your newly created database:

```sql
psql -d cyf_ecommerce -f cyf_ecommerce.sql
```

Open the file `cyf_ecommerce.sql` in VSCode and examine the SQL code. Take a piece of paper and draw the database with the different relationships between tables (as defined by the REFERENCES keyword in the CREATE TABLE commands). Identify the foreign keys and make sure you understand the full database schema.

## Task

Once you understand the database that you are going to work with, solve the following challenge by writing SQL queries using everything you learned about SQL:

1. Retrieve all the customers' names and addresses who live in the United States  

--select name, address from customers where country = 'United States';

2. Retrieve all the customers in ascending name sequence  

--select * from customers order by "name" asc;

3. Retrieve all the products whose name contains the word `socks`  

--select * from products where product_name like '%socks%'

4. Retrieve all the products which cost more than 100 showing product id, name, unit price and supplier id.  

--select product_availability.prod_id, products.product_name, product_availability.unit_price , product_availability.supp_id from product_availability
--inner join products on products.id = product_availability.prod_id
--where product_availability.unit_price > 100;

5. Retrieve the 5 most expensive products  

--select * from product_availability 
--order by unit_price desc
--limit 5

6. Retrieve all the products with their corresponding suppliers. The result should only contain the columns `product_name`, `unit_price` and `supplier_name`  

--SELECT products.product_name, product_availability.unit_price, suppliers.supplier_name from product_availability 
--inner join products on products.id = product_availability.prod_id  
--inner join suppliers on suppliers.id = product_availability.supp_id;

7. Retrieve all the products sold by suppliers based in the United Kingdom. The result should only contain the columns `product_name` and `supplier_name`.  

--select products.product_name, suppliers.supplier_name, from suppliers
--inner join products on products.id = suppliers.id 
--where suppliers.country = 'United Kingdom';

8. Retrieve all orders, including order items, from customer ID `1`. Include order id, reference, date and total cost (calculated as quantity * unit price).  

--select orders.id,order_reference, order_date, sum(product_availability.unit_price  * order_items.quantity) as total_cost
--from orders
--inner join order_items on order_items.order_id = orders.id
--inner join product_availability on product_availability.prod_id = order_items.product_id
--where orders.customer_id = 1
--GROUP BY orders.id,order_items.id,product_availability.prod_id,product_availability.supp_id;


9. Retrieve all orders, including order items, from customer named `Hope Crosby`  

--select orders, order_items from orders
--inner join order_items on order_items.order_id = orders.id 
--inner join customers on customers.id = orders.customer_id 
--where customers.name like '%Hope Crosby%';

10. Retrieve all the products in the order `ORD006`. The result should only contain the columns `product_name`, `unit_price` and `quantity`.  

--select product_name, unit_price, quantity from products  
--inner join product_availability on product_availability.prod_id = products.id  
--inner join order_items on order_items.order_id = product_availability.prod_id
--inner join orders on orders.id = order_items.order_id 
--where orders.order_reference = 'ORD006';

11. Retrieve all the products with their supplier for all orders of all customers. The result should only contain the columns `name` (from customer), `order_reference`, `order_date`, `product_name`, `supplier_name` and `quantity`.  

--select name, order_reference, order_date, product_name, supplier_name, quantity from customers
--inner join orders on orders.customer_id = customers.id  
--inner join products on products.id = customers.id 
--inner join suppliers on suppliers.id = products.id
--inner join order_items on order_items.supplier_id = suppliers.id

12. Retrieve the names of all customers who bought a product from a supplier based in China.  

--select distinct customers.name
--from customers
--inner join orders on orders.customer_id  = customers.id
--inner join order_items on order_items.order_id = orders.id
--inner join products on products.id = order_items.product_id
--inner join suppliers on suppliers.id = order_items.supplier_id
--where suppliers.country = 'China';

13. List all orders giving customer name, order reference, order date and order total amount (quantity * unit price) in descending order of total.  

--select customers.name, orders.order_reference, orders.order_date, sum(order_items.quantity * product_availability.unit_price) as order_total_amount
--from orders
--inner join customers on customers.id = orders.customer_id
--inner join order_items on order_items.order_id = orders.id
--inner join product_availability  on product_availability.prod_id = order_items.product_id
--group by customers.name, order_reference, orders.order_date
--order by order_total_amount desc;
