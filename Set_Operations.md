# 📝 SQL Set Operations Examples

Assume these two tables:

**Table: Students_A**

| Name  |
|-------|
| Gomo  |
| Riya  |
| Aman  |

**Table: Students_B**

| Name  |
|-------|
| Riya  |
| Neha  |
| Aman  |

---

## 1️⃣ UNION  
➡️ Combines unique records from both tables.

```bash
SELECT Name FROM Students_A
UNION
SELECT Name FROM Students_B;
```

📌 **Result:**

| Name  |
|-------|
| Gomo  |
| Riya  |
| Aman  |
| Neha  |

---

## 2️⃣ UNION ALL  
➡️ Combines all records from both tables, including duplicates.

```bash
SELECT Name FROM Students_A
UNION ALL
SELECT Name FROM Students_B;
```

📌 **Result:**

| Name  |
|-------|
| Gomo  |
| Riya  |
| Aman  |
| Riya  |
| Neha  |
| Aman  |

---

## 3️⃣ INTERSECT  
➡️ Returns only common records from both tables.

```bash
SELECT Name FROM Students_A
INTERSECT
SELECT Name FROM Students_B;
```

📌 **Result:**

| Name  |
|-------|
| Riya  |
| Aman  |

> ⚠️ **Note:** `INTERSECT` is not supported in MySQL directly (works in PostgreSQL, SQL Server, Oracle).  
> In MySQL, use `INNER JOIN` or `IN` as a workaround.

---

## 4️⃣ EXCEPT (PostgreSQL/SQL Server)  
➡️ Returns rows in Students_A that are not in Students_B.

```bash
SELECT Name FROM Students_A
EXCEPT
SELECT Name FROM Students_B;
```

📌 **Result:**

| Name  |
|-------|
| Gomo  |

---

## 5️⃣ MINUS (Oracle)  
➡️ Same as EXCEPT, used in Oracle SQL.

```bash
SELECT Name FROM Students_A
MINUS
SELECT Name FROM Students_B;
```

📌 **Result:**

| Name  |
|-------|
| Gomo  |
