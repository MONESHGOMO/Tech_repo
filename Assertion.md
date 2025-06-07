# ✅ SQL Assertion Example (Bank Account - Balance Constraint)

## 📘 Overview

An **assertion** in SQL is a rule that ensures the **validity of data across the entire table** (or even multiple tables).  
It is a **global constraint** that the database checks **automatically after every data change** (INSERT, UPDATE, DELETE).

> ❗ **Note:** MySQL does **not support** `CREATE ASSERTION` directly.  
> This document shows how it works in standard SQL, and how to simulate it using **triggers** in MySQL.

---

## 🏦 Scenario: Bank Account Balance

We want to make sure that:

> 🔒 No customer in the `Account` table should have a **negative balance**.

---

## ✅ Standard SQL: Using `CREATE ASSERTION`

```sql
CREATE ASSERTION positive_balance
CHECK (
    NOT EXISTS (
        SELECT * FROM Account WHERE balance < 0
    )
);
```

## ✅ Line-by-line Explanation of SQL Assertion

### 1. `CREATE ASSERTION positive_balance`

- `CREATE ASSERTION`: This starts the command to create an **assertion**, which is a **global rule** in SQL.
- `positive_balance`: This is the **name of the assertion**. You can name it anything, just like a variable.

---

### 2. `CHECK ( ... )`

- This part defines the **condition** that must **always be true** in the database.
- The database automatically checks this condition **after every INSERT, UPDATE, or DELETE**.
- If the condition becomes **false**, the operation will be **rejected** by the database.

---

### 3. `NOT EXISTS (SELECT * FROM Account WHERE balance < 0)`

- This checks:
  > “Is there **any row** in the `Account` table where `balance` is less than 0?”

- `EXISTS`: returns `TRUE` if such rows **do exist**.
- `NOT EXISTS`: returns `TRUE` only if **no such row exists**.

---

### ✅ What this means logically:

> ✔️ “Only allow the change if **no account** has `balance < 0`.”

If someone tries to insert or update an account with a **negative balance**, the assertion will **fail**, and the operation will be **blocked**.




- ✅ If anyone tries to insert or update a row where `balance < 0`, the database will reject it.

---

### ✅ Step 1: Create the Table

```sql
CREATE TABLE Account (
    acc_no INT PRIMARY KEY,
    name VARCHAR(100),
    balance DECIMAL(10, 2)
);
```





---
