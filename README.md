1. Create a database named ecommerce.

  CREATE DATABASE ecommerce;
  USE ecommerce;


 2. Create three tables: customers, orders, and products.

 
  customers table

CREATE TABLE customers (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100),
    address VARCHAR(255)
);

orders  table

CREATE TABLE orders (
    id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    total_amount DECIMAL(10,2),
    FOREIGN KEY (customer_id) REFERENCES customers(id)
);

products  table

CREATE TABLE products (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    price DECIMAL(10,2),
    description TEXT
);

3. Insert some sample data into the tables.

INSERT INTO customers (name, email, address) VALUES
('Alice', 'alice@example.com', 'Chennai'),
('Bob', 'bob@example.com', 'Delhi'),
('Charlie', 'charlie@example.com', 'Mumbai');

INSERT INTO products (name, price, description) VALUES
('Product A', 50.00, 'Basic product A'),
('Product B', 75.00, 'Standard product B'),
('Product C', 40.00, 'Premium product C');

INSERT INTO orders (customer_id, order_date, total_amount) VALUES
(1, CURDATE() - INTERVAL 10 DAY, 150.00),
(2, CURDATE() - INTERVAL 5 DAY, 200.00),
(3, CURDATE() - INTERVAL 40 DAY, 100.00);



Queries to Write:


1.Retrieve all customers who have placed an order in the last 30 days.
SELECT DISTINCT c.*
FROM customers c
JOIN orders o ON c.id = o.customer_id
WHERE o.order_date >= CURDATE() - INTERVAL 30 DAY;

2.Get the total amount of all orders placed by each customer.

SELECT c.name, SUM(o.total_amount) AS total_spent
FROM customers c
JOIN orders o ON c.id = o.customer_id
GROUP BY c.id;

3.Update the price of Product C to 45.00.
UPDATE products SET price = 45.00 WHERE name = 'Product C';


4.Add a new column discount to the products table.
ALTER TABLE products ADD discount DECIMAL(5,2);

5.Retrieve the top 3 products with the highest price.
SELECT * FROM products ORDER BY price DESC LIMIT 3;

6.Get the names of customers who have ordered Product A.
requires normalized schema with order_items table (done later)

7.Join the orders and customers tables to retrieve the customer's name and order date for each order. 
SELECT c.name, o.order_date
FROM orders o
JOIN customers c ON o.customer_id = c.id;

8.Retrieve the orders with a total amount greater than 150.00.
SELECT * FROM orders WHERE total_amount > 150.00;

9.Normalize the database by creating a separate table for order items and updating the orders table to reference the order_items table.
CREATE TABLE order_items (
    id INT AUTO_INCREMENT PRIMARY KEY,
    order_id INT,
    product_id INT,
    quantity INT,
    FOREIGN KEY (order_id) REFERENCES orders(id),
    FOREIGN KEY (product_id) REFERENCES products(id)
);


INSERT INTO order_items (order_id, product_id, quantity) VALUES
(1, 1, 2),
(1, 2, 1),
(2, 1, 1),
(2, 3, 1);


10.Retrieve the average total of all orders

SELECT AVG(total_amount) AS avg_order_value FROM orders;


