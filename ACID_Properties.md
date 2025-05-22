# ✅ ACID Properties

| Property            | Meaning                                                            |
| ------------------- | ------------------------------------------------------------------ |
| **A** – Atomicity   | All changes in a transaction are completed or none at all.         |
| **C** – Consistency | Transaction brings the database from one valid state to another.   |
| **I** – Isolation   | Transactions don't interfere with each other.                      |
| **D** – Durability  | Once committed, changes are saved permanently, even after a crash. |

---

# Atomicity in Databases

### What is Atomicity?

- **Atomicity** means **"all or nothing"** for a set of database operations.
- A transaction is a group of SQL statements executed as a single unit.
- Either **all statements succeed and are committed**, or **if any statement fails, all changes are rolled back**.
- This ensures the database never ends up in an inconsistent or partial state.

---

### How is Atomicity Implemented?

- Atomicity is implemented using **transactions**.
- In MySQL, use the commands:
  - `START TRANSACTION;` — begin a transaction
  - `COMMIT;` — save all changes if successful
  - `ROLLBACK;` — undo all changes if an error occurs

---

### Example: Demonstrating Atomicity in MySQL

#### Step 1: Create a sample `accounts` table

```sql
CREATE TABLE accounts (
    account_id VARCHAR(10) PRIMARY KEY,
    balance DECIMAL(10,2)
);

INSERT INTO accounts (account_id, balance) VALUES 
('A', 1000.00),
('B', 500.00);

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
- Step 2: Money transfer without transaction (no atomicity)
```sql
-- Deduct 200 from account A
UPDATE accounts SET balance = balance - 200 WHERE account_id = 'A';

-- Intentional error: wrong table name causes failure
UPDATE accnt SET balance = balance + 200 WHERE account_id = 'B';
```
- RESULT

  ```bash
    The first update succeeds, deducting from A.
    
    The second update fails (table accnt doesn't exist).
    
    No rollback occurs, so the database is left inconsistent
  ```
  - Step 3: Money transfer with transaction (atomicity guaranteed)

``` sql
START TRANSACTION;

UPDATE accounts SET balance = balance - 200 WHERE account_id = 'A';

-- Intentional error to simulate failure
UPDATE accnt SET balance = balance + 200 WHERE account_id = 'B';

COMMIT;

```
- RESULT

```bash

The second statement fails.

The transaction rolls back automatically.

The balance of account A remains unchanged.

Database remains consistent.
```
Step 4: Verify balances
```sql

Edit
SELECT * FROM accounts;
```
-RESULT
```bash
Balances remain as before the transaction.

No partial updates happened.
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
