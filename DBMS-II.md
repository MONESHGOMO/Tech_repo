# 📘 Structure of Relational Databases

## 🔷 Overview
A relational database organizes data into **tables** (called relations) made up of **rows** (tuples) and **columns** (attributes). It uses **keys** to connect and manage relationships among data.

---

## 🏪 Real-World Example: Online Grocery Store

### Tables:
- **Customers** (Stores user info)
- **Products** (Items for sale)
- **Orders** (Tracks purchases)
- **OrderItems** (Items inside each order)

| Table         | Columns (Attributes)                                                |
|---------------|---------------------------------------------------------------------|
| Customers     | customer_id (PK), name, email, phone                               |
| Products      | product_id (PK), name, price, category                              |
| Orders        | order_id (PK), customer_id (FK), order_date, total_amount           |
| OrderItems    | order_item_id (PK), order_id (FK), product_id (FK), quantity        |

---

## ✅ Key Concepts

### 1. **Table (Relation)**
- Represents a real-world entity
- Made up of columns and rows

### 2. **Primary Key (PK)**
- Uniquely identifies each row
- Cannot be NULL or duplicated

### 3. **Foreign Key (FK)**
- Links one table to another
- References the primary key in another table

### 4. **Relationships**
- **One-to-One**: Citizen ↔ Passport
- **One-to-Many**: Customer → Orders
- **Many-to-Many**: Orders ↔ Products (via OrderItems)

### 5. **Normalization**
- Organizing data to reduce redundancy
- Helps in maintaining data integrity

---

## 🔨 SQL Table Creation (Example)
```sql
CREATE TABLE Customers (
  customer_id INT PRIMARY KEY,
  name VARCHAR(50),
  email VARCHAR(100),
  phone VARCHAR(15)
);

CREATE TABLE Products (
  product_id INT PRIMARY KEY,
  name VARCHAR(50),
  price DECIMAL(6,2),
  category VARCHAR(30)
);

CREATE TABLE Orders (
  order_id INT PRIMARY KEY,
  customer_id INT,
  order_date DATE,
  total_amount DECIMAL(8,2),
  FOREIGN KEY (customer_id) REFERENCES Customers(customer_id)
);

CREATE TABLE OrderItems (
  order_item_id INT PRIMARY KEY,
  order_id INT,
  product_id INT,
  quantity INT,
  FOREIGN KEY (order_id) REFERENCES Orders(order_id),
  FOREIGN KEY (product_id) REFERENCES Products(product_id)
);
```

---

## 🎯 Must-Know Summary

| Concept         | Why It’s Important                       |
|----------------|-------------------------------------------|
| Table           | Stores structured data                   |
| Primary Key     | Ensures uniqueness in a table            |
| Foreign Key     | Connects data across tables              |
| Relationships   | Helps model real-world scenarios         |
| Normalization   | Reduces redundancy and improves integrity|
---

# 📘 Relational Algebra – With Real-World Examples

## 🔷 What is Relational Algebra?
Relational Algebra is a procedural query language for relational databases. It provides operations to manipulate relations (tables) and retrieve desired information.

---

## 🏪 Real-World Scenario: Online Grocery Store

We have:
- **Customers** Table
- **Orders** Table

### 🧾 Orders
| order_id | customer_id | total_amount |
|----------|-------------|--------------|
| 1        | 101         | 600.00       |
| 2        | 102         | 350.00       |
| 3        | 103         | 200.00       |

### 👤 Customers
| customer_id | name   | email             |
|-------------|--------|-------------------|
| 101         | Gomo   | gomo@email.com    |
| 102         | Monesh | monesh@email.com  |
| 104         | Ravi   | ravi@email.com    |

---

## 🔍 1. Selection (σ) – Filter Rows
**Get orders with total_amount > 300**

Relational Algebra:
```text
σ total_amount > 300 (Orders)
```

Result:
| order_id | customer_id | total_amount |
|----------|-------------|--------------|
| 1        | 101         | 600.00       |
| 2        | 102         | 350.00       |

SQL Equivalent:
```sql
SELECT * FROM Orders WHERE total_amount > 300;
```

---

## 🎯 2. Projection (π) – Select Columns
**Get all customer names**

Relational Algebra:
```text
π name (Customers)
```

Result:
| name   |
|--------|
| Gomo   |
| Monesh |
| Ravi   |

SQL Equivalent:
```sql
SELECT name FROM Customers;
```

---

## ➕ 3. Union (∪) – Combine Two Sets
Assume:
- `OrderCustomers` = customers who made orders
- `RegisteredCustomers` = all who registered

Relational Algebra:
```text
OrderCustomers ∪ RegisteredCustomers
```

SQL Equivalent:
```sql
SELECT * FROM OrderCustomers
UNION
SELECT * FROM RegisteredCustomers;
```

---

## ➖ 4. Difference (−) – Subtract Sets
**Find customers who registered but never ordered**

Relational Algebra:
```text
RegisteredCustomers − OrderCustomers
```

SQL Equivalent:
```sql
SELECT * FROM RegisteredCustomers
EXCEPT
SELECT * FROM OrderCustomers;
```

---

## 🔗 5. Join (⨝) – Merge Tables
**Get order details with customer names**

Relational Algebra:
```text
Orders ⨝ Orders.customer_id = Customers.customer_id Customers
```

Result:
| order_id | customer_id | total_amount | name   |
|----------|-------------|--------------|--------|
| 1        | 101         | 600.00       | Gomo   |
| 2        | 102         | 350.00       | Monesh |

SQL Equivalent:
```sql
SELECT Orders.order_id, Customers.name, Orders.total_amount
FROM Orders
JOIN Customers ON Orders.customer_id = Customers.customer_id;
```

---

## ✨ Summary Table
| Operation   | Symbol | Description                         | SQL Equivalent |
|-------------|--------|-------------------------------------|----------------|
| Selection   | σ      | Filters rows based on condition     | WHERE          |
| Projection  | π      | Select specific columns             | SELECT cols    |
| Union       | ∪      | Combine results from 2 sets         | UNION          |
| Difference  | −      | Get items in one set but not other  | EXCEPT         |
| Join        | ⨝      | Combine related rows from tables    | JOIN           |
| Cartesian   | ×      | Pair all rows from 2 tables         | CROSS JOIN     |
---
# Set Operations in SQL

Set operations are used to combine the results of two or more SQL queries. The main set operations are `UNION`, `INTERSECT`, and `EXCEPT`.

## 1. UNION
- **Purpose**: Combines the results of two queries into a **single result set**, removing duplicates.
- **Example**:
```sql
SELECT customer_id FROM OnlineCustomers
UNION
SELECT customer_id FROM InStoreCustomers;
```

## 2. INTERSECT

- Purpose: Returns only the common rows between two queries (rows that exist in both).

``` sql
SELECT customer_id FROM OnlineCustomers
INTERSECT
SELECT customer_id FROM InStoreCustomers;
```

## 3. EXCEPT (or MINUS in some databases)
   
- Purpose: Returns rows from the first query that do not exist in the second query.

``` sql
SELECT customer_id FROM OnlineCustomers
EXCEPT
SELECT customer_id FROM InStoreCustomers;

```
---
# Aggregate Functions in SQL

Aggregate functions are used to perform a calculation on a set of values and return a single result. These functions are often used with the `GROUP BY` clause in SQL to group rows that have the same values into summary rows. Here are the main aggregate functions:

1. **COUNT()**
2. **SUM()**
3. **AVG()**
4. **MIN()**
5. **MAX()**
--- 
# Null value  handling in DBMS
- It takes multiple arguments 
- COALESCE( )
 
```sql
SELECT name,
       COALESCE(email, phone, address) AS first_available_contact
FROM Users;

```
- IFNULL() Function in SQL

The `IFNULL()` function is used to replace `NULL` values with a default value.


```sql
IFNULL(expression, replacement_value)
```
---
# 📦 Nested Subqueries in SQL

A **subquery** (also known as a nested query) is a query within another SQL query. It is used to perform intermediate calculations that the main (outer) query can use.

---

## 🔹 Syntax

```sql
SELECT column_name
FROM table_name
WHERE column_name IN (SELECT column_name FROM another_table WHERE condition);
```
---
