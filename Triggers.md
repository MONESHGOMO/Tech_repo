## 🚫 Example: Enforce No Negative Balance on Account Table

---

### 🏦 Step 1: Create Table

```sql
CREATE TABLE Account (
    acc_no INT PRIMARY KEY,
    name VARCHAR(100),
    balance DECIMAL(10,2)
);
```

---

### 🛡️ Step 2: Create Trigger to Prevent Negative Balance on INSERT

```sql
DELIMITER $$

CREATE TRIGGER trg_check_balance_insert
BEFORE INSERT ON Account
FOR EACH ROW
BEGIN
    IF NEW.balance < 0 THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Balance cannot be negative';
    END IF;
END $$

DELIMITER ;
```

---

### 🛡️ Step 3: Create Trigger to Prevent Negative Balance on UPDATE

```sql
DELIMITER $$

CREATE TRIGGER trg_check_balance_update
BEFORE UPDATE ON Account
FOR EACH ROW
BEGIN
    IF NEW.balance < 0 THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Balance cannot be negative';
    END IF;
END $$

DELIMITER ;
```

---

## ❓ How It Works?

- When you try to **insert** or **update** a row in `Account` where `balance < 0`, the trigger runs.
- The trigger raises an error (`SIGNAL SQLSTATE '45000'`), stopping the operation.
- This keeps your data **consistent and valid**.

---

## 📝 Summary Table

| Term      | Description                                      |
|-----------|--------------------------------------------------|
| Trigger   | Automatic procedure fired by DB events           |
| BEFORE/AFTER | When the trigger runs relative to event       |
| Events    | `INSERT`, `UPDATE`, `DELETE`                     |
| NEW       | Keyword representing the new row data            |
| OLD       | Keyword representing the old row data (in UPDATE or DELETE) |

---

## ⚠️ Key Points: When **NOT** to Use Triggers

- When business logic can be handled in the application layer (for better clarity and debugging).
- If triggers make debugging or understanding data flow difficult.
- When performance is critical (triggers can slow down bulk operations).
- If you need to avoid hidden side effects (triggers can cause unexpected changes).
- When portability across different DBMS is required (trigger syntax varies).

---

## 🔢 How Many Triggers Can You Specify on a Table?

- In MySQL, you can have **one trigger per event per timing** on a table.
- That means:  
  - 1 BEFORE INSERT  
  - 1 AFTER INSERT  
  - 1 BEFORE UPDATE  
  - 1 AFTER UPDATE  
  - 1 BEFORE DELETE  
  - 1 AFTER DELETE  
- **Maximum:** 6 triggers per table (one for each timing/event combination).
