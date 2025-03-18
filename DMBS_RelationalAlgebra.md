## Selection vs Projection in Relational Algebra (DBMS)

In **Relational Algebra**, **Selection (σ) and Projection (π)** are two fundamental operations used to retrieve data from a relational database. They serve different purposes:

### **1. Selection (σ)**
- **Purpose**: Selection is used to **filter rows (tuples)** based on a specified condition.
- **Operation**: It selects only those rows that satisfy a given **predicate (condition)**.
- **Symbol**: **σ_condition (Relation)**
- **Effect**: It reduces the number of rows but keeps all columns.

#### **Example**:
Consider a **Student** table:

| Student_ID | Name  | Age | Department |
|------------|-------|-----|------------|
| 101        | John  | 20  | CS         |
| 102        | Alice | 21  | IT         |
| 103        | Bob   | 22  | CS         |

If we apply **σ_Age > 20 (Student)**, we get:

| Student_ID | Name  | Age | Department |
|------------|-------|-----|------------|
| 102        | Alice | 21  | IT         |
| 103        | Bob   | 22  | CS         |

---

### **2. Projection (π)**
- **Purpose**: Projection is used to **select specific columns (attributes)** from a relation.
- **Operation**: It eliminates unwanted columns and **removes duplicates automatically**.
- **Symbol**: **π_column1, column2, ... (Relation)**
- **Effect**: It reduces the number of columns but keeps all rows.

#### **Example**:
If we apply **π_Name, Age (Student)**, we get:

| Name  | Age |
|-------|-----|
| John  | 20  |
| Alice | 21  |
| Bob   | 22  |

---

### **Key Differences Between Selection and Projection**

| Feature       | Selection (σ) | Projection (π) |
|--------------|--------------|--------------|
| **Focus**     | Filters rows based on condition | Chooses specific columns |
| **Symbol**    | **σ_condition (Relation)** | **π_column1, column2, ... (Relation)** |
| **Effect**    | Reduces the number of rows | Reduces the number of columns |
| **Duplicate Handling** | Keeps all duplicates | Removes duplicates automatically |
| **Example Query** | σ_Age > 20 (Student) | π_Name, Age (Student) |

---

### **Summary**
- **Selection (σ)** → **Filters rows** based on a condition.
- **Projection (π)** → **Chooses specific columns** while removing duplicates.

## Union (∪), Intersection (∩), and Set Difference (−) in Relational Algebra

In **Relational Algebra**, **Union (∪), Intersection (∩), and Set Difference (−)** are **set operations** used to manipulate and compare relations (tables) with the **same structure** (i.e., the same number of attributes and matching attribute domains).  

---

## **1. Union (∪)**
- **Definition**: The **Union operation** combines the tuples of two relations and returns all **unique** tuples.
- **Symbol**: **R ∪ S**
- **Conditions for Union**:
  - Both relations **must have the same number of attributes**.
  - The **data types of corresponding attributes must match**.

### **Example**
#### **Given Two Relations:**
**CS_Students Table**  

| Student_ID | Name  |
|------------|-------|
| 101        | John  |
| 102        | Alice |

**IT_Students Table**  

| Student_ID | Name  |
|------------|-------|
| 102        | Alice |
| 103        | Bob   |

#### **Relational Algebra Expression:**
```
CS_Students ∪ IT_Students
```

#### **Result:**
| Student_ID | Name  |
|------------|-------|
| 101        | John  |
| 102        | Alice |
| 103        | Bob   |

---

## **2. Intersection (∩)**
- **Definition**: The **Intersection operation** returns only the **common tuples** that appear in **both** relations.
- **Symbol**: **R ∩ S**
- **Conditions for Intersection**:
  - Both relations **must have the same attributes and domains**.
  - It **returns only matching rows**.

### **Example**
Using the **same CS_Students and IT_Students tables**:

#### **Relational Algebra Expression:**
```
CS_Students ∩ IT_Students
```

#### **Result:**
| Student_ID | Name  |
|------------|-------|
| 102        | Alice |

---

## **3. Set Difference (−)**
- **Definition**: The **Set Difference operation** returns tuples that are **in the first relation but not in the second**.
- **Symbol**: **R − S**
- **Conditions for Set Difference**:
  - Both relations **must have the same attributes and domains**.
  - The result contains tuples that appear in **R but not in S**.

### **Example**
Using the **same CS_Students and IT_Students tables**:

#### **Relational Algebra Expression:**
```
CS_Students − IT_Students
```

#### **Result:**
| Student_ID | Name  |
|------------|-------|
| 101        | John  |

### **Key Points About Set Difference:**
✔ Returns tuples **only present in the first relation (R)**.  
✔ If there are **no exclusive tuples**, the result is **empty**.  
✔ Both tables must have **the same schema**.  

---

## **Other Types of Operations in Relational Algebra**
Relational Algebra consists of several fundamental operations:

### **1. Set Operations**
- **Union (∪)** – Combines tuples from two relations.
- **Intersection (∩)** – Returns only common tuples.
- **Set Difference (−)** – Finds tuples in one relation but not in another.

### **2. Basic Operations**
- **Selection (σ)** – Filters rows based on a condition.
- **Projection (π)** – Selects specific columns.

### **3. Cartesian Product (×)**
- **Combines two relations** to form all possible pairs of tuples.
- **Symbol**: **R × S**

### **4. Join Operations**
- **Theta Join (⨝θ)** – Combines two relations based on a condition.
- **Equi Join (⨝)** – A special case of Theta Join where the condition is equality.
- **Natural Join (⨝)** – Joins based on common attributes with the same name.
- **Outer Join (⟕, ⟖, ⟗)** – Includes unmatched tuples in the result.

### **5. Division (÷)**
- Used for queries involving "for all" conditions.
- **Finds tuples related to all values in another relation**.

---

## **Comparison Table:**
| **Operation**  | **Definition** | **Example Result** |
|---------------|--------------|----------------|
| **Union (∪)** | Returns all unique tuples from both relations | Combines rows and removes duplicates |
| **Intersection (∩)** | Returns only common tuples present in both relations | Shows only shared rows |
| **Set Difference (−)** | Finds tuples present in one relation but not the other | Excludes common rows |

Would you like detailed examples of **Join Operations** or **Division (÷)**? 🚀
