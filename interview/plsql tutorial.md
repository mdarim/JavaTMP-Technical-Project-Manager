# PL/SQL Tutorials

## PL/SQL - Basic Syntax

```sql
DECLARE 
   <declarations section> 
BEGIN 
   <executable command(s)>
EXCEPTION 
   <exception handling> 
END;
```

```plsql
DECLARE
    -- variable declaration 
    message  varchar2(20):= 'Hello, World!'; 
BEGIN 
    /* 
    *  PL/SQL executable statement(s) 
    */ 
    dbms_output.put_line(message); 
END; 
/ 
```

## PL/SQL Program Units

A PL/SQL unit is any one of the following −

* PL/SQL block
* Function
* Package
* Package body
* Procedure
* Trigger
* Type
* Type body


```
DECLARE 
   num1 INTEGER; 
   num2 REAL; 
   num3 DOUBLE PRECISION; 
BEGIN 
   null; 
END; 
/ 
```


```
DECLARE 
   a integer := 10; 
   b integer := 20; 
   c integer; 
   f real; 
BEGIN 
   c := a + b; 
   dbms_output.put_line('Value of c: ' || c); 
   f := 70.0/3.0; 
   dbms_output.put_line('Value of f: ' || f); 
END; 
/  
```

```
DECLARE 
   -- Global variables  
   num1 number := 95;  
   num2 number := 85;  
BEGIN  
   dbms_output.put_line('Outer Variable num1: ' || num1); 
   dbms_output.put_line('Outer Variable num2: ' || num2); 
   DECLARE  
      -- Local variables 
      num1 number := 195;  
      num2 number := 185;  
   BEGIN  
      dbms_output.put_line('Inner Variable num1: ' || num1); 
      dbms_output.put_line('Inner Variable num2: ' || num2); 
   END;  
END; 
/ 
```

```
DECLARE 
   c_id customers.id%type := 1; 
   c_name  customers.name%type; 
   c_addr customers.address%type; 
   c_sal  customers.salary%type; 
BEGIN 
   SELECT name, address, salary INTO c_name, c_addr, c_sal 
   FROM customers 
   WHERE id = c_id;  
   dbms_output.put_line 
   ('Customer ' ||c_name || ' from ' || c_addr || ' earns ' || c_sal); 
END; 
/  
```

```
DECLARE 
   message  varchar2(30):= 'That''s tutorialspoint.com!'; 
BEGIN 
   dbms_output.put_line(message); 
END; 
/  
```


```
DECLARE 
   a number(2) := 10; 
BEGIN 
   a:= 10; 
  -- check the boolean condition using if statement  
   IF( a < 20 ) THEN 
      -- if condition is true then print the following   
      dbms_output.put_line('a is less than 20 ' ); 
   END IF; 
   dbms_output.put_line('value of a is : ' || a); 
END; 
/
```

```
DECLARE 
   a number(3) := 100; 
BEGIN 
   -- check the boolean condition using if statement  
   IF( a < 20 ) THEN 
      -- if condition is true then print the following   
      dbms_output.put_line('a is less than 20 ' ); 
   ELSE 
      dbms_output.put_line('a is not less than 20 ' ); 
   END IF; 
   dbms_output.put_line('value of a is : ' || a); 
END; 
/
```

```
DECLARE 
   grade char(1) := 'A'; 
BEGIN 
   CASE grade 
      when 'A' then dbms_output.put_line('Excellent'); 
      when 'B' then dbms_output.put_line('Very good'); 
      when 'C' then dbms_output.put_line('Well done'); 
      when 'D' then dbms_output.put_line('You passed'); 
      when 'F' then dbms_output.put_line('Better try again'); 
      else dbms_output.put_line('No such grade'); 
   END CASE; 
END; 
/
```

```
DECLARE 
   a number(3) := 100; 
   b number(3) := 200; 
BEGIN 
   -- check the boolean condition  
   IF( a = 100 ) THEN 
   -- if condition is true then check the following  
      IF( b = 200 ) THEN 
      -- if condition is true then print the following  
      dbms_output.put_line('Value of a is 100 and b is 200' ); 
      END IF; 
   END IF; 
   dbms_output.put_line('Exact value of a is : ' || a ); 
   dbms_output.put_line('Exact value of b is : ' || b ); 
END; 
/ 
```

```
DECLARE 
   x number := 10; 
BEGIN 
   LOOP 
      dbms_output.put_line(x); 
      x := x + 10; 
      IF x > 50 THEN 
         exit; 
      END IF; 
   END LOOP; 
   -- after exit, control resumes here  
   dbms_output.put_line('After Exit x is: ' || x); 
END; 
/
```

```
DECLARE 
   a number(2) := 10; 
BEGIN 
   WHILE a < 20 LOOP 
      dbms_output.put_line('value of a: ' || a); 
      a := a + 1; 
   END LOOP; 
END; 
/ 
```

```
DECLARE 
   a number(2); 
BEGIN 
   FOR a in 10 .. 20 LOOP 
      dbms_output.put_line('value of a: ' || a); 
  END LOOP; 
END; 
/
```

```
DECLARE 
   i number(3); 
   j number(3); 
BEGIN 
   i := 2; 
   LOOP 
      j:= 2; 
      LOOP 
         exit WHEN ((mod(i, j) = 0) or (j = i)); 
         j := j +1; 
      END LOOP; 
   IF (j = i ) THEN 
      dbms_output.put_line(i || ' is prime'); 
   END IF; 
   i := i + 1; 
   exit WHEN i = 50; 
   END LOOP; 
END; 
/
```

```
DECLARE 
   i number(1); 
   j number(1); 
BEGIN 
   << outer_loop >> 
   FOR i IN 1..3 LOOP 
      << inner_loop >> 
      FOR j IN 1..3 LOOP 
         dbms_output.put_line('i is: '|| i || ' and j is: ' || j); 
      END loop inner_loop; 
   END loop outer_loop; 
END; 
/
```


```
DECLARE 
   a number(2) := 10; 
BEGIN 
   -- while loop execution  
   WHILE a < 20 LOOP 
      dbms_output.put_line ('value of a: ' || a); 
      a := a + 1; 
      IF a > 15 THEN 
         -- terminate the loop using the exit statement 
         EXIT; 
      END IF; 
   END LOOP; 
END; 
/ 
```


```
DECLARE 
   a number(2) := 10; 
BEGIN 
   -- while loop execution  
   WHILE a < 20 LOOP 
      dbms_output.put_line ('value of a: ' || a); 
      a := a + 1; 
      IF a = 15 THEN 
         -- skip the loop using the CONTINUE statement 
         a := a + 1; 
         CONTINUE; 
      END IF; 
   END LOOP; 
END; 
/ 
```


```
DECLARE 
   a number(2) := 10; 
BEGIN 
   <<loopstart>> 
   -- while loop execution  
   WHILE a < 20 LOOP
   dbms_output.put_line ('value of a: ' || a); 
      a := a + 1; 
      IF a = 15 THEN 
         a := a + 1; 
         GOTO loopstart; 
      END IF; 
   END LOOP; 
END; 
/
```

```
DECLARE 
   name varchar2(20); 
   company varchar2(30); 
   introduction clob; 
   choice char(1); 
BEGIN 
   name := 'John Smith'; 
   company := 'Infotech'; 
   introduction := ' Hello! I''m John Smith from Infotech.'; 
   choice := 'y'; 
   IF choice = 'y' THEN 
      dbms_output.put_line(name); 
      dbms_output.put_line(company); 
      dbms_output.put_line(introduction); 
   END IF; 
END; 
/
```

```
DECLARE 
   greetings varchar2(11) := 'hello world'; 
BEGIN 
   dbms_output.put_line(UPPER(greetings)); 
    
   dbms_output.put_line(LOWER(greetings)); 
    
   dbms_output.put_line(INITCAP(greetings)); 
    
   /* retrieve the first character in the string */ 
   dbms_output.put_line ( SUBSTR (greetings, 1, 1)); 
    
   /* retrieve the last character in the string */ 
   dbms_output.put_line ( SUBSTR (greetings, -1, 1)); 
    
   /* retrieve five characters,  
      starting from the seventh position. */ 
   dbms_output.put_line ( SUBSTR (greetings, 7, 5)); 
    
   /* retrieve the remainder of the string, 
      starting from the second position. */ 
   dbms_output.put_line ( SUBSTR (greetings, 2)); 
     
   /* find the location of the first "e" */ 
   dbms_output.put_line ( INSTR (greetings, 'e')); 
END; 
/ 
```

```
DECLARE 
   greetings varchar2(30) := '......Hello World.....'; 
BEGIN 
   dbms_output.put_line(RTRIM(greetings,'.')); 
   dbms_output.put_line(LTRIM(greetings, '.')); 
   dbms_output.put_line(TRIM( '.' from greetings)); 
END; 
/
```

```
CREATE [OR REPLACE] PROCEDURE procedure_name 
[(parameter_name [IN | OUT | IN OUT] type [, ...])] 
{IS | AS} 
BEGIN 
  < procedure_body > 
END procedure_name; 
```

```
CREATE OR REPLACE PROCEDURE greetings 
AS 
BEGIN 
   dbms_output.put_line('Hello World!'); 
END; 
/
```

```
EXECUTE greetings;
```

```
BEGIN 
   greetings; 
END; 
/
```

```
DROP PROCEDURE greetings; 
```

```
DECLARE 
   a number; 
   b number; 
   c number;
PROCEDURE findMin(x IN number, y IN number, z OUT number) IS 
BEGIN 
   IF x < y THEN 
      z:= x; 
   ELSE 
      z:= y; 
   END IF; 
END;   
BEGIN 
   a:= 23; 
   b:= 45; 
   findMin(a, b, c); 
   dbms_output.put_line(' Minimum of (23, 45) : ' || c); 
END; 
/
```

```
CREATE [OR REPLACE] FUNCTION function_name 
[(parameter_name [IN | OUT | IN OUT] type [, ...])] 
RETURN return_datatype 
{IS | AS} 
BEGIN 
   < function_body > 
END [function_name];
```

```
CREATE OR REPLACE FUNCTION totalCustomers 
RETURN number IS 
   total number(2) := 0; 
BEGIN 
   SELECT count(*) into total 
   FROM customers; 
    
   RETURN total; 
END; 
/ 
```

```
DECLARE 
   c number(2); 
BEGIN 
   c := totalCustomers(); 
   dbms_output.put_line('Total no. of Customers: ' || c); 
END; 
/
```

```
DECLARE 
   c_id customers.id%type; 
   c_name customers.name%type; 
   c_addr customers.address%type; 
   CURSOR c_customers is 
      SELECT id, name, address FROM customers; 
BEGIN 
   OPEN c_customers; 
   LOOP 
   FETCH c_customers into c_id, c_name, c_addr; 
      EXIT WHEN c_customers%notfound; 
      dbms_output.put_line(c_id || ' ' || c_name || ' ' || c_addr); 
   END LOOP; 
   CLOSE c_customers; 
END; 
/
```

```
DECLARE 
   customer_rec customers%rowtype; 
BEGIN 
   SELECT * into customer_rec 
   FROM customers 
   WHERE id = 5;  
   dbms_output.put_line('Customer ID: ' || customer_rec.id); 
   dbms_output.put_line('Customer Name: ' || customer_rec.name); 
   dbms_output.put_line('Customer Address: ' || customer_rec.address); 
   dbms_output.put_line('Customer Salary: ' || customer_rec.salary); 
END; 
/
```

```
DECLARE 
   CURSOR customer_cur is 
      SELECT id, name, address  
      FROM customers; 
   customer_rec customer_cur%rowtype; 
BEGIN 
   OPEN customer_cur; 
   LOOP 
      FETCH customer_cur into customer_rec; 
      EXIT WHEN customer_cur%notfound; 
      DBMS_OUTPUT.put_line(customer_rec.id || ' ' || customer_rec.name); 
   END LOOP; 
END; 
/
```

```
DECLARE 
   type books is record 
      (title varchar(50), 
      author varchar(50), 
      subject varchar(100), 
      book_id number); 
   book1 books; 
   book2 books; 
BEGIN 
   -- Book 1 specification 
   book1.title  := 'C Programming'; 
   book1.author := 'Nuha Ali ';  
   book1.subject := 'C Programming Tutorial'; 
   book1.book_id := 6495407;  
   -- Book 2 specification 
   book2.title := 'Telecom Billing'; 
   book2.author := 'Zara Ali'; 
   book2.subject := 'Telecom Billing Tutorial'; 
   book2.book_id := 6495700;  
  
  -- Print book 1 record 
   dbms_output.put_line('Book 1 title : '|| book1.title); 
   dbms_output.put_line('Book 1 author : '|| book1.author); 
   dbms_output.put_line('Book 1 subject : '|| book1.subject); 
   dbms_output.put_line('Book 1 book_id : ' || book1.book_id); 
   
   -- Print book 2 record 
   dbms_output.put_line('Book 2 title : '|| book2.title); 
   dbms_output.put_line('Book 2 author : '|| book2.author); 
   dbms_output.put_line('Book 2 subject : '|| book2.subject); 
   dbms_output.put_line('Book 2 book_id : '|| book2.book_id); 
END; 
/
```


```
DECLARE 
   c_id customers.id%type := 8; 
   c_name customerS.Name%type; 
   c_addr customers.address%type; 
BEGIN 
   SELECT  name, address INTO  c_name, c_addr 
   FROM customers 
   WHERE id = c_id;  
   DBMS_OUTPUT.PUT_LINE ('Name: '||  c_name); 
   DBMS_OUTPUT.PUT_LINE ('Address: ' || c_addr); 

EXCEPTION 
   WHEN no_data_found THEN 
      dbms_output.put_line('No such customer!'); 
   WHEN others THEN 
      dbms_output.put_line('Error!'); 
END; 
/
```


```
DECLARE 
   exception_name EXCEPTION; 
BEGIN 
   IF condition THEN 
      RAISE exception_name; 
   END IF; 
EXCEPTION 
   WHEN exception_name THEN 
   statement; 
END; 
```


```
DECLARE 
   c_id customers.id%type := &cc_id; 
   c_name customerS.Name%type; 
   c_addr customers.address%type;  
   -- user defined exception 
   ex_invalid_id  EXCEPTION; 
BEGIN 
   IF c_id <= 0 THEN 
      RAISE ex_invalid_id; 
   ELSE 
      SELECT  name, address INTO  c_name, c_addr 
      FROM customers 
      WHERE id = c_id;
      DBMS_OUTPUT.PUT_LINE ('Name: '||  c_name);  
      DBMS_OUTPUT.PUT_LINE ('Address: ' || c_addr); 
   END IF; 

EXCEPTION 
   WHEN ex_invalid_id THEN 
      dbms_output.put_line('ID must be greater than zero!'); 
   WHEN no_data_found THEN 
      dbms_output.put_line('No such customer!'); 
   WHEN others THEN 
      dbms_output.put_line('Error!');  
END; 
/
```

```
CREATE [OR REPLACE ] TRIGGER trigger_name  
{BEFORE | AFTER | INSTEAD OF }  
{INSERT [OR] | UPDATE [OR] | DELETE}  
[OF col_name]  
ON table_name  
[REFERENCING OLD AS o NEW AS n]  
[FOR EACH ROW]  
WHEN (condition)   
DECLARE 
   Declaration-statements 
BEGIN  
   Executable-statements 
EXCEPTION 
   Exception-handling-statements 
END; 
```


```
CREATE OR REPLACE TRIGGER display_salary_changes 
BEFORE DELETE OR INSERT OR UPDATE ON customers 
FOR EACH ROW 
WHEN (NEW.ID > 0) 
DECLARE 
   sal_diff number; 
BEGIN 
   sal_diff := :NEW.salary  - :OLD.salary; 
   dbms_output.put_line('Old salary: ' || :OLD.salary); 
   dbms_output.put_line('New salary: ' || :NEW.salary); 
   dbms_output.put_line('Salary difference: ' || sal_diff); 
END; 
/ 
```


```
CREATE PACKAGE cust_sal AS 
   PROCEDURE find_sal(c_id customers.id%type); 
END cust_sal; 
/
```

```
CREATE OR REPLACE PACKAGE BODY cust_sal AS     
   PROCEDURE find_sal(c_id customers.id%TYPE) IS 
   c_sal customers.salary%TYPE; 
   BEGIN 
      SELECT salary INTO c_sal 
      FROM customers 
      WHERE id = c_id; 
      dbms_output.put_line('Salary: '|| c_sal); 
   END find_sal; 
END cust_sal; 
/
```

```
DECLARE 
   code customers.id%type := &cc_id; 
BEGIN 
   cust_sal.find_sal(code); 
END; 
/
```

```
CREATE OR REPLACE PACKAGE c_package AS 
   -- Adds a customer 
   PROCEDURE addCustomer(c_id   customers.id%type, 
   c_name customers.Name%type, 
   c_age  customers.age%type, 
   c_addr customers.address%type,  
   c_sal  customers.salary%type); 
   
   -- Removes a customer 
   PROCEDURE delCustomer(c_id  customers.id%TYPE); 
   --Lists all customers 
   PROCEDURE listCustomer; 
  
END c_package; 
/
```

```
CREATE OR REPLACE PACKAGE BODY c_package AS 
   PROCEDURE addCustomer(c_id  customers.id%type, 
      c_name customers.Name%type, 
      c_age  customers.age%type, 
      c_addr  customers.address%type,  
      c_sal   customers.salary%type) 
   IS 
   BEGIN 
      INSERT INTO customers (id,name,age,address,salary) 
         VALUES(c_id, c_name, c_age, c_addr, c_sal); 
   END addCustomer; 
   
   PROCEDURE delCustomer(c_id   customers.id%type) IS 
   BEGIN 
      DELETE FROM customers 
      WHERE id = c_id; 
   END delCustomer;  
   
   PROCEDURE listCustomer IS 
   CURSOR c_customers is 
      SELECT  name FROM customers; 
   TYPE c_list is TABLE OF customers.Name%type; 
   name_list c_list := c_list(); 
   counter integer :=0; 
   BEGIN 
      FOR n IN c_customers LOOP 
      counter := counter +1; 
      name_list.extend; 
      name_list(counter) := n.name; 
      dbms_output.put_line('Customer(' ||counter|| ')'||name_list(counter)); 
      END LOOP; 
   END listCustomer;
   
END c_package; 
/
```

```
DECLARE 
   code customers.id%type:= 8; 
BEGIN 
   c_package.addcustomer(7, 'Rajnish', 25, 'Chennai', 3500); 
   c_package.addcustomer(8, 'Subham', 32, 'Delhi', 7500); 
   c_package.listcustomer; 
   c_package.delcustomer(code); 
   c_package.listcustomer; 
END; 
/  
```

```
DECLARE 
   TYPE salary IS TABLE OF NUMBER INDEX BY VARCHAR2(20); 
   salary_list salary; 
   name   VARCHAR2(20); 
BEGIN 
   -- adding elements to the table 
   salary_list('Rajnish') := 62000; 
   salary_list('Minakshi') := 75000; 
   salary_list('Martin') := 100000; 
   salary_list('James') := 78000;  
   
   -- printing the table 
   name := salary_list.FIRST; 
   WHILE name IS NOT null LOOP 
      dbms_output.put_line 
      ('Salary of ' || name || ' is ' || TO_CHAR(salary_list(name))); 
      name := salary_list.NEXT(name); 
   END LOOP; 
END; 
/
```

```
DECLARE 
   CURSOR c_customers is 
      select name from customers; 

   TYPE c_list IS TABLE of customers.Name%type INDEX BY binary_integer; 
   name_list c_list; 
   counter integer :=0; 
BEGIN 
   e; FOR n IN c_customers LOOP 
      counter := counter +1; 
      name_list(counter) := n.nam
      dbms_output.put_line('Customer('||counter||'):'||name_lis t(counter)); 
   END LOOP; 
END; 
/ 
```


```
DECLARE 
   TYPE names_table IS TABLE OF VARCHAR2(10); 
   TYPE grades IS TABLE OF INTEGER;  
   names names_table; 
   marks grades; 
   total integer; 
BEGIN 
   names := names_table('Kavita', 'Pritam', 'Ayan', 'Rishav', 'Aziz'); 
   marks:= grades(98, 97, 78, 87, 92); 
   total := names.count; 
   dbms_output.put_line('Total '|| total || ' Students'); 
   FOR i IN 1 .. total LOOP 
      dbms_output.put_line('Student:'||names(i)||', Marks:' || marks(i)); 
   end loop; 
END; 
/  
```


```
SELECT SYSDATE FROM DUAL; 
SELECT TO_CHAR(CURRENT_DATE, 'DD-MM-YYYY HH:MI:SS') FROM DUAL; 
SELECT ADD_MONTHS(SYSDATE, 5) FROM DUAL;
SELECT LOCALTIMESTAMP FROM DUAL; 
```

```
BEGIN 
   dbms_output.put_line  (user || ' Tables in the database:'); 
   FOR t IN (SELECT table_name FROM user_tables) 
   LOOP 
      dbms_output.put_line(t.table_name); 
   END LOOP; 
END; 
/ 
```


```
CREATE OR REPLACE TYPE address AS OBJECT 
(house_no varchar2(10), 
 street varchar2(30), 
 city varchar2(20), 
 state varchar2(10), 
 pincode varchar2(10) 
); 
/ 
```

```
CREATE OR REPLACE TYPE customer AS OBJECT 
(code number(5), 
 name varchar2(30), 
 contact_no varchar2(12), 
 addr address, 
 member procedure display 
); 
/ 
```

```
DECLARE 
   residence address; 
BEGIN 
   residence := address('103A', 'M.G.Road', 'Jaipur', 'Rajasthan','201301'); 
   dbms_output.put_line('House No: '|| residence.house_no); 
   dbms_output.put_line('Street: '|| residence.street); 
   dbms_output.put_line('City: '|| residence.city); 
   dbms_output.put_line('State: '|| residence.state); 
   dbms_output.put_line('Pincode: '|| residence.pincode); 
END; 
/ 
```


```
CREATE OR REPLACE TYPE rectangle AS OBJECT 
(length number, 
 width number, 
 member function enlarge( inc number) return rectangle, 
 member procedure display, 
 map member function measure return number 
); 
/ 
```

```
CREATE OR REPLACE TYPE BODY rectangle AS 
   MEMBER FUNCTION enlarge(inc number) return rectangle IS 
   BEGIN 
      return rectangle(self.length + inc, self.width + inc); 
   END enlarge;  
   MEMBER PROCEDURE display IS 
   BEGIN  
      dbms_output.put_line('Length: '|| length); 
      dbms_output.put_line('Width: '|| width); 
   END display;  
   MAP MEMBER FUNCTION measure return number IS 
   BEGIN 
      return (sqrt(length*length + width*width)); 
   END measure; 
END; 
/ 
```

```
DECLARE 
   r1 rectangle; 
   r2 rectangle; 
   r3 rectangle; 
   inc_factor number := 5; 
BEGIN 
   r1 := rectangle(3, 4); 
   r2 := rectangle(5, 7); 
   r3 := r1.enlarge(inc_factor); 
   r3.display;  
   IF (r1 > r2) THEN -- calling measure function 
      r1.display; 
   ELSE 
      r2.display; 
   END IF; 
END; 
/
```

```
CREATE OR REPLACE TYPE rectangle AS OBJECT 
(length number, 
 width number, 
 member procedure display, 
 order member function measure(r rectangle) return number 
); 
/
```

```
CREATE OR REPLACE TYPE BODY rectangle AS 
   MEMBER PROCEDURE display IS 
   BEGIN 
      dbms_output.put_line('Length: '|| length); 
      dbms_output.put_line('Width: '|| width); 
   END display;  
   ORDER MEMBER FUNCTION measure(r rectangle) return number IS 
   BEGIN 
      IF(sqrt(self.length*self.length + self.width*self.width)> 
         sqrt(r.length*r.length + r.width*r.width)) then 
         return(1); 
      ELSE 
         return(-1); 
      END IF; 
   END measure; 
END; 
/ 
```

```
DECLARE 
   r1 rectangle; 
   r2 rectangle; 
BEGIN 
   r1 := rectangle(23, 44); 
   r2 := rectangle(15, 17); 
   r1.display; 
   r2.display; 
   IF (r1 > r2) THEN -- calling measure function 
      r1.display; 
   ELSE 
      r2.display; 
   END IF; 
END; 
/
```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```
