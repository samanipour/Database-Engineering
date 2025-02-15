# **Creating and Manipulating Tables**

## **Objective**
This manual will guide students through **creating and manipulating tables** in MySQL, including:
- Creating tables with various data types and constraints.
- Updating and deleting tables.
- Using `AUTO_INCREMENT` and `DEFAULT` values.
- Managing `NULL` values and primary keys.

---

## **Step 1: Creating Tables**
1. **Purpose**
   - `CREATE TABLE` is used to **define a new table** with specified columns and data types.

2. **Basic Syntax**
   ```sql
   CREATE TABLE table_name (
       column_name1 data_type constraints,
       column_name2 data_type constraints,
       ...
   );
   ```
   - `table_name`: Name of the new table.
   - `column_name`: Name of the column.
   - `data_type`: Data type for the column (e.g., `INT`, `VARCHAR`, `DATE`).
   - `constraints`: Optional constraints (e.g., `NOT NULL`, `UNIQUE`).

3. **Example: Creating a Customers Table**
   ```sql
   CREATE TABLE customers (
       cust_id INT NOT NULL AUTO_INCREMENT,
       cust_name VARCHAR(50) NOT NULL,
       cust_address VARCHAR(100),
       cust_city VARCHAR(50),
       cust_state CHAR(2),
       cust_zip CHAR(10),
       cust_country VARCHAR(50) NOT NULL DEFAULT 'USA',
       PRIMARY KEY (cust_id)
   );
   ```
   - `cust_id` is an `INT` and **auto-incremented**.
   - `cust_name` cannot be `NULL`.
   - `cust_country` has a **default value** of `'USA'`.
   - `PRIMARY KEY` uniquely identifies each row.

4. **Output Example**
   ```
   Query OK, 0 rows affected (0.02 sec)
   ```

5. **Tip: Checking Table Structure**
   - To **verify the table structure**, use:
     ```sql
     DESCRIBE customers;
     ```
     - This shows the column names, data types, and constraints.

---

## **Step 2: Working with NULL Values**
1. **Purpose**
   - `NULL` represents a **missing or unknown value**.
   - By default, columns are **nullable** unless specified as `NOT NULL`.

2. **Example: Allowing and Restricting NULL Values**
   ```sql
   CREATE TABLE products (
       prod_id INT NOT NULL AUTO_INCREMENT,
       prod_name VARCHAR(100) NOT NULL,
       prod_desc TEXT,
       prod_price DECIMAL(10,2) NOT NULL,
       PRIMARY KEY (prod_id)
   );
   ```
   - `prod_desc` can be `NULL`.
   - `prod_name` and `prod_price` **cannot** be `NULL`.

3. **Inserting NULL Values**
   ```sql
   INSERT INTO products (prod_name, prod_price)
   VALUES ('Sample Product', 19.99);
   ```
   - `prod_desc` is **automatically set** to `NULL`.

4. **Tip: Checking for NULL Values**
   - Use `IS NULL` or `IS NOT NULL` to check for `NULL` values:
     ```sql
     SELECT prod_name
     FROM products
     WHERE prod_desc IS NULL;
     ```

---

## **Step 3: Using PRIMARY KEY and AUTO_INCREMENT**
1. **Purpose**
   - `PRIMARY KEY` **uniquely identifies** each row in a table.
   - `AUTO_INCREMENT` **automatically generates** a unique value for each new row.

2. **Example: Using PRIMARY KEY and AUTO_INCREMENT**
   ```sql
   CREATE TABLE orders (
       order_id INT NOT NULL AUTO_INCREMENT,
       order_date DATE NOT NULL,
       cust_id INT NOT NULL,
       PRIMARY KEY (order_id)
   );
   ```
   - `order_id` is the **primary key** and is **auto-incremented**.

3. **Inserting Data with AUTO_INCREMENT**
   ```sql
   INSERT INTO orders (order_date, cust_id)
   VALUES ('2025-02-14', 1001);
   ```
   - `order_id` is **automatically incremented** by MySQL.

4. **Output Example**
   ```
   Query OK, 1 row affected (0.01 sec)
   ```

5. **Tip: Getting the Last AUTO_INCREMENT Value**
   - Use `LAST_INSERT_ID()` to **retrieve the last auto-incremented value**:
     ```sql
     SELECT LAST_INSERT_ID();
     ```

---

## **Step 4: Specifying Default Values**
1. **Purpose**
   - `DEFAULT` is used to **set a default value** for a column if no value is provided.

2. **Example: Using DEFAULT Values**
   ```sql
   CREATE TABLE products (
       prod_id INT NOT NULL AUTO_INCREMENT,
       prod_name VARCHAR(100) NOT NULL,
       prod_price DECIMAL(10,2) NOT NULL DEFAULT 9.99,
       prod_status CHAR(1) NOT NULL DEFAULT 'A',
       PRIMARY KEY (prod_id)
   );
   ```
   - `prod_price` defaults to `9.99`.
   - `prod_status` defaults to `'A'`.

3. **Inserting Data with DEFAULT Values**
   ```sql
   INSERT INTO products (prod_name)
   VALUES ('New Product');
   ```
   - `prod_price` is set to `9.99` and `prod_status` to `'A'`.

4. **Output Example**
   ```
   Query OK, 1 row affected (0.01 sec)
   ```

5. **Tip: Viewing Default Values**
   - Use `SHOW COLUMNS` to **view the default values**:
     ```sql
     SHOW COLUMNS FROM products;
     ```

---

## **Step 5: Updating Tables**
1. **Purpose**
   - `ALTER TABLE` is used to **modify the structure** of an existing table.

2. **Example: Adding a New Column**
   ```sql
   ALTER TABLE products
   ADD COLUMN prod_inventory INT DEFAULT 0;
   ```
   - Adds a new column `prod_inventory` with a **default value of `0`**.

3. **Example: Modifying an Existing Column**
   ```sql
   ALTER TABLE products
   MODIFY COLUMN prod_status CHAR(1) DEFAULT 'I';
   ```
   - Changes the **default value** of `prod_status` to `'I'`.

4. **Example: Dropping a Column**
   ```sql
   ALTER TABLE products
   DROP COLUMN prod_inventory;
   ```
   - **Removes** the `prod_inventory` column.

5. **Tip: Renaming a Table**
   - Use `RENAME TABLE` to **rename an existing table**:
     ```sql
     RENAME TABLE products TO product_catalog;
     ```

---

## **Step 6: Deleting Tables**
1. **Purpose**
   - `DROP TABLE` **deletes a table and all its data** permanently.

2. **Example: Dropping a Table**
   ```sql
   DROP TABLE product_catalog;
   ```
   - **Deletes** the `product_catalog` table.

3. **Tip: Deleting All Table Data Without Dropping Structure**
   - Use `TRUNCATE TABLE` to **delete all rows** but keep the table structure:
     ```sql
     TRUNCATE TABLE products;
     ```

---

## **Step 7: Hands-on Exercises**
### **Objective:**
- Practice creating, updating, and deleting tables.

### **Tasks:**
1. **Create a Customers Table**
   ```sql
   CREATE TABLE customers (
       cust_id INT NOT NULL AUTO_INCREMENT,
       cust_name VARCHAR(50) NOT NULL,
       cust_email VARCHAR(100),
       PRIMARY KEY (cust_id)
   );
   ```
2. **Add a New Column to Orders Table**
   ```sql
   ALTER TABLE orders
   ADD COLUMN order_status CHAR(1) DEFAULT 'P';
   ```
3. **Delete the Orders Table**
   ```sql
   DROP TABLE orders;
   ```

---

## **Conclusion**
By following this manual, you will learn:
- How to create tables with different data types and constraints.
- How to manage `NULL` values, `PRIMARY KEY`, `AUTO_INCREMENT`, and `DEFAULT` values.
- How to update and delete tables.