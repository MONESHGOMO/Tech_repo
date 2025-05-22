# ✅ ACID Properties

| Property            | Meaning                                                            |
| ------------------- | ------------------------------------------------------------------ |
| **A** – Atomicity   | All changes in a transaction are completed or none at all.         |
| **C** – Consistency | Transaction brings the database from one valid state to another.   |
| **I** – Isolation   | Transactions don't interfere with each other.                      |
| **D** – Durability  | Once committed, changes are saved permanently, even after a crash. |

---

## 🛠️ MySQL Transaction Commands

```sql
START TRANSACTION;  -- 🚦 Begins the transaction
-- SQL queries go here (INSERT, UPDATE, DELETE)
COMMIT;             -- ✅ Saves all changes permanently
ROLLBACK;           -- ❌ Cancels changes if an error occurs
```

---

```sql
START TRANSACTION;

UPDATE accounts SET balance = balance - 1000 WHERE account_id = 'A';
UPDATE accounts SET balance = balance + 1000 WHERE account_id = 'B';

COMMIT;
```

---

# 🍔 Scenario: Online Food Ordering System – Payment Process

## 🛒 Use Case

A customer places an order and pays online. The system must:

- 💸 Deduct the order amount from the customer's wallet.
- 📝 Add the order details to the orders table.
- 🍗 Update the restaurant's stock (ingredients used in the meal).

These 3 operations must succeed together, or none at all.

---

## 🗃️ Tables Used

- **customers** (`id`, `name`, `wallet_balance`)
- **orders** (`id`, `customer_id`, `total_amount`, `order_time`)
- **stock** (`item_id`, `item_name`, `quantity`)

---

## 🏗️ Step 1: Create Tables in MySQL

### 1️⃣ customers Table

```sql
CREATE TABLE customers (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    wallet_balance DECIMAL(10, 2)
);
```

### 2️⃣ orders Table

```sql
CREATE TABLE orders (
    id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT,
    total_amount DECIMAL(10, 2),
    order_time DATETIME,
    FOREIGN KEY (customer_id) REFERENCES customers(id)
);
```

### 3️⃣ stock Table

```sql
CREATE TABLE stock (
    item_id INT AUTO_INCREMENT PRIMARY KEY,
    item_name VARCHAR(100),
    quantity INT
);
```

---

## 📝 Step 2: Insert Sample Data

### 👤 Add a customer with ₹1000 in wallet

```sql
INSERT INTO customers (name, wallet_balance)
VALUES ('Gomo', 1000.00);
```

### 🍗 Add stock item (e.g., Chicken)

```sql
INSERT INTO stock (item_name, quantity)
VALUES ('Chicken', 10);
```

---

# 🚦 Ready for the Transaction

Now you're all set to run the transaction block for placing an order:

---

## 🔁 Transaction Logic (₹500 chicken order)

```sql
START TRANSACTION;

-- 1️⃣ Deduct from wallet
UPDATE customers 
SET wallet_balance = wallet_balance - 500 
WHERE id = 1;

-- 2️⃣ Insert order
INSERT INTO orders (customer_id, total_amount, order_time)
VALUES (1, 500, NOW());

-- 3️⃣ Update stock
UPDATE stock 
SET quantity = quantity - 2 
WHERE item_name = 'Chicken';

-- ✅ If all is successful
COMMIT;

-- ❌ If an error occurs
-- ROLLBACK;
```
