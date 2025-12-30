Here’s a **practical interview practice guide for Oracle PL/SQL**, complete with *hands-on coding questions*, *answers*, and *code examples* you can run in your Oracle database. These combine typical interview theory with real PL/SQL blocks you might be asked to write. ([GeeksforGeeks][1])

---

## 📌 1. Basic PL/SQL Block

**Q1. Write and execute a simple PL/SQL block that prints “Hello PL/SQL”.**

✅ **Answer & Code**

```sql
BEGIN
   DBMS_OUTPUT.PUT_LINE('Hello PL/SQL');
END;
/
```

> *DBMS_OUTPUT.PUT_LINE prints output to the console (SQL*Plus, SQL Developer, etc.).*

---

## 📌 2. Variables and Conditional Logic

**Q2. Declare variables and use an `IF` statement to check a number.**

**Task**: If a number is even, print “Even”; else print “Odd”.

```sql
DECLARE
   v_num NUMBER := 27;
BEGIN
   IF MOD(v_num, 2) = 0 THEN
      DBMS_OUTPUT.PUT_LINE('Even');
   ELSE
      DBMS_OUTPUT.PUT_LINE('Odd');
   END IF;
END;
/
```

---

## 📌 3. Cursor Example (Row-by-Row Processing)

**Q3. Use a cursor to fetch and display employee names from a table.**

```sql
DECLARE
   CURSOR emp_cur IS SELECT first_name FROM employees;
   v_name employees.first_name%TYPE;
BEGIN
   OPEN emp_cur;
   LOOP
      FETCH emp_cur INTO v_name;
      EXIT WHEN emp_cur%NOTFOUND;
      DBMS_OUTPUT.PUT_LINE('Employee: ' || v_name);
   END LOOP;
   CLOSE emp_cur;
END;
/
```

**Explanation**:
This example uses an explicit cursor to fetch each employee. Interviewers often ask about `%FOUND`, `%NOTFOUND`, and `%ROWCOUNT`. ([w3resource][2])

---

## 📌 4. Loop Without Explicit Cursor

**Q4. Use a `FOR` cursor loop to simplify fetching.**

```sql
BEGIN
   FOR rec IN (SELECT employee_id, salary FROM employees WHERE salary > 5000) 
   LOOP
      DBMS_OUTPUT.PUT_LINE('ID: ' || rec.employee_id || ', Salary: ' || rec.salary);
   END LOOP;
END;
/
```

No OPEN/FETCH/CLOSE required — PL/SQL handles it. ([w3resource][2])

---

## 📌 5. Stored Procedure

**Q5. Write a stored procedure to increase salary by percentage.**

```sql
CREATE OR REPLACE PROCEDURE raise_salary (
   p_dept_id  IN  NUMBER,
   p_pct      IN  NUMBER
)
IS
BEGIN
   UPDATE employees
   SET salary = salary + (salary * p_pct / 100)
   WHERE department_id = p_dept_id;
   COMMIT;
END;
/
```

**Usage**

```sql
BEGIN
   raise_salary(50, 10);
END;
/
```

---

## 📌 6. Function

**Q6. Create a function that returns the total salary for a department.**

```sql
CREATE OR REPLACE FUNCTION get_dept_salary(p_dept_id IN NUMBER)
   RETURN NUMBER
IS
   v_total NUMBER(10,2);
BEGIN
   SELECT SUM(salary) INTO v_total
   FROM employees
   WHERE department_id = p_dept_id;
   RETURN v_total;
END;
/
```

**Call It**

```sql
BEGIN
   DBMS_OUTPUT.PUT_LINE('Total Salary: ' || get_dept_salary(30));
END;
/
```

---

## 📌 7. Exception Handling

**Q7. Demonstrate exception handling in a block.**

```sql
DECLARE
   v_emp_id NUMBER := 99999;
   v_name   VARCHAR2(50);
BEGIN
   SELECT first_name
   INTO v_name
   FROM employees
   WHERE employee_id = v_emp_id;

   DBMS_OUTPUT.PUT_LINE('Name: ' || v_name);

EXCEPTION
   WHEN NO_DATA_FOUND THEN
      DBMS_OUTPUT.PUT_LINE('No employee found for ID ' || v_emp_id);
END;
/
```

---

## 📌 8. Trigger Example

**Q8. Create a trigger to log salary changes.**

```sql
CREATE OR REPLACE TRIGGER trg_salary_audit
AFTER UPDATE OF salary ON employees
FOR EACH ROW
BEGIN
   INSERT INTO salary_log(emp_id, old_sal, new_sal, change_dt)
   VALUES(:OLD.employee_id, :OLD.salary, :NEW.salary, SYSDATE);
END;
/
```

**Explanation**:
Triggers fire automatically on DML — a common interview topic. ([GeeksforGeeks][1])

---

## 📌 9. %TYPE and %ROWTYPE

**Q9. Use `%TYPE` to declare types based on table columns.**

```sql
DECLARE
   v_emp_name employees.first_name%TYPE;
   v_emp_rec  employees%ROWTYPE;
BEGIN
   SELECT first_name, salary
   INTO v_emp_name, v_emp_rec.salary
   FROM employees
   WHERE employee_id = 100;

   DBMS_OUTPUT.PUT_LINE('Name: ' || v_emp_name);
END;
/
```

---

## 📌 10. Bulk Collect and FORALL (Performance)

**Q10. Bulk processing for performance.**

```sql
DECLARE
   TYPE num_tab IS TABLE OF employees.employee_id%TYPE;
   emp_ids   num_tab;

BEGIN
   SELECT employee_id
   BULK COLLECT INTO emp_ids
   FROM employees WHERE salary > 5000;

   FORALL i IN emp_ids.FIRST .. emp_ids.LAST
      UPDATE employees
      SET salary = salary * 1.10
      WHERE employee_id = emp_ids(i);

   COMMIT;
END;
/
```

**Use Case**: Efficient batch updates instead of row-by-row. ([w3resource][2])

---

## 🧠 Common PL/SQL Topics Interviewers Ask

You should be ready to explain and write code for:
✔ Basic PL/SQL block structure (DECLARE/BEGIN/EXCEPTION/END)
✔ Cursors (explicit vs implicit)
✔ Procedures & Functions
✔ Triggers
✔ Exception handling
✔ %TYPE / %ROWTYPE
✔ Transaction control (COMMIT/ROLLBACK)
✔ Bulk operations (BULK COLLECT / FORALL) ([GeeksforGeeks][1])

---

## 📦 Final Practice Tips

🔹 Practice by creating and running code in Oracle SQL Developer or Oracle Live SQL.
🔹 Try variations like *write a procedure with OUT parameters* or *trigger with conditional logic*.
🔹 Review cursor attributes (`%FOUND`, `%NOTFOUND`, `%ROWCOUNT`). ([w3resource][2])

---

If you want a **downloadable set of practice exercises or answers in PDF format**, let me know and I can generate one!

[1]: https://www.geeksforgeeks.org/sql/pl-sql-interview-questions/?utm_source=chatgpt.com "PL/SQL Interview Questions and Answers"
[2]: https://www.w3resource.com/plsql-exercises/cursor/index.php?utm_source=chatgpt.com "PL/SQL Cursor Exercises with Solution"
