***What is a Join?***

A **Join** lets you fetch related data from two or more tables based on a common column.

---

***Types of Joins in MySQL***

- **INNER JOIN** — Returns only matching rows between tables.
- **LEFT JOIN** (or LEFT OUTER JOIN) — Returns all rows from the left table, plus matching rows from the right table. If no match, NULLs on the right.
- **RIGHT JOIN** (or RIGHT OUTER JOIN) — Returns all rows from the right table, plus matching rows from the left table. If no match, NULLs on the left.
- **FULL JOIN** (or FULL OUTER JOIN) — Returns rows when there is a match in one of the tables.  
  *Note: MySQL does not support FULL JOIN directly, but you can emulate it.*
- **CROSS JOIN** — Returns the Cartesian product (all possible combinations) of rows from two tables.

---

***Example Setup***

Let’s say you have two tables:

**authors**

| author_id | name    |
|-----------|---------|
| 1         | Alice   |
| 2         | Bob     |
| 3         | Charlie |

**books**

| book_id | title   | author_id |
|---------|---------|-----------|
| 101     | Book A  | 1         |
| 102     | Book B  | 1         |
| 103     | Book C  | 3         |

---

***Step 1: Create Tables***

```sql
CREATE TABLE authors (
    author_id INT PRIMARY KEY,
    name VARCHAR(50)
);

CREATE TABLE books (
    book_id INT PRIMARY KEY,
    title VARCHAR(100),
    author_id INT,
    FOREIGN KEY (author_id) REFERENCES authors(author_id)
);
```

---

***Step 2: Insert Sample Data***

```sql
INSERT INTO authors VALUES
(1, 'Alice'),
(2, 'Bob'),
(3, 'Charlie');

INSERT INTO books VALUES
(101, 'Book A', 1),
(102, 'Book B', 1),
(103, 'Book C', 3);
```

---

***Step 3: INNER JOIN***

Fetch author names with their book titles.

```sql
SELECT authors.name, books.title
FROM authors
INNER JOIN books ON authors.author_id = books.author_id;
```

**Result:**

| name    | title   |
|---------|---------|
| Alice   | Book A  |
| Alice   | Book B  |
| Charlie | Book C  |

Only authors with books appear.

---

***Step 4: LEFT JOIN***

Returns all authors, even if they don’t have books. If an author has no book, the book columns will be NULL.

```sql
SELECT authors.name, books.title
FROM authors
LEFT JOIN books ON authors.author_id = books.author_id;
```

**Result:**

| name    | title   |
|---------|---------|
| Alice   | Book A  |
| Alice   | Book B  |
| Bob     | NULL    |
| Charlie | Book C  |

Shows all authors, even if they have no books.

---

***Step 5: RIGHT JOIN***

Returns all books, even if the book's author doesn’t exist in the authors table (in this example, all books have authors so output same as INNER JOIN).

```sql
SELECT authors.name, books.title
FROM authors
RIGHT JOIN books ON authors.author_id = books.author_id;
```

---

***Step 6: FULL OUTER JOIN (Emulated in MySQL)***

MySQL does NOT support FULL JOIN directly. You can simulate a FULL JOIN using UNION of LEFT and RIGHT JOIN:

```sql
SELECT authors.name, books.title
FROM authors
LEFT JOIN books ON authors.author_id = books.author_id

UNION

SELECT authors.name, books.title
FROM authors
RIGHT JOIN books ON authors.author_id = books.author_id;
```

---

***Step 7: CROSS JOIN***

Returns every possible pair of authors and books.

```sql
SELECT authors.name, books.title
FROM authors
CROSS JOIN books;
```

This will produce all combinations (3 authors × 3 books = 9 rows).

---

***Want to try some JOIN queries with your own tables?***

Tell me your table structures, or I can create examples for your blog app tables!
