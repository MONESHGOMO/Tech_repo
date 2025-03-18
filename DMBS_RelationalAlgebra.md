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

