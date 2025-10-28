
# 🧾 Task 6: Subqueries and Nested Queries

## 🎯 Objective
Learn how to use **subqueries (nested queries)** inside `SELECT`, `WHERE`, and `FROM` clauses to perform advanced data retrieval operations.

---

## 🧰 Tools
- **DB Browser for SQLite**  
- **MySQL Workbench**

---

## 📚 Learning Goals
By completing this task, you will:
- Understand **scalar**, **correlated**, and **nested** subqueries.  
- Use subqueries effectively inside:
  - `SELECT` clauses  
  - `WHERE` filters  
  - `FROM` derived tables  
- Apply logical operators like `IN`, `EXISTS`, and `=` with subqueries.

---

## 💡 Concepts Overview

### 🔸 1. Scalar Subquery
A subquery that returns **a single value** (one row, one column).

Example use:
```sql
SELECT name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
````

👉 Returns employees whose salary is greater than the **average salary**.

---

### 🔸 2. Correlated Subquery

A subquery that **depends on the outer query** — it runs once for each row of the outer query.

Example:

```sql
SELECT e1.name, e1.salary
FROM employees e1
WHERE e1.salary > (
  SELECT AVG(e2.salary)
  FROM employees e2
  WHERE e2.department_id = e1.department_id
);
```

👉 Finds employees earning more than the **average salary in their own department**.

---

### 🔸 3. Subquery in FROM Clause (Derived Table)

You can use a subquery as a temporary table.

Example:

```sql
SELECT department_id, avg_salary
FROM (
  SELECT department_id, AVG(salary) AS avg_salary
  FROM employees
  GROUP BY department_id
) AS dept_avg
WHERE avg_salary > 50000;
```

👉 First, the inner query calculates average salaries per department,
then the outer query filters departments with avg salary > 50,000.

---

### 🔸 4. Using IN / EXISTS with Subqueries

#### Using `IN`:

```sql
SELECT name
FROM employees
WHERE department_id IN (
  SELECT id
  FROM departments
  WHERE location = 'New York'
);
```

#### Using `EXISTS`:

```sql
SELECT d.name
FROM departments d
WHERE EXISTS (
  SELECT 1
  FROM employees e
  WHERE e.department_id = d.id
);
```

👉 `EXISTS` checks whether the subquery returns any rows.

---

## 🧩 Example Scenario

**Tables:**

```sql
CREATE TABLE employees (
  id INTEGER PRIMARY KEY,
  name TEXT,
  department_id INTEGER,
  salary INTEGER
);

CREATE TABLE departments (
  id INTEGER PRIMARY KEY,
  name TEXT,
  location TEXT
);
```

**Sample Data:**

```sql
INSERT INTO departments VALUES
(1, 'Engineering', 'New York'),
(2, 'HR', 'Boston'),
(3, 'Marketing', 'Chicago');

INSERT INTO employees VALUES
(101, 'Alice', 1, 75000),
(102, 'Bob', 1, 50000),
(103, 'Charlie', 2, 40000),
(104, 'Diana', 3, 60000),
(105, 'Evan', 3, 65000);
```

---

## 💻 Example Queries

### 1️⃣ Employees earning above company average

```sql
SELECT name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```

---

### 2️⃣ Departments with average salary above 60,000

```sql
SELECT name
FROM departments
WHERE id IN (
  SELECT department_id
  FROM employees
  GROUP BY department_id
  HAVING AVG(salary) > 60000
);
```

---

### 3️⃣ Employees who earn more than average in their department

```sql
SELECT e1.name, e1.salary
FROM employees e1
WHERE e1.salary > (
  SELECT AVG(e2.salary)
  FROM employees e2
  WHERE e2.department_id = e1.department_id
);
```

---

### 4️⃣ Show each department with its average salary (FROM subquery)

```sql
SELECT department_id, avg_salary
FROM (
  SELECT department_id, AVG(salary) AS avg_salary
  FROM employees
  GROUP BY department_id
) AS dept_avg
ORDER BY avg_salary DESC;
```

---

### 5️⃣ Departments having employees located in New York

```sql
SELECT d.name
FROM departments d
WHERE EXISTS (
  SELECT 1
  FROM employees e
  WHERE e.department_id = d.id
);
```

---

## 🧠 Outcome

By completing this task, you will gain:

* Strong understanding of **subquery logic**.
* Ability to write **complex queries** combining multiple layers of logic.
* Confidence in using `IN`, `EXISTS`, and correlated subqueries.

---

**Author:** Gadieen Khemkaar
**Topic:** SQL — Subqueries & Nested Queries
**Difficulty:** Intermediate → Advanced
**Goal:** Build strong query composition and analytical reasoning in SQL.

