# **Understanding SQL**

## **Objective**
This manual will guide you through the foundational concepts of SQL and databases, ensuring they can understand and interact with MySQL databases.

---

## **Step 1: Understanding Databases**
1. **What is a Database?**
   - A **database** is a structured collection of data stored systematically.
   - Think of it as a **digital filing cabinet** where data is organized for easy retrieval.

2. **Common Uses of Databases**
   - Contact lists on phones.
   - Email address books.
   - Online shopping order history.
   - ATM transaction processing.

3. **Database Management System (DBMS)**
   - A **DBMS** is software that allows you to create, manage, and manipulate databases.
   - Examples: MySQL, PostgreSQL, Microsoft SQL Server.

---

## **Step 2: Understanding Database Structure**
1. **Tables**
   - A **table** is a structured list of related data.
   - Example: A "Customers" table might store customer names, emails, and phone numbers.

2. **Columns and Datatypes**
   - A **column** represents a specific type of data (e.g., "Name" column stores names).
   - Each column has a **datatype**, such as:
     - `VARCHAR(50)` for text data.
     - `INT` for numerical data.
     - `DATE` for date values.

3. **Rows**
   - Each entry in a table is a **row**, also called a **record**.
   - Example: A row in the "Customers" table might contain:
     ```
     | ID | Name  | Email          |
     |----|-------|----------------|
     | 1  | John  | john@email.com |
     ```

4. **Primary Keys**
   - A **primary key** is a column (or combination of columns) that uniquely identifies each row.
   - Example: In a "Students" table, the "Student_ID" column can be the primary key.

---

## **Step 3: Understanding SQL**
1. **What is SQL?**
   - **Structured Query Language (SQL)** is used to communicate with databases.
   - SQL helps in retrieving, inserting, updating, and deleting data.

2. **Why Use SQL?**
   - It is a **universal language** supported by most database systems.
   - SQL is **simple yet powerful** for handling data.

---

## **Step 4: Hands-on Practice**
### **Try It Yourself**
1. **Checking MySQL Installation**
   - Open **MySQL Command Line** or **MySQL Workbench**.
   - Type the following command to check if MySQL is running:
     ```sql
     SELECT VERSION();
     ```

2. **Creating a Sample Database**
   - Use the following command to create a new database:
     ```sql
     CREATE DATABASE DB_Lab;
     ```
   - Switch to the new database:
     ```sql
     USE DB_Lab;
     ```

3. **Creating a Table**
   - Create a "Students" table:
     ```sql
     CREATE TABLE Students (
         Student_ID INT PRIMARY KEY,
         Name VARCHAR(50),
         Email VARCHAR(100)
     );
     ```

4. **Inserting Data into the Table**
   - Add a record to the "Students" table:
     ```sql
     INSERT INTO Students (Student_ID, Name, Email) 
     VALUES (1, 'Alice', 'alice@email.com');
     ```

5. **Retrieving Data**
   - Fetch all records from the "Students" table:
     ```sql
     SELECT * FROM Students;
     ```

---

## **Conclusion**
By following this manual, you will understand the basic concepts of databases and SQL. They should now be able to:
- Define what a database is.
- Identify key database components (tables, columns, rows, primary keys).
- Understand SQL as a language.
- Perform basic SQL operations in MySQL.
