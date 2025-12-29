Here’s a **practical Oracle SQL tutorial you can use to study and practice for hands-on online interviews** — with clear code examples you can run (e.g., in Oracle SQL*Plus, SQL Developer, or Oracle Dev Gym) and explanations to help you *explain your thought process* in interviews.

---

## 📌 1. **Basic Concepts & Setup**

Oracle SQL is the dialect of SQL used with Oracle Database. You’ll be writing queries to *create tables*, *insert data*, *select data*, *update data*, and more. You can practice interactively on platforms like **Oracle Dev Gym** (free quizzes and code) ([devgym.oracle.com][1]).

---

## 📌 2. **Creating Tables (DDL)**

```sql
-- Create a simple table for employees
CREATE TABLE employees (
    emp_id      NUMBER PRIMARY KEY,
    first_name  VARCHAR2(50),
    last_name   VARCHAR2(50),
    age         NUMBER,
    department  VARCHAR2(50)
);

-- Insert sample data
INSERT INTO employees VALUES (1, 'Alice', 'Wong', 30, 'Finance');
INSERT INTO employees VALUES (2, 'Bob', 'Smith', 28, 'Human Resources');
INSERT INTO employees VALUES (3, 'Charlie', 'Gupta', 35, 'IT');
INSERT INTO employees VALUES (4, 'Diana', 'Lee', 40, 'IT');

COMMIT;
```

📌 *Interview Tip:* Know the difference between `CREATE TABLE`, `ALTER TABLE`, and `DROP TABLE`.
*DDL = Data Definition Language*.

---

## 📌 3. **Basic SELECT Queries**

```sql
-- Select all columns and rows
SELECT * FROM employees;

-- Select specific columns
SELECT first_name, last_name FROM employees;

-- Filter rows with WHERE
SELECT * FROM employees
WHERE department = 'IT';

-- Sort results
SELECT * FROM employees
ORDER BY age DESC;
```

🔹 `SELECT` is the most fundamental SQL command.
🔹 Use `WHERE` to filter rows and `ORDER BY` to sort.
🔹 Remember interview questions often start at this level. ([DataCamp][2])

---

## 📌 4. **Aggregate Functions & Grouping**

```sql
-- Count number of employees
SELECT COUNT(*) total_employees FROM employees;

-- Average age of employees
SELECT AVG(age) avg_age FROM employees;

-- Count employees per department
SELECT department, COUNT(*) emp_count
FROM employees
GROUP BY department;
```

📌 *Aggregate functions* like `COUNT`, `AVG`, `MAX` are frequently asked topics. ([DataCamp][2])

---

## 📌 5. **Joins (Combining Tables)**

Create another table for departments:

```sql
CREATE TABLE departments (
    dept_id NUMBER PRIMARY KEY,
    dept_name VARCHAR2(50),
    location VARCHAR2(50)
);

INSERT INTO departments VALUES (1, 'Finance', 'Abu Dhabi');
INSERT INTO departments VALUES (2, 'Human Resources', 'Dubai');
INSERT INTO departments VALUES (3, 'IT', 'Sharjah');

COMMIT;
```

Now join:

```sql
-- Inner Join: only matching rows
SELECT e.first_name,
       e.department,
       d.location
FROM employees e
JOIN departments d
ON e.department = d.dept_name;

-- Left Join: all employees even if no department match
SELECT e.first_name,
       e.department,
       d.location
FROM employees e
LEFT JOIN departments d
ON e.department = d.dept_name;
```

🔹 Joins are *core* in most interviews (inner, left, right, full). ([DataCamp][2])

---

## 📌 6. **Subqueries**

```sql
-- Subquery in WHERE
SELECT first_name, last_name
FROM employees
WHERE department = (
    SELECT dept_name
    FROM departments
    WHERE location = 'Sharjah'
);

-- Using IN for multiple
SELECT * FROM employees
WHERE department IN (
    SELECT dept_name FROM departments
    WHERE location IN ('Dubai','Sharjah')
);
```

📌 Subqueries help solve *nested scenarios* often seen in interviews. ([DataCamp][2])

---

## 📌 7. **Updating & Deleting Data**

```sql
-- Update an employee’s department
UPDATE employees
SET department = 'IT'
WHERE emp_id = 2;

-- Delete a record
DELETE FROM employees
WHERE emp_id = 4;

COMMIT;
```

🔹 Interviewers may ask *what changes after a commit*, or *difference between DELETE and TRUNCATE*. ([DataCamp][2])

---

## 📌 8. **Useful Oracle SQL Resources (Free)**

**🧠 Practice & Learn**

* Oracle **Dev Gym** — Live SQL tasks & quizzes (requires account) ([devgym.oracle.com][1])
* **W3Schools SQL Tutorial** — beginner-friendly with examples ([W3Schools][3])
* **SQLZoo** — interactive SQL tutorials (good for practice) ([SQLZoo][4])
* **Great Learning Academy Oracle SQL Course (free)** — structured learner path ([MyGreatLearning][5])
* **InterviewBit SQL questions & cheat sheets** — real interview examples ([InterviewBit][6])
* **SQL practice problems** on CodeChef & w3resource for exercises ([CodeChef][7])

---

## 🧠 Interview-Ready Tips

✅ **Explain your steps** — tell the interviewer what your query does.
✅ **Use real sample data** — like the examples above and adapt them.
✅ **Know key concepts:**

* DDL vs DML vs DCL
* Joins & subqueries
* Aggregations & grouping
* Index basics
* Constraint types (PK, FK, UNIQUE) ([DataCamp][2])

---

If you want, I can also generate **a set of practice questions + answers** specifically tailored for Oracle SQL interviews (easy → hard). Just let me know!

[1]: https://devgym.oracle.com/pls/apex/dg/competition/sql?utm_source=chatgpt.com "Free SQL Quizzes and Questions - the Dev Gym - Oracle"
[2]: https://www.datacamp.com/blog/top-sql-interview-questions-and-answers-for-beginners-and-intermediate-practitioners?utm_source=chatgpt.com "Top 84 SQL Interview Questions and Answers for 2026 - DataCamp"
[3]: https://www.w3schools.com/sql/?utm_source=chatgpt.com "SQL Tutorial - W3Schools"
[4]: https://sqlzoo.net/wiki/SQL_Tutorial?utm_source=chatgpt.com "SQLZoo"
[5]: https://www.mygreatlearning.com/academy/learn-for-free/courses/oracle-sql?utm_source=chatgpt.com "Free Oracle SQL Tutorial & Course With Certificate For Beginners"
[6]: https://www.interviewbit.com/sql-interview-questions/?utm_source=chatgpt.com "SQL Interview Questions CHEAT SHEET (2025) - InterviewBit"
[7]: https://www.codechef.com/practice/sql-case-studies-topic-wise?utm_source=chatgpt.com "SQL Practice Queries - CodeChef"

Great! Here’s a set of **Oracle SQL interview practice questions with answers and code examples** you can use to prepare for hands-on interviews. These include both conceptual questions and practical SQL exercises you can run and explain during an online interview — including answers you can *write and execute in Oracle SQL*. ([DataCamp][1])

---

## ✅ **1. Find Duplicate Records**

**Q:** How do you find duplicate values in an Oracle table?

**Answer & SQL:**

```sql
SELECT emp_name, COUNT(emp_name) AS count_dup
FROM employees
GROUP BY emp_name
HAVING COUNT(emp_name) > 1;
```

✔ This groups employees by name and shows only those with duplicates. ([DG Support][2])

---

## ✅ **2. Get Current Date & Time**

**Q:** How do you find the current date/time in Oracle?

**Answer & SQL:**

```sql
SELECT SYSDATE FROM dual;
```

Oracle uses the special table `DUAL` for these pseudo queries. ([ويكيبيديا][3])

---

## ✅ **3. Difference Between DELETE and TRUNCATE**

**Q:** What’s the difference between `DELETE` and `TRUNCATE`?

**Answer:**

* `DELETE` removes selected rows and **can be rolled back**.
* `TRUNCATE` deletes *all* rows, **can’t be rolled back** and frees storage space. ([DG Support][2])

---

## ✅ **4. Count Rows in a Table**

**Q:** Write a SQL query to count rows.

**SQL:**

```sql
SELECT COUNT(*) total_employees FROM employees;
```

---

## ✅ **5. Find Employees Above Average Salary**

**Q:** List employees whose salary is greater than the average.

**SQL:**

```sql
SELECT emp_name, salary 
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```

✔ Common interview request — evaluates subqueries. ([Testbook][4])

---

## ✅ **6. Sorting & Filtering**

**Q:** Show employees ordered by salary high to low.

**SQL:**

```sql
SELECT emp_name, salary
FROM employees
ORDER BY salary DESC;
```

---

## ✅ **7. Using Aggregate Functions**

**Q:** Get total and average salary.

**SQL:**

```sql
SELECT SUM(salary) total_sal,
       AVG(salary) avg_sal
FROM employees;
```

---

## ✅ **8. Grouping with HAVING Clause**

**Q:** Show departments with more than 5 employees.

**SQL:**

```sql
SELECT department, COUNT(*) emp_count
FROM employees
GROUP BY department
HAVING COUNT(*) > 5;
```

✔ `HAVING` filters after grouping. ([TestGorilla][5])

---

## ✅ **9. Join Two Tables (Inner Join)**

Assume tables `employees` and `departments`:

**SQL:**

```sql
SELECT e.emp_name,
       d.location
FROM employees e
JOIN departments d
ON e.dept_id = d.dept_id;
```

✔ Joins combine related rows from different tables. ([DataCamp][1])

---

## ✅ **10. Find Top N Rows (Using ROWNUM)**

**Q:** Get top 5 highest salaries.

**SQL:**

```sql
SELECT *
FROM (
  SELECT emp_name, salary
  FROM employees
  ORDER BY salary DESC
)
WHERE ROWNUM <= 5;
```

---

## 📌 **11. Practical Coding Challenge (Scenario)**

**Q:** Write Oracle SQL to show employees who earn more than the average *but less than the maximum* salary in their department.

**SQL (using analytic functions):**

```sql
SELECT emp_name, dept_id, salary
FROM (
  SELECT emp_name,
         dept_id,
         salary,
         AVG(salary) OVER (PARTITION BY dept_id) AS dept_avg,
         MAX(salary) OVER (PARTITION BY dept_id) AS dept_max
  FROM employees
)
WHERE salary > dept_avg
  AND salary < dept_max;
```

✔ Uses analytic functions — common in intermediate interviews. ([LinkedIn][6])

---

## 📌 **12. Create Table & Insert (Practical)**

**SQL:**

```sql
CREATE TABLE employees (
  emp_id NUMBER PRIMARY KEY,
  emp_name VARCHAR2(50),
  dept_id NUMBER,
  salary NUMBER
);

INSERT INTO employees VALUES (1, 'Alice', 10, 50000);
INSERT INTO employees VALUES (2, 'Bob', 10, 70000);
INSERT INTO employees VALUES (3, 'Charlie', 20, 90000);

COMMIT;
```

---

## 📌 **13. Interactive Practice Sites**

To *practice live SQL questions* try:

* **Oracle Dev Gym** — real Oracle SQL quizzes and practice problems. ([Dev Gym][7])
* **w3resource Oracle SQL exercises** — 15+ interactive queries with solutions. ([w3resource][8])
* **GeeksforGeeks SQL exercises** — beginner through advanced practice. ([GeeksforGeeks][9])

---

## 🧠 **Interview Strategy Tips**

✔ Always explain your logic *before* writing a query.
✔ Use real sample data and walk through results.
✔ Practice with incremental difficulty — start basic (`SELECT`, `JOIN`) then go to *subqueries*, *analytic functions*, and *performance optimizations*. ([DataCamp][1])

---

If you want, I can generate a **printable PDF cheat sheet** of these Oracle SQL questions & answers tailored to your experience level (beginner, intermediate, advanced). Just tell me your level!

[1]: https://www.datacamp.com/blog/top-sql-interview-questions-and-answers-for-beginners-and-intermediate-practitioners?utm_source=chatgpt.com "Top 84 SQL Interview Questions and Answers for 2026"
[2]: https://support.dbagenesis.com/sql/oracle-sql-interview-questions?utm_source=chatgpt.com "Oracle SQL Interview Questions"
[3]: https://en.wikipedia.org/wiki/DUAL_table?utm_source=chatgpt.com "DUAL table"
[4]: https://testbook.com/interview/oracle-interview-questions?utm_source=chatgpt.com "50+ Oracle Interview Questions: Freshers & Experienced ..."
[5]: https://www.testgorilla.com/blog/tricky-oracle-sql-interview-questions/?utm_source=chatgpt.com "30 Tricky Oracle SQL Interview Questions and Answers"
[6]: https://www.linkedin.com/posts/saxonchris_69-sql-interview-questions-and-answers-including-activity-7263159036802478080-SJJt?utm_source=chatgpt.com "69 SQL Interview Questions and Answers (Including ..."
[7]: https://devgym.oracle.com/pls/apex/dg/competition/sql?utm_source=chatgpt.com "Free SQL Quizzes and Questions"
[8]: https://www.w3resource.com/oracle-exercises/basic/?utm_source=chatgpt.com "Oracle SQL Queries: Basic Exercises, Practice, Solution"
[9]: https://www.geeksforgeeks.org/sql/sql-exercises/?utm_source=chatgpt.com "SQL Exercises : SQL Practice with Solution for Beginners ..."

Great — here’s a **challenging set of Oracle SQL practice questions focused specifically on advanced *JOIN* and *GROUP BY* logic**, with complete answers and code you can run in an interview or practice environment. These go beyond basic syntax and are designed to test real analytical and query-building skills. ([Verve Copilot][1])

---

## 🧠 **Hard Oracle SQL Join + Grouping Practice Questions**

---

### **1) Total Sales and Number of Orders by Joined Region and Product**

**Scenario:**
You have two Oracle tables:

```sql
CREATE TABLE sales (
  sale_id     NUMBER,
  customer_id NUMBER,
  product_id  NUMBER,
  amount      NUMBER,
  sale_date   DATE
);

CREATE TABLE products (
  product_id   NUMBER PRIMARY KEY,
  product_name VARCHAR2(100),
  category     VARCHAR2(50)
);

CREATE TABLE customers (
  customer_id NUMBER PRIMARY KEY,
  city        VARCHAR2(50),
  region      VARCHAR2(50)
);
```

**Q:** Write a query to find the **total sales amount** and **total number of orders** for each **region** *and* **product category**.

👉 **Answer:**

```sql
SELECT 
    c.region,
    p.category,
    COUNT(s.sale_id) AS total_orders,
    SUM(s.amount)  AS total_sales
FROM sales s
JOIN customers c
    ON s.customer_id = c.customer_id
JOIN products p
    ON s.product_id = p.product_id
GROUP BY
    c.region,
    p.category
ORDER BY
    c.region,
    p.category;
```

✔ This uses multiple joins and a grouped aggregated result by two dimensions (region & category). ([Talentuner][2])

---

### **2) Find Customers Who Bought Above Average for Their Region**

**Q:** Find all customers whose **total purchases** (sum of `amount`) are **above the average for their region** — calculated *per region*. Show customer ID, region, total spent.

👉 **Answer:**

```sql
SELECT customer_id, region, total_spent
FROM (
    SELECT 
        c.customer_id,
        c.region,
        SUM(s.amount) AS total_spent,
        AVG(SUM(s.amount)) OVER (PARTITION BY c.region) AS region_avg
    FROM sales s
    JOIN customers c
        ON s.customer_id = c.customer_id
    GROUP BY c.customer_id, c.region
)
WHERE total_spent > region_avg;
```

✔ This uses **GROUP BY + analytic window** to compute region averages *without an extra join back*. ([credmark][3])

---

### **3) Categories with No Sales in a Given Quarter**

**Q:** Which product **categories have no sales** in Q4 (Oct–Dec 2025)? Return category names.

👉 **Answer:**

```sql
SELECT category
FROM products
WHERE category NOT IN (
    SELECT DISTINCT p.category
    FROM sales s
    JOIN products p
        ON s.product_id = p.product_id
    WHERE s.sale_date BETWEEN DATE '2025-10-01' AND DATE '2025-12-31'
);
```

✔ Tests anti-join logic via `NOT IN` and filtering on join results. ([Verve Copilot][1])

---

### **4) Highest Selling Product per Region**

**Q:** For each region, find the **product with the highest total sales**.

👉 **Answer (using analytic functions):**

```sql
WITH regional_sales AS (
    SELECT
        c.region,
        s.product_id,
        SUM(s.amount) AS total_by_prod
    FROM sales s
    JOIN customers c
        ON s.customer_id = c.customer_id
    GROUP BY c.region, s.product_id
)
SELECT region, product_id, total_by_prod
FROM (
    SELECT
        region,
        product_id,
        total_by_prod,
        RANK() OVER (PARTITION BY region ORDER BY total_by_prod DESC) AS rk
    FROM regional_sales
)
WHERE rk = 1;
```

✔ Uses join + aggregation + ranking to pick *top product per region*. ([SQL Practice Online][4])

---

### **5) Count of Customers by Region with Average Spend Over Global Median**

**Q:** Return the **count of customers** in each region whose **average spending** is **above the global median** average spending.

👉 **Answer:**

```sql
WITH cust_avg AS (
    SELECT
        c.region,
        s.customer_id,
        AVG(s.amount) AS avg_spend
    FROM sales s
    JOIN customers c
        ON s.customer_id = c.customer_id
    GROUP BY c.region, s.customer_id
),
median_spend AS (
    SELECT PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY avg_spend) AS med
    FROM cust_avg
)
SELECT
    region,
    COUNT(*) AS cnt_customers
FROM cust_avg, median_spend
WHERE avg_spend > med
GROUP BY region;
```

✔ Exercises **advanced grouping**, aggregation, analytic functions & median. ([Oracle Docs][5])

---

### **6) Multi-Table Join with Group Filtering**

**Q:** Report the **regions that had total sales exceeding the total returns** (assume returns table below), for 2025.

```sql
CREATE TABLE returns (
    return_id   NUMBER,
    sale_id     NUMBER,
    return_date DATE,
    amount      NUMBER
);
```

👉 **Answer:**

```sql
SELECT 
    c.region
FROM (
    SELECT 
        c.region,
        SUM(s.amount) total_sales,
        NVL(SUM(r.amount),0) total_returns
    FROM sales s
    JOIN customers c
        ON s.customer_id = c.customer_id
    LEFT JOIN returns r
        ON s.sale_id = r.sale_id
           AND r.return_date BETWEEN DATE '2025-01-01' AND DATE '2025-12-31'
    WHERE s.sale_date BETWEEN DATE '2025-01-01' AND DATE '2025-12-31'
    GROUP BY c.region
) t
WHERE total_sales > total_returns;
```

✔ Tests join with filters on both sides and conditional join logic. ([Verve Copilot][1])

---

## 🧠 **Key Concepts Tested in These Questions**

✔ Multi-table joins (INNER & LEFT) and conditions on join key matching. ([Talentuner][2])
✔ Aggregation with `GROUP BY`, including multiple grouping dimensions. ([Verve Copilot][6])
✔ Analytic functions (e.g., `RANK()`, `AVG() OVER`, etc.) for per-group ranking and comparisons. ([SQL Practice Online][4])
✔ Filtering aggregated results with subqueries/anti-joins (`NOT IN`). ([Verve Copilot][1])
✔ Combining joins + grouping + advanced calculations (median, top-N). ([Oracle Docs][5])

---

## 🧩 Extra Challenge (Optional)

**Q:** Write a single query to *simulate a pivot* showing total sales per category for each region.
*(Hint: use `CASE` in combination with `SUM()` + `GROUP BY`)*

You can try this on your own and I can provide the solution too!

---

Would you like a **runnable SQL script** with sample data and expected outputs for all these questions? (Great for hands-on practice.)

[1]: https://www.vervecopilot.com/interview-questions/top-30-most-common-sql-joins-interview-questions-you-should-prepare-for?utm_source=chatgpt.com "Top 30 Most Common sql joins interview questions You Should Prepare For"
[2]: https://talentuner.com/blog/sql-interview-questions?utm_source=chatgpt.com "SQL Interview Questions Guide"
[3]: https://credmark.ai/practice/top-sql-analytics-interview-questions-and-answers?utm_source=chatgpt.com "SQL Analytics interview questions and answers - credmark"
[4]: https://www.sql-practice.online/interview-blog/advanced-sql-interview-questions?utm_source=chatgpt.com "Advanced SQL Interview Questions: Senior Developer Level Challenges | SQL Interview Blog | SQL Practice Online"
[5]: https://docs.oracle.com/en/database/oracle/oracle-database/26/dwhsg/advanced-analytical-sql-data-warehouses.html?utm_source=chatgpt.com "Advanced Analytical SQL"
[6]: https://www.vervecopilot.com/interview-questions/top-30-most-common-advanced-sql-interview-questions-you-should-prepare-for?utm_source=chatgpt.com "Top 30 Most Common Advanced Sql Interview Questions You Should Prepare For"

Here are the **main types of Oracle SQL joins** with clear code examples you can use for **practice or interview prep** — along with explanations of what each join does. Oracle supports ANSI join syntax like most modern SQL databases. ([Oracle Docs][1])

---

## 📌 1. **INNER JOIN**

Returns only rows where the join condition matches in **both tables**. ([Oracle Docs][2])

```sql
SELECT
    e.employee_id,
    e.first_name,
    d.department_name
FROM
    employees e
INNER JOIN
    departments d
ON
    e.department_id = d.department_id;
```

✅ Only employees **with a matching department** show up.

---

## 📌 2. **LEFT OUTER JOIN** (or **LEFT JOIN**)

Returns **all rows from the left table**, even if there’s no match in the right table; unmatched columns from the right will be `NULL`. ([موقع PHP][3])

```sql
SELECT
    e.employee_id,
    e.first_name,
    d.department_name
FROM
    employees e
LEFT JOIN
    departments d
ON
    e.department_id = d.department_id;
```

✅ Shows all employees — including those **without a department**.

---

## 📌 3. **RIGHT OUTER JOIN** (or **RIGHT JOIN**)

Returns **all rows from the right table**, even if there’s no match in the left; unmatched left columns are `NULL`. ([موقع PHP][3])

```sql
SELECT
    e.employee_id,
    e.first_name,
    d.department_name
FROM
    employees e
RIGHT JOIN
    departments d
ON
    e.department_id = d.department_id;
```

✅ Shows all departments — even those with **no employees**.

---

## 📌 4. **FULL OUTER JOIN** (or **FULL JOIN**)

Returns **all rows from both tables**, matching where possible — and where there’s no match in either table, the missing fields are `NULL`. ([موقع PHP][3])

```sql
SELECT
    e.employee_id,
    e.first_name,
    d.department_name
FROM
    employees e
FULL OUTER JOIN
    departments d
ON
    e.department_id = d.department_id;
```

✅ Combines the results of both LEFT and RIGHT joins.

---

## 📌 5. **CROSS JOIN**

Produces a **Cartesian product** — every row from the first table combined with every row in the second table. This can produce a lot of rows! ([Oracle Docs][1])

```sql
SELECT
    e.employee_id,
    e.first_name,
    d.department_name
FROM
    employees e
CROSS JOIN
    departments d;
```

✅ Useful when you need *all possible combinations* of rows (e.g., all product–store pairs).

---

## 📌 6. **SELF JOIN**

A **self join** is not its own type but is a use of a normal join to relate a table to itself. It often uses aliases. ([موقع PHP][3])

**Example:** Find employees and their managers (in the same table):

```sql
SELECT
    e.employee_id AS emp,
    e.first_name AS employee,
    m.first_name AS manager
FROM
    employees e
LEFT JOIN
    employees m
ON
    e.manager_id = m.employee_id;
```

---

## 🧠 Notes

* **`JOIN` alone equals `INNER JOIN`**. ([Oracle Docs][1])
* The use of `OUTER` in `LEFT OUTER JOIN` / `RIGHT OUTER JOIN` is optional (ANSI standard). ([موقع PHP][3])
* Oracle also historically had a legacy outer join operator `+` — but ANSI joins (`LEFT JOIN`, etc.) are preferred today. ([relationaldbdesign.com][4])

---

If you want, I can provide these examples with **sample table data and expected outputs** so you can run and see results immediately. Just let me know!

[1]: https://docs.oracle.com/cd/E29805_01/server.230/es_eql/src/ceql_join.html?utm_source=chatgpt.com "JOIN clauses"
[2]: https://docs.oracle.com/database/121/TGSQL/tgsql_join.htm?utm_source=chatgpt.com "Joins"
[3]: https://m.php.cn/en/faq/1796852363.html?utm_source=chatgpt.com "What are the various types of joins (e.g., INNER, OUTER, CROSS, SELF) available in Oracle SQL?-Oracle-php.cn"
[4]: https://www.relationaldbdesign.com/oracle-sql/module5/oracle-outer-join-syntax.php?utm_source=chatgpt.com "Oracle Outer Join Syntax (+) Interpretation Guide"
