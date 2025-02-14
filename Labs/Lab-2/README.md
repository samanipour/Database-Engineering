# **Working with MySQL**

## **Objective**
This manual will guide you through the process of connecting to MySQL, selecting databases, and obtaining information about databases and tables using both the command-line utility and MySQL Workbench.

---

## **Step 1: Connecting and Logging In to MySQL**
1. **Understanding MySQL Authentication**
   - MySQL requires users to **log in** before issuing any commands.
   - Each user has a **username** and **password** stored internally in MySQL with associated privileges.
   - **Administrative Account:** Often named `root`, this account has full rights to manage databases, users, and more.
   - **Caution:** Do not use the `root` account for day-to-day tasks in real-world environments due to its extensive privileges.

2. **Requirements for Connecting to MySQL**
   - **Hostname**: Usually `localhost` for local installations.
   - **Port**: Default is `3306`.
   - **Username**: The MySQL username (e.g., `root`).
   - **Password**: Password for the specified username.

3. **Connecting Using Command-Line Tool**
   - Open **Command Prompt** (Windows) or **Terminal** (macOS/Linux).
   - To connect as `root`, use:
     ```bash
     mysql --user=root --password
     ```
     - If a different username is used, replace `root` with that username.
     - You will be prompted for the password. If successful, the following prompt will appear:
     ```
     Welcome to the MySQL monitor.  Commands end with ; or \g.
     mysql>
     ```
   - **Exiting the Command Line:**
     ```sql
     quit;
     ```
     or
     ```sql
     exit;
     ```

4. **Connecting Using MySQL Workbench**
   - Open **MySQL Workbench**.
   - Click on the connection name (e.g., **Local instance MySQL**).
   - Enter the **username** and **password**.
   - Click **OK** to connect.
   - If successful, you will see the MySQL Workbench user interface with the **Navigator** on the left.

---

## **Step 2: Selecting a Database**
1. **Why Select a Database?**
   - When first connected, no database is selected.
   - You must select a database before performing any operations on it.

2. **Using Command-Line Tool**
   - To select a database, use the `USE` statement:
     ```sql
     USE database_name;
     ```
     Example:
     ```sql
     USE mysql;
     ```
     - Output:
     ```
     Database changed
     ```
   - **Note:** The `USE` statement does not return query results but shows a confirmation message.

3. **Using MySQL Workbench**
   - In the **Navigator** panel, expand **Schemas** to see the list of available databases.
   - Double-click the database name to select it.
     - The database name becomes **bold** to indicate it is selected.
   - **Note:** Double-clicking in Workbench runs the `USE` statement automatically.

---

## **Step 3: Learning About Databases and Tables**
1. **Viewing Available Databases**
   - Use the `SHOW DATABASES;` command to list all databases:
     ```sql
     SHOW DATABASES;
     ```
     - Output Example:
     ```
     +--------------------+
     | Database           |
     +--------------------+
     | information_schema |
     | mysql              |
     | performance_schema |
     | sys                |
     +--------------------+
     ```

2. **Viewing Tables in a Database**
   - After selecting a database, list its tables using:
     ```sql
     SHOW TABLES;
     ```
     - Example Output:
     ```
     +-----------------+
     | Tables_in_mysql |
     +-----------------+
     | user            |
     | db              |
     | tables_priv     |
     +-----------------+
     ```

3. **Viewing Table Structure**
   - To see the columns and structure of a table, use:
     ```sql
     SHOW COLUMNS FROM table_name;
     ```
     Example:
     ```sql
     SHOW COLUMNS FROM user;
     ```
     - Alternatively, use `DESCRIBE`:
     ```sql
     DESCRIBE user;
     ```
     - Output Example:
     ```
     +-----------+--------------+------+-----+---------+-------+
     | Field     | Type         | Null | Key | Default | Extra |
     +-----------+--------------+------+-----+---------+-------+
     | Host      | char(60)     | NO   | PRI |         |       |
     | User      | char(32)     | NO   | PRI |         |       |
     | Password  | char(41)     | NO   |     |         |       |
     +-----------+--------------+------+-----+---------+-------+
     ```

---

## **Step 4: Executing SQL Statements in MySQL Workbench**
1. **Opening the Query Editor**
   - In **MySQL Workbench**, click on the **New Query** button (top left).
   - A new editor window will open for writing SQL commands.

2. **Writing and Running SQL Statements**
   - Type your SQL query in the editor. For example:
     ```sql
     SELECT * FROM user;
     ```
   - Click on the **Execute** button (yellow lightning bolt) to run the query.
   - The results will be displayed in a grid below the query editor.

---

## **Step 5: Hands-on Exercises**
### **Objective:**
- Practice using both **Command-Line Tool** and **MySQL Workbench** to perform basic database operations.

### **Tasks:**
1. **List all databases.**
   ```sql
   SHOW DATABASES;
   ```
2. **Select the `mysql` database.**
   ```sql
   USE mysql;
   ```
3. **List all tables in the `mysql` database.**
   ```sql
   SHOW TABLES;
   ```
4. **View the structure of the `user` table.**
   ```sql
   DESCRIBE user;
   ```
5. **Retrieve all records from the `user` table.**
   ```sql
   SELECT * FROM user;
   ```

---

## **Conclusion**
By following this manual, you will learn:
- How to connect and log in to MySQL using both the command-line and MySQL Workbench.
- How to select databases and retrieve information about tables.
- How to write and execute basic SQL commands.