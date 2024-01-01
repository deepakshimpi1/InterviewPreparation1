## Reference
- https://www.interviewbit.com/sql-interview-questions/#what-is-data-integrity


## .How to find nth highest salary in SQL?

Highest salary:
```
select max(salary) from emp;
```

2nd highest salary
- Using sub query

```
select max(salary) from emp where 
sal < (select max(salary) from emp)
```

nth highest salary
- Using sub query
```
select top 1 salary
    (select distinct top N salary
    from emp
    order by salary desc) result
order by salary
```
- Using CTE and DENSE_RANK()
```
with Result as (
    select salary, DENSE_RANK() over(order by salary desc) as DenseRank
    from emp
)

select top 1 salary from Result
Where DenseRank = N
```
Note: If you use ROW_NUMBER, then it will not work with duplicate records.

---

## What is DBMS and RDBMS?

### DBMS (Database Management System):
 
- A DBMS is a software that allows users to interact with a database. 
- It provides an interface for managing and organizing data. 
- However, it `doesn't` enforce `relationships between tables or ensure the integrity of the data`. 
- In a DBMS, data is typically stored in `files`, and it supports basic functionalities like data retrieval, insertion, deletion, and updating.

### Example of a DBMS:
- Consider a simple flat-file database system used to manage a list of students in a school. 
- The system might consist of `separate files` for student information, courses, and grades. The DBMS would provide basic functions to add, retrieve, update, and delete data from these files. 
- However, it doesn't establish relationships between the files or enforce any constraints.

### RDBMS (Relational Database Management System):

- An RDBMS is a type of DBMS that manages data in a relational database. 
- It `enforces` the `relationships between tables, ensures data integrity through constraints, and supports the use of SQL (Structured Query Language) for complex queries`. 
- In an RDBMS, data is organized into tables with rows and columns, and relationships between tables are established using keys.

### Example of an RDBMS:

- Continuing with the school example, in an RDBMS, you might have separate tables for students, courses, and grades. Each student's record is identified by a unique student ID, and this ID is used as a foreign key in the grades table to establish a relationship between students and their grades.
- The RDBMS ensures that only valid student IDs can exist in the grades table, maintaining referential integrity.

## SQL constraints

- Constraints are used to specify the rules for the data in the table.
- It can be applied for single or multiple fields in an SQL table during the creation of the table or after creating using the ALTER TABLE command.

1. `NOT NULL` - Restricts NULL value from being inserted into a column. 
1. `CHECK` - Verifies that all values in a field satisfy a condition.
1. `DEFAULT` - Automatically assigns a default value if no value has been specified for the field.
1. `UNIQUE` - Ensures unique values to be inserted into the field.
1. `INDEX` - Indexes a field providing faster retrieval of records.
1. `PRIMARY KEY` - Uniquely identifies each record in a table.
1. `FOREIGN KEY` - Ensures referential integrity for a record in another table (Primary key of another table).

### Example

```SQL
CREATE TABLE Students (   /* Create table with multiple fields as primary key */
   ID INT NOT NULL
   LastName VARCHAR(255)
   FirstName VARCHAR(255) NOT NULL,
   Age INT CHECK (Age >= 18),
   AdmissionDate Date DEFAULT GETDATE()
   PRIMARY KEY (ID)
);
```
---

## What is a Primary Key?
- The PRIMARY KEY constraint uniquely identifies each row in a table. 
- It must contain UNIQUE values and has an implicit NOT NULL constraint.
- A table in SQL is strictly restricted to have one and only one primary key, which is comprised of single or multiple fields (columns).
```sql
CREATE TABLE Students (   /* Create table with a single field as primary key */
   ID INT NOT NULL
   Name VARCHAR(255)
   PRIMARY KEY (ID)
);

CREATE TABLE Students (   /* Create table with multiple fields as primary key */
   ID INT NOT NULL
   LastName VARCHAR(255)
   FirstName VARCHAR(255) NOT NULL,
   CONSTRAINT PK_Student
   PRIMARY KEY (ID, FirstName)
);

ALTER TABLE Students   /* Set a column as primary key */
ADD PRIMARY KEY (ID);

ALTER TABLE Students   /* Set multiple columns as primary key */
ADD CONSTRAINT PK_Student   /*Naming a Primary Key*/
PRIMARY KEY (ID, FirstName);
```
## What is a UNIQUE constraint?

- A UNIQUE constraint ensures that all values in a column are different. - This provides uniqueness for the column(s) and helps identify each row uniquely. 
- Unlike primary key, there can be multiple unique constraints defined per table. 
- The code syntax for UNIQUE is quite similar to that of PRIMARY KEY and can be used interchangeably.

```SQL
CREATE TABLE Students (   /* Create table with a single field as unique */
   ID INT NOT NULL UNIQUE
   Name VARCHAR(255)
);

CREATE TABLE Students (   /* Create table with multiple fields as unique */
   ID INT NOT NULL
   LastName VARCHAR(255)
   FirstName VARCHAR(255) NOT NULL
   CONSTRAINT PK_Student
   UNIQUE (ID, FirstName)
);

ALTER TABLE Students   /* Set a column as unique */
ADD UNIQUE (ID);

ALTER TABLE Students   /* Set multiple columns as unique */
ADD CONSTRAINT PK_Student   /* Naming a unique constraint */
UNIQUE (ID, FirstName);
```

---
## What is a Foreign Key?

- A FOREIGN KEY comprises of single or collection of fields in a table that essentially refers to the PRIMARY KEY in another table. 
- Foreign key constraint ensures referential integrity in the relation between two tables.
- The table with the foreign key constraint is labeled as the child table, and the table containing the candidate key is labeled as the referenced or parent table.

```SQL
CREATE TABLE Students (   /* Create table with foreign key - Way 1 */
   ID INT NOT NULL
   Name VARCHAR(255)
   LibraryID INT
   PRIMARY KEY (ID)
   FOREIGN KEY (Library_ID) REFERENCES Library(LibraryID)
);

CREATE TABLE Students (   /* Create table with foreign key - Way 2 */
   ID INT NOT NULL PRIMARY KEY
   Name VARCHAR(255)
   LibraryID INT FOREIGN KEY (Library_ID) REFERENCES Library(LibraryID)
);

ALTER TABLE Students   /* Add a new foreign key */
ADD FOREIGN KEY (Library_ID) /* Student table column */
REFERENCES Library (LibraryID); /* Library table column */
```
---

## Primary Key and Unique Key

Certainly! Here's a comparison of primary keys and unique keys in a tabular format:

| Feature                               | Primary Key                                  | Unique Key                                     |
|----------------------------------------|----------------------------------------------|------------------------------------------------|
| Uniqueness                             | Unique identifier for each record.           | Ensures uniqueness but allows multiple occurrences of NULL values. |
| Null Values                            | Not allowed.                                 | Allowed, but uniqueness constraint applies only to non-NULL values. |
| Number of Columns                      | Can consist of one or more columns.         | Can be applied to a single column or a combination of columns. |
| Purpose                                | Used to uniquely identify records and establish relationships between tables. | Ensures uniqueness but not necessarily used for relationships. More flexible. |
| Number of Keys per Table               | Only one primary key per table.             | Multiple unique keys can be defined in a table.  |
| Automatic Indexing                     | Often automatically indexed for performance optimization. | May or may not be automatically indexed, depending on the database system. |


## Union vs UnionAll vs Intersection vs Minus(Expect)

 

| Operation       | Purpose                                                          | Duplicate Rows | NULL Handling | Example                                   |
|-----------------|------------------------------------------------------------------|-----------------|----------------|-------------------------------------------|
| UNION           | Combines the result sets of two or more SELECT statements.       | Eliminated      | Respects NULLs | `SELECT column1 FROM table1 UNION SELECT column1 FROM table2;` |
| UNION ALL       | Combines the result sets of two or more SELECT statements.       | Retained        | Respects NULLs | `SELECT column1 FROM table1 UNION ALL SELECT column1 FROM table2;` |
| INTERSECT       | Returns common rows between two SELECT statements.               | Eliminated      | Respects NULLs | `SELECT column1 FROM table1 INTERSECT SELECT column1 FROM table2;` |
| MINUS (or EXCEPT) | Returns rows that exist in the first SELECT statement but not in the second. | Eliminated      | Respects NULLs | `SELECT column1 FROM table1 MINUS SELECT column1 FROM table2;` |
 
---

## Having vs Where?

Here's a comparison of the HAVING and WHERE clauses in a tabular format:

| Feature          | WHERE Clause                                             | HAVING Clause                                            |
|-------------------|----------------------------------------------------------|----------------------------------------------------------|
| Purpose           | Filters rows before grouping or aggregation.             | Filters groups after grouping or aggregation.             |
| Used with         | Used with SELECT, UPDATE, DELETE statements.              | Used with SELECT statements that involve GROUP BY clauses.|
| Aggregation       | Typically used for individual row conditions.            | Typically used for aggregate conditions (e.g., SUM, AVG). |
| Type of Condition | Applies to individual rows in the result set.            | Applies to groups of rows based on GROUP BY criteria.     |
| Sequence          | Applied before the GROUP BY clause in a SELECT statement. | Applied after the GROUP BY clause in a SELECT statement.   |
| Example           | `SELECT column1 FROM table WHERE condition;`            | `SELECT column1, COUNT(*) FROM table GROUP BY column1 HAVING COUNT(*) > 1;` |

---

## Delete vs Truncate vs Drop
 
| Operation       | Purpose                                          | Rollback         | Where Clause    | Conditions    | Example                                          |
|-----------------|--------------------------------------------------|------------------|-----------------|---------------|--------------------------------------------------|
| DELETE          | Removes rows from a table based on a condition.  | Yes              | Yes             | Conditional   | `DELETE FROM table WHERE condition;`              |
| TRUNCATE        | Removes all rows from a table (fast, but less flexible). | No               | No              | N/A           | `TRUNCATE TABLE table;`                           |
| DROP            | Deletes an entire table (structure and data).   | No (Schema only) | N/A             | N/A           | `DROP TABLE table;`                              |

This table summarizes the key differences between DELETE, TRUNCATE, and DROP operations in a relational database.

---
## SQL joins 

**1. Sample Data for `employees` table:**

```sql
CREATE TABLE employees (
    employee_id INT,
    employee_name VARCHAR(255),
    department_id INT
);

INSERT INTO employees VALUES (1, 'John', 101);
INSERT INTO employees VALUES (2, 'Jane', 102);
INSERT INTO employees VALUES (3, 'Bob', 101);
INSERT INTO employees VALUES (4, 'Alice', 103);
```

**2. Sample Data for `departments` table:**

```sql
CREATE TABLE departments (
    department_id INT,
    department_name VARCHAR(255)
);

INSERT INTO departments VALUES (101, 'HR');
INSERT INTO departments VALUES (102, 'Finance');
INSERT INTO departments VALUES (103, 'IT');
```

Now, let's provide examples for each type of join using these tables:

**1. INNER JOIN:**

An INNER JOIN returns only the rows where there is a match in both tables.
```sql
SELECT employees.employee_id, employees.employee_name, departments.department_name
FROM employees
INNER JOIN departments ON employees.department_id = departments.department_id;
```

**2. LEFT JOIN (or LEFT OUTER JOIN):**

A LEFT JOIN returns all rows from the left table (employees) and the matched rows from the right table (departments). If there is no match, NULL values are returned for columns from the right table.
```sql
SELECT employees.employee_id, employees.employee_name, departments.department_name
FROM employees
LEFT JOIN departments ON employees.department_id = departments.department_id;
```

**3. RIGHT JOIN (or RIGHT OUTER JOIN):**

A RIGHT JOIN returns all rows from the right table (departments) and the matched rows from the left table (employees). If there is no match, NULL values are returned for columns from the left table.
```sql
SELECT employees.employee_id, employees.employee_name, departments.department_name
FROM employees
RIGHT JOIN departments ON employees.department_id = departments.department_id;
```

**4. FULL JOIN (or FULL OUTER JOIN):**

A FULL JOIN returns all rows when there is a match in either the left or right table. If there is no match, NULL values are returned for columns from the table without a match.
```sql
SELECT employees.employee_id, employees.employee_name, departments.department_name
FROM employees
FULL JOIN departments ON employees.department_id = departments.department_id;
```

These examples include both the SQL query and the corresponding data for the `employees` and `departments` tables, illustrating how the joins work in the context of the provided data.
 
1. **Employee and Department Information:**
   **Question:**
   You have two tables, `employees` and `departments`. The `employees` table contains information about employees (employee_id, employee_name, department_id), and the `departments` table contains information about departments (department_id, department_name). Write an SQL query to retrieve a list of employees with their corresponding department names.

   **Answer:**
   ```sql
   SELECT employees.employee_id, employees.employee_name, departments.department_name
   FROM employees
   INNER JOIN departments ON employees.department_id = departments.department_id;
   ```

2. **Customers and Orders:**
   **Question:**
   In a database, you have two tables - `customers` and `orders`. The `customers` table includes information about customers (customer_id, customer_name), and the `orders` table contains order details (order_id, customer_id, order_date). Write an SQL query to retrieve a list of customers along with the total number of orders each customer has placed.

   **Answer:**
   ```sql
   SELECT customers.customer_id, customers.customer_name, COUNT(orders.order_id) AS total_orders
   FROM customers
   LEFT JOIN orders ON customers.customer_id = orders.customer_id
   GROUP BY customers.customer_id, customers.customer_name;
   ```

3. **Product Categories and Sales:**
   **Question:**
   You have two tables, `products` and `sales`. The `products` table includes information about products (product_id, product_name, category_id), and the `sales` table contains sales data (sale_id, product_id, sale_amount). Write an SQL query to display the total sales amount for each product category.

   **Answer:**
   ```sql
   SELECT products.category_id, SUM(sales.sale_amount) AS total_sales
   FROM products
   LEFT JOIN sales ON products.product_id = sales.product_id
   GROUP BY products.category_id;
   ```

4. **Employees and Managers:**
   **Question:**
   Given a table named `employees` with columns (employee_id, employee_name, manager_id), write an SQL query to retrieve the names of employees along with the names of their respective managers.

   **Answer:**
   ```sql
   SELECT e.employee_name, m.employee_name AS manager_name
   FROM employees e
   LEFT JOIN employees m ON e.manager_id = m.employee_id;
   ```

5. **Students and Courses:**
   **Question:**
   You have three tables, `students` (student_id, student_name), `courses` (course_id, course_name), and `grades` (grade_id, student_id, course_id, grade). Write an SQL query to retrieve a list of students and the courses they are enrolled in.

   **Answer:**
   ```sql
   SELECT students.student_name, courses.course_name
   FROM students
   INNER JOIN enrollments ON students.student_id = enrollments.student_id
   INNER JOIN courses ON enrollments.course_id = courses.course_id;
   ```

6. **User Activity Tracking:**
   **Question:**
   In a web application, you have two tables - `users` and `activity_logs`. The `users` table contains user information (user_id, username), and the `activity_logs` table contains logs of user activity (log_id, user_id, activity_type, timestamp). Write an SQL query to retrieve the usernames of users who have performed a specific activity within a given time range.

   **Answer:**
   ```sql
   SELECT DISTINCT users.username
   FROM users
   INNER JOIN activity_logs ON users.user_id = activity_logs.user_id
   WHERE activity_logs.activity_type = 'specific_activity'
   AND activity_logs.timestamp BETWEEN 'start_date' AND 'end_date';
   ```

7. **Employee Salaries and Departments:**
   **Question:**
   Given two tables, `employees` (employee_id, employee_name, department_id) and `salaries` (employee_id, salary_amount), write an SQL query to retrieve the average salary for each department.

   **Answer:**
   ```sql
   SELECT employees.department_id, AVG(salaries.salary_amount) AS avg_salary
   FROM employees
   LEFT JOIN salaries ON employees.employee_id = salaries.employee_id
   GROUP BY employees.department_id;
   ```

8. **Student Grades and Courses:**
   **Question:**
   You have three tables, `students` (student_id, student_name), `courses` (course_id, course_name), and `grades` (grade_id, student_id, course_id, grade). Write an SQL query to find the average grade for each course.

   **Answer:**
   ```sql
   SELECT courses.course_id, courses.course_name, AVG(grades.grade) AS avg_grade
   FROM courses
   LEFT JOIN grades ON courses.course_id = grades.course_id
   GROUP BY courses.course_id, courses.course_name;
   ```

9. **Product Reviews and Users:**
   **Question:**
   Given two tables, `products` (product_id, product_name) and `reviews` (review_id, product_id, user_id, review_text), write an SQL query to retrieve the names of products along with the total number of reviews each product has received.

   **Answer:**
   ```sql
   SELECT products.product_name, COUNT(reviews.review_id) AS total_reviews
   FROM products
   LEFT JOIN reviews ON products.product_id = reviews.product_id
   GROUP BY products.product_name;
   ```

10. **Employee Hierarchies:**
    **Question:**
    In an organization, you have a table named `employees` with columns (employee_id, employee_name, manager_id). Write an SQL query to display the employee hierarchy, showing each employee and their respective manager, including the CEO at the top level.

    **Answer:**
    ```sql
    SELECT e.employee_name, m.employee_name AS manager_name
    FROM employees e
    LEFT JOIN employees m ON e.manager_id = m.employee_id
    ORDER BY manager_name, e.employee_name;
    ```

These questions and answers cover a variety of scenarios involving different types of joins and data relationships.

---
## Triggers

A `trigger` is a set of instructions or programs that are `automatically executed` ("triggered") in response to certain events on a particular table or view. 

These events can include data manipulation language (DML) events such as INSERT, UPDATE, DELETE operations, or data definition language (DDL) events like CREATE, ALTER, and DROP.

### Syntax

```sql
CREATE [OR REPLACE] TRIGGER trigger_name
{BEFORE | AFTER} {INSERT | UPDATE | DELETE}
ON table_name
[FOR EACH ROW]
WHEN (condition)
BEGIN
   -- Trigger body (SQL statements or procedure calls)
END;
```

### Example
- Auditing changes to data.

### Real time example:

**Scenario: Employee Salary Adjustment**

Suppose you have an HR database with an `employees` table, and you want to create a trigger that automatically updates an `audit_log` table whenever an employee's salary is adjusted. The `audit_log` table will store information about the employee, the old salary, and the new salary.

Here are the tables:

1. **`employees` Table:**
   ```sql
   CREATE TABLE employees (
      employee_id INT PRIMARY KEY,
      employee_name VARCHAR(255),
      salary DECIMAL(10, 2)
   );

   INSERT INTO employees VALUES (1, 'John Doe', 50000.00);
   INSERT INTO employees VALUES (2, 'Jane Smith', 60000.00);
   ```

2. **`audit_log` Table:**
   ```sql
   CREATE TABLE audit_log (
      log_id INT PRIMARY KEY,
      employee_id INT,
      old_salary DECIMAL(10, 2),
      new_salary DECIMAL(10, 2),
      adjustment_date TIMESTAMP
   );
   ```

Now, let's create a trigger that automatically updates the `audit_log` table when an employee's salary is adjusted:

```sql
CREATE OR REPLACE TRIGGER salary_adjustment_trigger
BEFORE UPDATE OF salary ON employees
FOR EACH ROW
BEGIN
   IF :NEW.salary != :OLD.salary THEN
      INSERT INTO audit_log (log_id, employee_id, old_salary, new_salary, adjustment_date)
      VALUES (
         audit_log_seq.NEXTVAL,
         :NEW.employee_id,
         :OLD.salary,
         :NEW.salary,
         CURRENT_TIMESTAMP
      );
   END IF;
END;
/
```

Explanation of the trigger:

- **Trigger Name:** `salary_adjustment_trigger`
- **Timing:** `BEFORE UPDATE OF salary ON employees` means the trigger will fire before an update operation on the `salary` column of the `employees` table.
- **FOR EACH ROW:** Indicates that the trigger operates at the row level.
- **BEGIN...END:** Encloses the trigger body.
- **Condition:** Checks if the new salary (`:NEW.salary`) is different from the old salary (`:OLD.salary`). If there's a difference, it means a salary adjustment is happening.
- **INSERT INTO audit_log:** If a salary adjustment is detected, it inserts a record into the `audit_log` table, capturing the employee ID, old salary, new salary, and the current timestamp.

Now, let's test the trigger:

```sql
-- Update John Doe's salary
UPDATE employees SET salary = 55000.00 WHERE employee_id = 1;

-- View the contents of the audit_log table
SELECT * FROM audit_log;
```

The `audit_log` table should now contain a record reflecting the salary adjustment for John Doe.

### Instead Off Triggers

Here are the key points about "INSTEAD OF" triggers:

1. **Purpose:**
   - "INSTEAD OF" triggers are used to `replace the default action of an event with custom logic` defined by the trigger.
   - They are commonly used with views to allow for custom processing when certain operations are performed on the view.

2. **Associated Events:**
   - "INSTEAD OF" triggers are typically associated with DML (Data Manipulation Language) events like INSERT, UPDATE, and DELETE.

3. **Usage with Views:**
   - One common use case for "INSTEAD OF" triggers is with updatable views. By default, some views may not be updatable because they represent complex queries involving multiple tables. "INSTEAD OF" triggers can be used to define how changes to the view should be handled.

4. **Syntax:**
   - The syntax for creating an "INSTEAD OF" trigger varies among database management systems (DBMS), but in general, it includes the trigger type ("INSTEAD OF"), the triggering event (e.g., INSERT, UPDATE, DELETE), the associated table or view, and the trigger body with custom logic.

Here's a simplified example using a hypothetical scenario:

```sql
-- Create a view
CREATE VIEW employee_info AS
SELECT employee_id, employee_name, salary
FROM employees;

-- Create an INSTEAD OF INSERT trigger for the view
CREATE OR REPLACE TRIGGER instead_of_insert_employee_info
INSTEAD OF INSERT ON employee_info
FOR EACH ROW
BEGIN
   -- Custom logic for handling the INSERT operation
   INSERT INTO employees (employee_id, employee_name, salary)
   VALUES (:NEW.employee_id, :NEW.employee_name, :NEW.salary * 1.1); -- Give a 10% salary increase
END;
```

In this example, the "INSTEAD OF INSERT" trigger is associated with the `employee_info` view. When an attempt is made to insert a new record into the `employee_info` view, the trigger intercepts the operation and executes custom logic to handle the insertion, such as modifying the salary before inserting it into the underlying `employees` table.

Sure, let's go through examples of both BEFORE and AFTER triggers using a hypothetical scenario with an `employees` table.

### BEFORE Trigger Example:

Suppose you want to create a BEFORE INSERT trigger that automatically sets a default value for the `joining_date` column if it is not provided during an INSERT operation.

```sql
-- Create the employees table
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(255),
    joining_date DATE
);

-- Create a BEFORE INSERT trigger
CREATE OR REPLACE TRIGGER before_insert_joining_date
BEFORE INSERT ON employees
FOR EACH ROW
BEGIN
   IF :NEW.joining_date IS NULL THEN
      :NEW.joining_date := SYSDATE; -- Set current date as the default
   END IF;
END;
/
```

In this example, the `before_insert_joining_date` trigger is set to execute before an INSERT operation on the `employees` table. If the `joining_date` is not provided during the INSERT, the trigger sets it to the current date (`SYSDATE`) as a default.

Now, let's test the trigger:

```sql
-- Insert a new employee without providing joining_date
INSERT INTO employees (employee_id, employee_name) VALUES (1, 'John Doe');

-- View the contents of the employees table
SELECT * FROM employees;
```

The trigger ensures that if `joining_date` is not provided, it defaults to the current date.

### AFTER Trigger Example:

Now, let's create an AFTER UPDATE trigger that logs information about updated salaries into an `audit_log` table.

```sql
-- Create the employees table
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(255),
    salary DECIMAL(10, 2)
);

-- Create the audit_log table
CREATE TABLE audit_log (
    log_id INT PRIMARY KEY,
    employee_id INT,
    old_salary DECIMAL(10, 2),
    new_salary DECIMAL(10, 2),
    update_date TIMESTAMP
);

-- Create an AFTER UPDATE trigger
CREATE OR REPLACE TRIGGER after_update_salary
AFTER UPDATE OF salary ON employees
FOR EACH ROW
BEGIN
   IF :OLD.salary != :NEW.salary THEN
      INSERT INTO audit_log (log_id, employee_id, old_salary, new_salary, update_date)
      VALUES (audit_log_seq.NEXTVAL, :NEW.employee_id, :OLD.salary, :NEW.salary, CURRENT_TIMESTAMP);
   END IF;
END;
/
```

In this example, the `after_update_salary` trigger is set to execute after an UPDATE operation on the `salary` column of the `employees` table. If the salary is actually updated, it logs information into the `audit_log` table.

Let's test the trigger:

```sql
-- Update an employee's salary
UPDATE employees SET salary = 55000.00 WHERE employee_id = 1;

-- View the contents of the audit_log table
SELECT * FROM audit_log;
```

The trigger logs information about the salary update into the `audit_log` table.

These examples illustrate the use of BEFORE and AFTER triggers to enforce business rules, provide default values, or log changes in response to specific events in the database. Keep in mind that the syntax and behavior of triggers can vary depending on the specific database management system you are using.

## Cursors

Cursors are particularly useful when dealing with queries that return multiple rows, and you need to perform operations on each row individually.

In SQL, a cursor is a database object that allows you to traverse the result set of a query one row at a time.

There are two main types of cursors: implicit and explicit.

### 1. Implicit Cursors:

Implicit cursors are automatically created by the database management system to handle the result sets of SQL statements. They are typically used in programming constructs like stored procedures, triggers, and functions. 

Implicit cursors are convenient, but they have limitations and are not as flexible as explicit cursors.

Here's an example of an implicit cursor in a stored procedure:

```sql
CREATE OR REPLACE PROCEDURE get_employee_names AS
BEGIN
   FOR employee_rec IN (SELECT employee_name FROM employees) LOOP
      -- Process each row
      DBMS_OUTPUT.PUT_LINE('Employee Name: ' || employee_rec.employee_name);
   END LOOP;
END;
/
```

In this example, the implicit cursor is used to loop through the result set of the SELECT statement and process each row.

### 2. Explicit Cursors:

Explicit cursors are defined, opened, fetched, and closed by the user. They provide more control and flexibility, allowing you to manage the cursor's lifecycle explicitly.

Here's an example of an explicit cursor:

```sql
-- Create the orders table
CREATE TABLE orders (
   order_id INT PRIMARY KEY,
   customer_id INT,
   order_date DATE,
   order_amount DECIMAL(10, 2)
);

-- Insert sample data
INSERT INTO orders VALUES (1, 101, '2023-01-01', 100.00);
INSERT INTO orders VALUES (2, 102, '2023-01-02', 150.00);
INSERT INTO orders VALUES (3, 101, '2023-01-03', 200.00);
INSERT INTO orders VALUES (4, 103, '2023-01-04', 120.00);

-- Create a procedure to calculate total order amount for each customer
CREATE OR REPLACE PROCEDURE calculate_total_order_amount AS
   -- Declare variables
   customer_id orders.customer_id%TYPE;
   total_amount DECIMAL(10, 2);
BEGIN
   -- Declare explicit cursor
   CURSOR order_cursor IS
      SELECT DISTINCT customer_id FROM orders;

   -- Open the cursor
   OPEN order_cursor;

   -- Fetch and process each customer's total order amount
   LOOP
      FETCH order_cursor INTO customer_id;
      EXIT WHEN order_cursor%NOTFOUND;

      -- Calculate total order amount for the customer
      SELECT SUM(order_amount) INTO total_amount
      FROM orders
      WHERE customer_id = order_cursor.customer_id;

      -- Display the result
      DBMS_OUTPUT.PUT_LINE('Customer ID: ' || customer_id || ', Total Order Amount: ' || total_amount);
   END LOOP;

   -- Close the cursor
   CLOSE order_cursor;
END;
/

```

In this example:

- The `DECLARE` section defines the explicit cursor (`order_cursor`).
- The `OPEN` statement opens the cursor.
- The `LOOP` statement fetches each row into the variable `customer_id` and processes it.
- The `EXIT WHEN` statement exits the loop when there are no more rows to fetch (`%NOTFOUND`).
- The `CLOSE` statement closes the cursor.


Cursors are often used when row-by-row processing is necessary, but they `can have performance implications`, especially in large datasets.


## Temp table

It is used to store temporary data temporarily.

It is typically created and populated with data during the execution of a query, stored procedure, or batch process, and it exists only for the duration of that session or transaction.

Temporary tables are local to the session or transaction that creates them. They are automatically dropped when the session ends or the transaction is committed or rolled back.

 Certainly! Temp tables can be useful in various scenarios, and here are a few additional examples:

### Scenario 1: Pagination with Large Result Sets

Suppose you have a table with a large number of rows, and you want to implement pagination efficiently. Using a temp table can help by storing a subset of the data based on the desired page and size.

```sql
-- Create a temp table for pagination
CREATE TABLE #TempPagination (
   row_number INT IDENTITY(1,1),
   column1 INT,
   column2 VARCHAR(255)
);

-- Insert a specific page of data into the temp table
INSERT INTO #TempPagination (column1, column2)
SELECT column1, column2
FROM OriginalTable
ORDER BY column1, column2
OFFSET 10 ROWS FETCH NEXT 20 ROWS ONLY;

-- Query the temp table for the paginated results
SELECT * FROM #TempPagination;

-- Optionally, drop the temp table
DROP TABLE #TempPagination;
```

In this scenario, the temp table stores a specific page of data, and you can perform efficient queries on the temp table to implement pagination.

### Scenario 2: Batch Processing or Data Transformation

Suppose you need to perform a complex data transformation or batch processing task that involves multiple steps. Using a temp table for interim storage can simplify the logic and make the process more modular.

```sql
-- Create a temp table for data transformation
CREATE TABLE #TempTransformation (
   column1 INT,
   transformed_value INT
);

-- Step 1: Populate the temp table with initial data
INSERT INTO #TempTransformation (column1)
SELECT column1
FROM OriginalTable;

-- Step 2: Perform a data transformation and update the temp table
UPDATE #TempTransformation
SET transformed_value = column1 * 2;

-- Step 3: Query the temp table for the final results
SELECT * FROM #TempTransformation;

-- Optionally, drop the temp table
DROP TABLE #TempTransformation;
```

In this example, the temp table is used to store interim results during a multi-step data transformation process.

### Scenario 3: Avoiding Duplicate Computations

Suppose you have a query with complex subqueries that are repeated multiple times. Using a temp table to store the results of the subqueries can avoid redundant computations and improve performance.

```sql
-- Create a temp table for subquery results
CREATE TABLE #TempSubqueryResults (
   column1 INT,
   subquery_result INT
);

-- Populate the temp table with results of a complex subquery
INSERT INTO #TempSubqueryResults (column1, subquery_result)
SELECT column1, (SELECT MAX(another_column) FROM AnotherTable WHERE AnotherTable.column1 = OriginalTable.column1)
FROM OriginalTable;

-- Query the temp table with the subquery results
SELECT * FROM #TempSubqueryResults;

-- Optionally, drop the temp table
DROP TABLE #TempSubqueryResults;
```

In this scenario, the temp table helps avoid redundant computations by storing the results of a complex subquery, which can then be reused in the main query.

These scenarios illustrate how temp tables can be employed in different situations to simplify complex tasks, improve performance, and enhance the readability of SQL queries. Keep in mind that the appropriateness of using temp tables depends on the specific requirements of the task and the characteristics of the database system you are working with.

## CTE

- CTE stands for Common Table Expression. 
- It is a temporary result set that you can reference within a SELECT, INSERT, UPDATE, or DELETE statement. 
- CTEs are defined using the WITH clause and offer a way to create more readable and modular SQL 
- CTEs are particularly useful for making complex queries more modular, readable, and maintainable. They provide a way to break down a complex task into smaller, more understandable components, resulting in cleaner and more organized SQL code.

## CTE vs Temp table
 

| Feature                               | Temporary Table                             | Common Table Expression (CTE)                    |
|---------------------------------------|---------------------------------------------|-------------------------------------------------|
| **Scope**                             | Session-specific or global (local or global temp tables) | Limited to the query where the CTE is defined      |
| **Lifespan**                          | Persists until explicitly dropped (`DROP TABLE`) or end of session (temporary tables) | Exists only for the duration of the query execution  |
| **Declaration**                       | Explicitly created with `CREATE TABLE`       | Defined within the query using `WITH` clause        |
| **Usage**                             | Can be used across multiple queries or sessions | Typically used within a single query or subquery    |
| **Storage**                           | Stored on disk or in memory, depending on the database engine | In-memory structure, no physical storage           |
| **Indexing**                          | Can have indexes for optimization            | No direct indexes, but may benefit from indexes on underlying tables |
| **Syntax**                            | `CREATE TABLE #TempTable (...)`              | `WITH CTE_Name AS (SELECT ...) SELECT * FROM CTE_Name` |
| **Cleanup**                           | Explicitly dropped with `DROP TABLE`         | Automatically deallocated at the end of the query   |
| **Recursive Queries**                 | Can be used for recursive queries (limited support) | Designed for recursive queries using `WITH RECURSIVE` |
| **Readability**                       | May introduce additional code for creation, population, and cleanup | Enhances query readability by breaking down complex logic |
| **Performance Considerations**        | Can have performance benefits, especially for large datasets or complex operations | Optimized by the database engine, often suitable for smaller datasets and simpler queries |

 

Common Table Expressions (CTEs) or Temporary Tables.

### Scenario 1: Recursive Query for Hierarchical Data

**Use of CTE:**
Suppose you have a table storing hierarchical data, like an organizational chart with employees and managers. You want to find all the subordinates of a specific manager.

```sql
-- Using CTE for recursive query
WITH RecursiveHierarchy AS (
   SELECT employee_id, employee_name, manager_id
   FROM employees
   WHERE manager_id = @selected_manager_id
   UNION ALL
   SELECT e.employee_id, e.employee_name, e.manager_id
   FROM employees e
   JOIN RecursiveHierarchy r ON e.manager_id = r.employee_id
)
SELECT * FROM RecursiveHierarchy;
```

In this case, a CTE is well-suited for handling recursive queries, allowing you to define the recursive part of the query within the CTE itself.

### Scenario 2: Aggregation with Result Set Reuse

**Use of Temporary Table:**
Suppose you have a complex query that involves aggregating data from multiple tables, and you need to reuse the aggregated result set in subsequent queries.

```sql
-- Using Temporary Table for aggregation
CREATE TEMPORARY TABLE TempAggregatedData AS (
   SELECT department_id, AVG(salary) AS avg_salary
   FROM employees
   GROUP BY department_id
);

-- Subsequent queries using the temporary table
SELECT * FROM TempAggregatedData WHERE avg_salary > 50000;
SELECT * FROM TempAggregatedData WHERE avg_salary < 40000;

-- Drop the temporary table when done
DROP TEMPORARY TABLE IF EXISTS TempAggregatedData;
```

In this case, using a Temporary Table allows you to store the aggregated result set for reuse in multiple queries, potentially avoiding redundant computations.

### Scenario 3: Complex Data Transformation

**Use of Temporary Table:**
Suppose you need to perform a multi-step data transformation involving calculations and updates, and you want to simplify the logic.

```sql
-- Using Temporary Table for data transformation
CREATE TEMPORARY TABLE TempTransformedData AS (
   SELECT employee_id, salary, bonus,
          (salary + bonus) AS total_compensation
   FROM employees
);

-- Update the temporary table with additional transformation
UPDATE TempTransformedData
SET total_compensation = total_compensation * 1.1; -- 10% increase

-- Query the final transformed data
SELECT * FROM TempTransformedData;

-- Drop the temporary table when done
DROP TEMPORARY TABLE IF EXISTS TempTransformedData;
```

In this scenario, using a Temporary Table helps break down the data transformation into more manageable steps, making the logic clearer.

### Summary:

- Use CTEs when dealing with recursive queries, smaller result sets, or when modularizing complex queries.
  
- Use Temporary Tables when dealing with larger result sets, complex data transformations, or when you need to reuse result sets across multiple queries or sessions.

Ultimately, the choice between CTEs and Temporary Tables depends on the specific needs of your task, the characteristics of your data, and the performance considerations of your database system. Each has its strengths, and the appropriate choice should align with the goals of your query or operation.

## Types of Temp table

In SQL, temporary tables can be categorized into two main types: local temporary tables and global temporary tables. The key difference between them lies in their scope and lifespan.

1. **Local Temporary Tables:**
   - **Scope:** Local temporary tables are visible only to the session that creates them.
   - **Lifespan:** They are automatically dropped when the session ends, and they are not visible to other sessions.
   - **Naming Convention:** Local temporary tables are usually prefixed with a single pound sign (`#`).

   **Example:**
   ```sql
   -- Create a local temporary table
   CREATE TABLE #LocalTempTable (
      ID INT PRIMARY KEY,
      Name VARCHAR(255)
   );

   -- Use the local temporary table within the same session
   INSERT INTO #LocalTempTable VALUES (1, 'John');

   -- The table will be automatically dropped when the session ends
   ```

2. **Global Temporary Tables:**
   - **Scope:** Global temporary tables are visible to all sessions.
   - **Lifespan:** They are automatically dropped when the last session using the table ends.
   - **Naming Convention:** Global temporary tables are usually prefixed with a double pound sign (`##`).

   **Example:**
   ```sql
   -- Create a global temporary table
   CREATE TABLE ##GlobalTempTable (
      ID INT PRIMARY KEY,
      Name VARCHAR(255)
   );

   -- Use the global temporary table across multiple sessions
   INSERT INTO ##GlobalTempTable VALUES (1, 'Jane');

   -- The table will be automatically dropped when the last session using it ends
   ```

**Usage Considerations:**

- Use local temporary tables when the data is needed for a specific session and should not be visible to other sessions.
  
- Use global temporary tables when the data needs to be shared across multiple sessions and should persist until the last session referencing it is closed.

- Keep in mind that the naming conventions with single (`#`) or double (`##`) pound signs are conventions, and you can adjust the prefix as needed based on your organization's coding standards.

- Temporary tables are typically used for intermediate results or for storing temporary data during complex operations. Always consider the necessity of using temporary tables, as they introduce additional complexity and potential performance implications. In many cases, alternatives like table variables, derived tables, or CTEs may be sufficient.

## Temp variable
 
1. **Declaration:**
   - Table variables are declared using the `DECLARE` statement, and their structure is defined similarly to a regular table.
  
   ```sql
   DECLARE @TableVariable TABLE (
      Column1 INT,
      Column2 VARCHAR(255)
   );
   ```

2. **Usage:**
   - You can use table variables to store and manipulate data within the scope of the current batch, stored procedure, or function.

   ```sql
   -- Inserting data into the table variable
   INSERT INTO @TableVariable (Column1, Column2) VALUES (1, 'John'), (2, 'Jane');

   -- Querying the table variable
   SELECT * FROM @TableVariable;
   ```

3. **Scope:**
   - Table variables are local to the batch, stored procedure, or function in which they are declared. They are not visible outside of that scope.

4. **Deallocating:**
   - Table variables are automatically deallocated when the scope in which they are declared is exited. You don't need to explicitly drop or deallocate them.

5. **Transaction Scope:**
   - Table variables are bound to the transaction scope of the batch or the transaction in which they are used.

**Example:**

```sql
-- Declare a table variable
DECLARE @EmployeeTable TABLE (
   EmployeeID INT,
   EmployeeName VARCHAR(255)
);

-- Insert data into the table variable
INSERT INTO @EmployeeTable VALUES (1, 'John'), (2, 'Jane'), (3, 'Doe');

-- Query the table variable
SELECT * FROM @EmployeeTable;
```

Table variables are often used when you need a temporary storage solution for a result set within a specific scope, and you want the variable to be automatically deallocated when the scope is exited. They provide a convenient way to work with temporary data without the need for explicit cleanup. Keep in mind that, similar to temporary tables, the use of table variables should be considered based on the specific requirements of your task and the performance characteristics of your database system.

## Temp table vs Temp Variable


| Feature                              | Temporary Table                                       | Table Variable                                        |
| ------------------------------------ | ----------------------------------------------------- | ----------------------------------------------------- |
| **Declaration**                      | Created using the `CREATE TEMPORARY TABLE` statement. | Declared using the `DECLARE` statement with the `TABLE` keyword. |
| **Structure Definition**             | Supports column definitions, indexes, and constraints. | Supports column definitions but no indexes or constraints. |
| **Naming Convention**                | Prefixed with a single pound sign (`#`).               | Prefixed with an `@` symbol.                            |
| **Scope**                            | Can have local or global scope (local if prefixed with a single pound sign `#`, global if prefixed with a double pound sign `##`). | Local to the batch, stored procedure, or function where it is declared. |
| **Visibility**                       | Visible to the session or all sessions, depending on local or global scope. | Visible only within the scope in which it is declared.   |
| **Lifetime**                         | Persists until explicitly dropped or until the session ends for local temporary tables. Persists until the last session using it ends for global temporary tables. | Automatically deallocated when the scope (batch, stored procedure, or function) is exited. |
| **Transaction Boundaries**           | Bound to the transaction scope of the batch or transaction in which they are used. | Bound to the transaction scope of the batch or transaction in which they are used. |
| **Indexing**                         | Supports indexing, which can improve performance for certain queries. | Does not support indexes.                               |
| **Constraints**                      | Supports the creation of primary key and unique constraints. | Does not support the creation of constraints.           |
| **Dynamic SQL Usage**                | Can be used with dynamic SQL queries.                    | Cannot be used directly in dynamic SQL queries.         |
| **Statistics and Optimization**      | May benefit from query optimization statistics.         | May benefit from query optimization statistics.         |
| **Explicit Deallocating**            | Requires explicit dropping using the `DROP TABLE` statement. | Automatically deallocated when the scope is exited.      |


## Index
An index is a data structure that provides a quick and efficient way to look up and retrieve data from a database table

### Purpose:
The primary purpose of an index is to speed up the retrieval of rows from a table based on the values in one or more columns.

### Columns:

Indexes can be created on one or more columns of a table. Indexing columns that are frequently used in WHERE clauses or JOIN conditions can significantly improve query performance.

### Benefits:

Improves query performance by reducing the number of data pages that need to be read.

Speeds up sorting and grouping operations.

Enhances the efficiency of JOIN operations.

### Considerations:

While indexes improve read performance, they can slow down write operations (INSERT, UPDATE, DELETE) because the indexes need to be updated whenever the data changes.
Creating too many indexes on a table can consume additional storage space and impact write performance.


Here's a comparison of clustered and non-clustered indexes in tabular format:

| Feature                               | Clustered Index                              | Non-Clustered Index                            |
|---------------------------------------|----------------------------------------------|-----------------------------------------------|
| **Definition**                        | Determines the physical order of data in the table. | Does not affect the physical order of data in the table. |
| **Number per Table**                  | Only one per table.                           | Multiple non-clustered indexes can be created on a table. |
| **Data Storage**                      | Actual data rows are stored at the leaf level of the index structure. | The index structure contains a mapping of index values to the corresponding row identifiers (such as primary key or row address). |
| **Leaf Nodes**                        | The leaf nodes of the index structure are the actual data rows. | The leaf nodes contain the index values and pointers to the actual data rows. |
| **Sorting**                           | Data is physically sorted based on the clustered index key. | No physical sorting of data; creates a separate structure for efficient lookup. |
| **Read Performance**                  | Generally better for range queries and sequential data access. | Efficient for individual lookups and searching, especially for columns not frequently updated. |
| **Write Performance**                 | Write operations (INSERT, UPDATE, DELETE) can be affected, especially for columns in the index key. | Generally imposes less impact on write operations compared to clustered indexes. |
| **Example Syntax**                    | ```sql CREATE CLUSTERED INDEX idx_column ON table (column); ``` | ```sql CREATE NONCLUSTERED INDEX idx_column ON table (column); ``` |


Scenario based:

Let's consider some scenarios and examples to illustrate the considerations between clustered and non-clustered indexes.

### Scenario 1: Frequent Range Queries

**Consideration:**
- If your application frequently performs range queries or sequential data access, a clustered index on the column used in the range queries can improve read performance.

**Example:**
```sql
-- Create a clustered index on the 'timestamp' column for a time-series table
CREATE CLUSTERED INDEX idx_timestamp ON time_series_data (timestamp);
```

### Scenario 2: Primary Key and Frequent Write Operations

**Consideration:**
- If a table has a primary key, creating a clustered index on the primary key column(s) is common. However, for tables with frequent write operations, especially if the primary key is an identity column, consider the impact on performance.

**Example:**
```sql
-- Create a clustered index on the primary key 'employee_id'
CREATE CLUSTERED INDEX idx_employee_id ON employees (employee_id);
```

### Scenario 3: Frequently Updated Columns

**Consideration:**
- If a table has columns that are frequently updated, especially if they are part of a clustered index, consider the impact on write performance and potential fragmentation.

**Example:**
```sql
-- Create a non-clustered index on the 'last_name' column for efficient lookups
CREATE NONCLUSTERED INDEX idx_last_name ON employees (last_name);
```

### Scenario 4: Composite Index for Specific Queries

**Consideration:**
- For specific queries that involve multiple columns, a composite non-clustered index may be beneficial to cover those queries.

**Example:**
```sql
-- Create a composite non-clustered index on 'department_id' and 'salary' for a query that filters by department and sorts by salary
CREATE NONCLUSTERED INDEX idx_department_salary ON employees (department_id, salary);
```

### Scenario 5: Unique Constraint

**Consideration:**
- Both clustered and non-clustered indexes can enforce a unique constraint. Choose based on the characteristics of the table and the impact on query performance.

**Example:**
```sql
-- Create a unique non-clustered index on the 'email' column to enforce uniqueness
CREATE UNIQUE NONCLUSTERED INDEX idx_unique_email ON users (email);
```

### Scenario 6: Tables with Small Size

**Consideration:**
- For small tables or lookup tables, the overhead of maintaining a clustered index may not provide significant benefits. Non-clustered indexes may be sufficient.

**Example:**
```sql
-- Create a non-clustered index on a small reference table
CREATE NONCLUSTERED INDEX idx_reference_code ON reference_table (code);
```

In each scenario, it's important to analyze the specific characteristics of your data and the types of queries performed. Consider the trade-offs between read and write performance, the impact on storage, and the requirements of your application. It's also advisable to use tools like query execution plans and database monitoring to assess the effectiveness of your chosen indexing strategy.


## View

- A view in SQL is a virtual table based on the result-set of an SQL statement. 
- A view contains rows and columns, just like a real table. The fields in a view are fields from one or more real tables in the database.

### Is it possible to run update and delete query on view in sql?

`Yes`, it is possible to run `UPDATE` and `DELETE` queries on a view in SQL under certain conditions. 

### Updateable Views:

For a view to be updateable, it must meet certain criteria:

1. **Single Base Table:**
   - The view must reference only one base table. Joins or subqueries in the view might make it non-updateable.

2. **No Aggregations or Expressions:**
   - The view's SELECT statement should not use aggregate functions or expressions that cannot be easily mapped to the base table's columns.

3. **All Columns Directly Mapped:**
   - The columns being updated in the view must directly correspond to columns in the underlying base table.

4. **No DISTINCT or GROUP BY:**
   - The view should not use `DISTINCT` or `GROUP BY` clauses that make it non-updateable.

### Example of an Updatable View:

```sql
-- Create a base table
CREATE TABLE employees (
   employee_id INT PRIMARY KEY,
   first_name VARCHAR(50),
   last_name VARCHAR(50),
   salary INT
);

-- Insert data into the base table
INSERT INTO employees VALUES (1, 'John', 'Doe', 50000);
INSERT INTO employees VALUES (2, 'Jane', 'Smith', 60000);

-- Create an Updatable view
CREATE VIEW vw_employee_salary AS
SELECT employee_id, first_name, last_name, salary
FROM employees;
```

Now, you can perform updates directly on this view:

```sql
-- Update the salary through the view
UPDATE vw_employee_salary
SET salary = salary + 1000
WHERE employee_id = 1;
```

### Delete Operations on Views:

The conditions for allowing `DELETE` operations on a view are similar to those for `UPDATE`:

1. The view must be based on a single table.
2. The columns being deleted must directly correspond to columns in the underlying table.

### Example of a Delete Operation on a View:

```sql
-- Delete a record through the view
DELETE FROM vw_employee_salary
WHERE employee_id = 2;
```

Additionally, consider using `INSTEAD OF` triggers if you need to customize the logic for updates and deletes on non-Updatable views.

## Materialized view

- materialized views store the actual data on disk.

Key points about materialized views:

1. **Physical Storage:**
   - Materialized views store the actual rows and columns of data on disk, providing quick access to the result set.

2. **Snapshot of Data:**
   - A materialized view is a snapshot of the data at the time of its creation or the last refresh. It reflects the state of the data at a specific point in time.

3. **Query Performance:**
   - Materialized views are used to improve query performance by avoiding the need to recompute the result set of expensive queries on the fly. Instead, the precomputed results are stored in the materialized view.

4. **Periodic Refresh:**
   - Materialized views need to be periodically refreshed to synchronize with changes in the underlying tables. The refresh can be scheduled at specific intervals or triggered based on events.

5. **Read-Only:**
   - Materialized views are typically read-only. Any modifications to the data are done through the underlying tables, and the changes are reflected in the materialized view during the next refresh.

6. **Example:**
   ```sql
   -- Creating a materialized view to store the total salary per department
   CREATE MATERIALIZED VIEW mv_department_salary AS
   SELECT department, SUM(salary) AS total_salary
   FROM employees
   GROUP BY department;
   ```

In summary, a materialized view is a mechanism for storing the results of a query in a physical form to improve query performance. It is particularly useful for scenarios where complex queries or aggregations are frequently performed, and the cost of computation is high. Keep in mind the trade-off between query performance and the need to refresh the materialized view periodically.

### Scenarios

  
### Scenario 1: Sales Data Analysis

#### Requirement:
You have a large dataset of sales transactions, and you need to perform both real-time and aggregated analyses on the data. The dataset includes information about products, customers, and sales amounts.

### Use of Regular View:

#### Scenario:
You want to simplify the querying process and provide a virtual representation of the sales data, allowing users to easily retrieve information about individual transactions.

#### Implementation:
```sql
-- Create a regular view to represent individual sales transactions
CREATE VIEW vw_sales_transaction AS
SELECT transaction_id, product_id, customer_id, sale_amount, sale_date
FROM sales;
```

#### Usage:
Users can now query the `vw_sales_transaction` view to get information about individual sales transactions without directly interacting with the underlying tables.

```sql
-- Example query on the regular view
SELECT * FROM vw_sales_transaction WHERE customer_id = 1001;
```

### Use of Materialized View:

#### Scenario:
You need to provide a quick summary of total sales amounts per product on a daily basis for reporting purposes. The query to calculate this sum is resource-intensive.

#### Implementation:
```sql
-- Create a materialized view to store the total sales amounts per product on a daily basis
CREATE MATERIALIZED VIEW mv_daily_product_sales AS
SELECT product_id, SUM(sale_amount) AS total_sales, DATE_TRUNC('day', sale_date) AS sale_day
FROM sales
GROUP BY product_id, DATE_TRUNC('day', sale_date);
```

#### Usage:
Users can now query the `mv_daily_product_sales` materialized view to get a quick overview of total sales amounts per product on a daily basis.

```sql
-- Example query on the materialized view
SELECT * FROM mv_daily_product_sales WHERE sale_day = '2023-01-15';
```

#### Refresh:
The materialized view needs to be periodically refreshed to keep the aggregated data up to date. This refresh can be scheduled at specific intervals.

```sql
-- Refresh the materialized view
REFRESH MATERIALIZED VIEW mv_daily_product_sales;
```
 
### Scenario 2: Employee Performance Dashboard

#### Requirement:
You are developing an employee performance dashboard for a company. The dashboard should provide real-time information about individual employee performance as well as aggregated statistics for teams.

### Use of Regular View:

#### Scenario:
You want to simplify the querying process and provide a virtual representation of individual employee performance metrics, allowing managers to view details for each employee.

#### Implementation:
```sql
-- Create a regular view to represent individual employee performance
CREATE VIEW vw_employee_performance AS
SELECT employee_id, first_name, last_name, department, project, performance_rating
FROM employee_performance_metrics;
```

#### Usage:
Managers can query the `vw_employee_performance` view to get detailed information about individual employee performance metrics.

```sql
-- Example query on the regular view
SELECT * FROM vw_employee_performance WHERE department = 'Engineering';
```

### Use of Materialized View:

#### Scenario:
You need to provide quick summaries of average performance ratings for each department on a quarterly basis. The calculation of average performance ratings is computationally intensive.

#### Implementation:
```sql
-- Create a materialized view to store the average performance ratings per department on a quarterly basis
CREATE MATERIALIZED VIEW mv_avg_department_performance AS
SELECT department, AVG(performance_rating) AS avg_rating, 
       DATE_TRUNC('quarter', rating_date) AS quarter
FROM employee_performance_metrics
GROUP BY department, DATE_TRUNC('quarter', rating_date);
```

#### Usage:
Executives can query the `mv_avg_department_performance` materialized view to get a quick overview of average performance ratings per department on a quarterly basis.

```sql
-- Example query on the materialized view
SELECT * FROM mv_avg_department_performance WHERE quarter = '2023-01-01';
```

#### Refresh:
The materialized view needs to be periodically refreshed to keep the aggregated data up to date. This refresh can be scheduled at the end of each quarter.

```sql
-- Refresh the materialized view
REFRESH MATERIALIZED VIEW mv_avg_department_performance;
```

### Summary:

- **Regular View:**
  - Used for simplifying queries and providing a virtual representation of individual employee performance metrics.
  - Suitable for real-time access to detailed data for specific employees.

- **Materialized View:**
  - Used for improving query performance by precomputing and storing aggregated results.
  - Suitable for providing quick summaries of performance metrics at a higher level, where periodic refreshes are acceptable.

---

## Procedure and Function

Here's a comparison between stored procedures and functions in SQL in tabular form:

| Feature                             | Stored Procedure                        | Function                                |
|-------------------------------------|----------------------------------------|-----------------------------------------|
| **Return Type**                     | May or may not return a value.        | Must return a single value.             |
| **Usage in SELECT Statements**      | Cannot be used directly in SELECT statements. | Can be used in SELECT statements as part of an expression. |
| **Return Statement**                | Uses `RETURN` statement for status or error handling. | Uses `RETURN` statement to return a value. |
| **Input Parameters**                | Can have input parameters, output parameters, or both. | Can have input parameters and returns a value. |
| **Output Parameters**               | Can have output parameters to return values. | Returns a single value; multiple values can be returned using a result set. |
| **Transaction Control**             | Can include transaction control statements (BEGIN, COMMIT, ROLLBACK). | Generally does not include transaction control statements. |
| **Error Handling**                  | Typically uses TRY...CATCH for error handling. | Typically uses exception handling.      |
| **Example Syntax**                  | ```sql CREATE PROCEDURE sp_example AS BEGIN ... END; ``` | ```sql CREATE FUNCTION fn_example RETURNS INT AS BEGIN ... END; ``` |
| **Execution**                       | Invoked using `EXECUTE` or `EXEC` statement. | Can be called in SELECT statements, as part of expressions, or used in assignments. |
| **Return Multiple Values**          | Can return multiple result sets.       | Returns a single value, but multiple values can be returned using a result set. |
| **Use in JOIN or WHERE Clause**     | Cannot be used directly in JOIN or WHERE clauses. | Can be used directly in JOIN or WHERE clauses if it returns a single value. |
| **Modifying Data**                  | Can modify data using INSERT, UPDATE, DELETE statements. | Generally does not modify data; used for calculations or processing. |

**Note:**
- Stored procedures are often used for performing a sequence of operations, implementing business logic, or modifying data.
- Functions are typically used for calculations or tasks that return a single value, making them suitable for use in SELECT statements or expressions.
 
### Example:
 
### Stored Procedure Example:

Consider a simple stored procedure that retrieves employee information for a given department.

```sql
-- Create a stored procedure
CREATE PROCEDURE GetEmployeesByDepartment
    @DepartmentName NVARCHAR(50)
AS
BEGIN
    -- Select employee information for the specified department
    SELECT EmployeeID, FirstName, LastName, Department
    FROM Employees
    WHERE Department = @DepartmentName;
END;
```

In this example, the stored procedure `GetEmployeesByDepartment` takes a parameter `@DepartmentName` and returns employee information for the specified department.

To execute the stored procedure:

```sql
-- Execute the stored procedure
EXEC GetEmployeesByDepartment @DepartmentName = 'IT';
```

### Function Example:

Now, let's create a function that calculates the total salary for an employee based on their hourly rate and hours worked.

```sql
-- Create a function
CREATE FUNCTION CalculateTotalSalary
    (@HourlyRate DECIMAL(10, 2), @HoursWorked INT)
RETURNS DECIMAL(10, 2)
AS
BEGIN
    -- Calculate total salary
    DECLARE @TotalSalary DECIMAL(10, 2);
    SET @TotalSalary = @HourlyRate * @HoursWorked;

    -- Return the calculated total salary
    RETURN @TotalSalary;
END;
```

In this example, the function `CalculateTotalSalary` takes two parameters, `@HourlyRate` and `@HoursWorked`, and returns the calculated total salary.

To use the function in a SELECT statement:

```sql
-- Use the function in a SELECT statement
SELECT EmployeeID, FirstName, LastName,
       HourlyRate, HoursWorked,
       dbo.CalculateTotalSalary(HourlyRate, HoursWorked) AS TotalSalary
FROM Employees;
```

In this SELECT statement, the function `CalculateTotalSalary` is used to calculate the total salary for each employee based on their hourly rate and hours worked.

---
