# **Inserting Data**

## **Objective**
This manual will guide you through **inserting data** into MySQL tables using the `INSERT` statement. Students will learn how to insert complete rows, insert multiple rows, and use `INSERT SELECT` to insert data retrieved from other tables.

---

## **Step 1: Understanding Data Insertion**
1. **What Is Data Insertion?**
   - **Inserting data** involves adding **new rows** to a table.
   - This is done using the `INSERT` statement.

2. **Types of Insertion**
   - **Complete Row Insertion**: Inserting values for **all columns**.
   - **Partial Row Insertion**: Inserting values for **some columns**.
   - **Multiple Row Insertion**: Inserting **multiple rows** in one statement.
   - **Inserting Retrieved Data**: Using `INSERT SELECT` to insert **query results** into a table.

3. **When to Use Data Insertion?**
   - When **adding new records** to a database, such as:
     - **Customer information** into a `customers` table.
     - **New products** into an `inventory` table.
     - **Order details** into an `orders` table.

---

## **Step 2: Inserting Complete Rows**
1. **Purpose**
   - Insert data for **all columns** in a table.

2. **Syntax**
   ```sql
   INSERT INTO table_name
   VALUES (value1, value2, ..., valueN);
   ```
   - Values **must match the order** of the columns in the table definition.

3. **Example: Inserting a Complete Row**
   ```sql
   INSERT INTO customers
   VALUES (NULL,
           'Pep E. LaPew',
           '100 Main Street',
           'Los Angeles',
           'CA',
           '90046',
           'USA',
           NULL,
           NULL);
   ```
   - `NULL` is used for columns that allow it.
   - MySQL **auto-increments** `cust_id` because it is set as an `AUTO_INCREMENT` column.

4. **Output**
   - `INSERT` statements **do not generate output**.
   - Use `SELECT` to **verify the insertion**:
     ```sql
     SELECT * FROM customers WHERE cust_name = 'Pep E. LaPew';
     ```

5. **Tip: Specifying Column Names**
   - It is **safer** to specify column names to **avoid dependency** on the column order:
     ```sql
     INSERT INTO customers (cust_name, cust_address, cust_city, cust_state, cust_zip, cust_country)
     VALUES ('Pep E. LaPew', '100 Main Street', 'Los Angeles', 'CA', '90046', 'USA');
     ```

---

## **Step 3: Inserting Partial Rows**
1. **Purpose**
   - Insert data for **some columns** while leaving others as `NULL` or with default values.

2. **Syntax**
   ```sql
   INSERT INTO table_name (column1, column2, ..., columnN)
   VALUES (value1, value2, ..., valueN);
   ```
   - Only **specified columns** receive values.
   - Other columns get **default values** or `NULL`.

3. **Example: Inserting a Partial Row**
   ```sql
   INSERT INTO customers (cust_name, cust_address, cust_city, cust_country)
   VALUES ('Wile E. Coyote', 'Desert Road', 'Albuquerque', 'USA');
   ```
   - Only 4 columns are specified.
   - `cust_id` is auto-incremented, and other unspecified columns receive `NULL` or default values.

4. **Output**
   - Use `SELECT` to check the new record:
     ```sql
     SELECT * FROM customers WHERE cust_name = 'Wile E. Coyote';
     ```

5. **Tip: Omitting Columns**
   - You can omit columns if:
     - They allow `NULL` values.
     - They have **default values** defined.

---

## **Step 4: Inserting Multiple Rows**
1. **Purpose**
   - Insert **multiple rows** in one `INSERT` statement.

2. **Why Use Multiple Row Insertion?**
   - It **improves performance** by reducing database operations.

3. **Syntax**
   ```sql
   INSERT INTO table_name (column1, column2, ..., columnN)
   VALUES (value1a, value2a, ..., valueNa),
          (value1b, value2b, ..., valueNb),
          ...;
   ```
   - Each row is enclosed in parentheses and **separated by commas**.

4. **Example: Inserting Multiple Rows**
   ```sql
   INSERT INTO customers (cust_name, cust_address, cust_city, cust_state, cust_zip, cust_country)
   VALUES ('Pep E. LaPew', '100 Main Street', 'Los Angeles', 'CA', '90046', 'USA'),
          ('M. Martian', '42 Galaxy Way', 'New York', 'NY', '11213', 'USA');
   ```
   - Inserts **two new customers** in a single statement.

5. **Output**
   - Use `SELECT` to verify:
     ```sql
     SELECT * FROM customers WHERE cust_name IN ('Pep E. LaPew', 'M. Martian');
     ```

6. **Tip: Performance Improvement**
   - Inserting multiple rows in one statement is **faster** than using multiple `INSERT` statements.

---

## **Step 5: Inserting Retrieved Data (INSERT SELECT)**
1. **Purpose**
   - Insert the **results of a query** into another table.

2. **Syntax**
   ```sql
   INSERT INTO table_name (column1, column2, ..., columnN)
   SELECT column1, column2, ..., columnN
   FROM source_table
   WHERE condition;
   ```
   - Combines an `INSERT` statement with a `SELECT` query.

3. **Example: Copying Data from Another Table**
   ```sql
   INSERT INTO customers (cust_name, cust_address, cust_city, cust_state, cust_zip, cust_country)
   SELECT cust_name, cust_address, cust_city, cust_state, cust_zip, cust_country
   FROM custnew;
   ```
   - Copies customer data from `custnew` table into the `customers` table.

4. **Output**
   - Use `SELECT` to verify the inserted data:
     ```sql
     SELECT * FROM customers;
     ```

5. **Tip: Avoiding Duplicate Primary Keys**
   - Ensure no **duplicate primary keys** are inserted.
   - Omit auto-increment columns if needed.

---

## **Step 6: Hands-on Exercises**
### **Objective:**
- Practice using `INSERT` statements to add data to tables.

### **Tasks:**
1. **Insert a Complete Row**
   ```sql
   INSERT INTO customers (cust_name, cust_address, cust_city, cust_country)
   VALUES ('Road Runner', 'Speedway', 'Tucson', 'USA');
   ```
2. **Insert Multiple Rows**
   ```sql
   INSERT INTO customers (cust_name, cust_address, cust_city, cust_country)
   VALUES ('Daffy Duck', 'Duck Pond', 'Orlando', 'USA'),
          ('Porky Pig', 'Farm Road', 'Dallas', 'USA');
   ```
3. **Insert Retrieved Data**
   ```sql
   INSERT INTO customers (cust_name, cust_address, cust_city, cust_country)
   SELECT cust_name, cust_address, cust_city, cust_country
   FROM custnew WHERE cust_country = 'USA';
   ```

---

## **Conclusion**
By following this manual, students will learn:
- How to insert complete and partial rows.
- How to insert multiple rows efficiently.
- How to use `INSERT SELECT` to copy data from another table.