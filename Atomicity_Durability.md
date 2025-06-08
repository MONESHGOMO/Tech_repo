# ✅ ACID Property Example – ATM Withdrawal (Banking System)

## 🔔 Scenario:
When a customer withdraws money from an ATM, the system must:
1. ✅ Check account balance.
2. 💸 Deduct the amount.
3. 🧾 Record the transaction in a log/history.

All three operations must succeed **together** (Atomicity) and must be **permanent** once done (Durability).

---

## 🗃️ Database Tables

### 1️⃣ accounts Table

```sql
CREATE TABLE accounts (
    account_id INT PRIMARY KEY,
    name VARCHAR(100),
    balance DECIMAL(10,2)
);
```
2️⃣ transactions Table


```sql
CREATE TABLE transactions (
    transaction_id INT AUTO_INCREMENT PRIMARY KEY,
    account_id INT,
    amount DECIMAL(10,2),
    type VARCHAR(10),  -- e.g., 'withdraw'
    transaction_time DATETIME
);
```
🔁 Transaction Block: Withdraw ₹1000


```sql
START TRANSACTION;

-- Step 1: Deduct ₹1000 from account if balance is enough
UPDATE accounts 
SET balance = balance - 1000 
WHERE account_id = 1 AND balance >= 1000;

-- Step 2: Record the transaction in history
INSERT INTO transactions (account_id, amount, type, transaction_time)
VALUES (1, 1000, 'withdraw', NOW());

-- Step 3: Commit if both operations succeeded
COMMIT;

-- If any error occurs before commit, rollback can be used
-- ROLLBACK;
```

✅ How This Example Shows ACID

<h3>🔸 Atomicity:</h3>
<ul>
  <li>If either <strong>balance update</strong> or <strong>transaction insert</strong> fails,</li>
  <li>The entire transaction is <strong>rolled back</strong>.</li>
  <li><strong>No partial changes</strong> are made.</li>
  <li>Ensures <em>data consistency</em> (no money disappears).</li>
</ul>

<h3>🔸 Durability:</h3>
<ul>
  <li>After <code>COMMIT</code>, changes are <strong>permanently saved</strong>.</li>
  <li>Even if the system crashes right after commit,</li>
  <li>The <strong>balance change</strong> and <strong>transaction log</strong> will <u>not be lost</u>.</li>
  <li>Ensures <em>reliability</em> and <em>trust</em>.</li>
</ul>


