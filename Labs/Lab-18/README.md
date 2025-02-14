# **Inserting Data**

## **Objective**
This manual will guide you through **inserting data** into MySQL tables using the `INSERT` statement. It covers:
- Inserting complete rows
- Inserting partial rows
- Inserting multiple rows
- Inserting data retrieved from another query

---

## **Step 1: Understanding Data Insertion**
1. **What Is Data Insertion?**
   - `INSERT` is used to **add new rows** to a table in a database.
   - It is one of the most frequently used statements in MySQL, along with `SELECT`, `UPDATE`, and `DELETE`.

2. **Ways to Insert Data:**
   - **Complete Rows**: Inserting values for all columns.
   - **Partial Rows**: Inserting values for specific columns.
   - **Multiple Rows**: Inserting several rows at once.
   - **Retrieved Data**: Inserting the result of a `SELECT` query into a table.

3. **Security Note:**
   - The use of `INSERT` can be **restricted** per table or user using MySQL security settings.

---

## **Step 2: Inserting Complete Rows**
1. **Purpose**
   - This method **inserts a new row** by providing values for **all columns** in the table.

2. **Syntax**
   ```sql
   INSERT INTO table_name
   VALUES (value1, value2, ..., valueN);
   ```
   - The number of values **must match** the number of columns in the table.
   - The order of values **must match** the column order in the table definition.

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
   - `cust_id` is `NULL` because it is **auto-incremented**.
   - `cust_contact` and `cust_email` are `NULL` because **no values** are provided.

4. **Tip: Avoid Using This Method**
   - This method is **not recommended** because it depends on the **order of columns** in the table.
   - If the table structure changes, the query **may fail** or **insert incorrect data**.

---

## **Step 3: Inserting Complete Rows with Explicit Column Names**
1. **Purpose**
   - This method **specifies column names** to ensure the data is inserted into the **correct columns**.

2. **Syntax**
   ```sql
   INSERT INTO table_name (column1, column2, ..., columnN)
   VALUES (value1, value2, ..., valueN);
   ```
   - Column names are listed in **parentheses**.
   - The number of columns must **match the number of values**.

3. **Example: Inserting with Column Names**
   ```sql
   INSERT INTO customers (cust_name,
                          cust_address,
                          cust_city,
                          cust_state,
                          cust_zip,
                          cust_country,
                          cust_contact,
                          cust_email)
   VALUES ('Pep E. LaPew',
           '100 Main Street',
           'Los Angeles',
           'CA',
           '90046',
           'USA',
           NULL,
           NULL);
   ```
   - Column names are explicitly listed.
   - This method is **safer** because it is **not affected** by column order changes in the table.

4. **Tip: Always Use Explicit Column Names**
   - It increases the **maintainability** of the query.
   - It reduces the risk of **data misalignment** if the table structure changes.

---

## **Step 4: Inserting Partial Rows**
1. **Purpose**
   - Allows inserting **only some columns** while leaving others as `NULL` or using **default values**.

2. **Syntax**
   ```sql
   INSERT INTO table_name (column1, column2)
   VALUES (value1, value2);
   ```
   - Only the **specified columns** are populated.
   - The other columns are set to:
     - `NULL` (if allowed), or
     - Default values (if defined in the table schema).

3. **Example: Inserting Partial Row**
   ```sql
   INSERT INTO customers (cust_name, cust_email)
   VALUES ('M. Martian', 'martian@example.com');
   ```
   - Only `cust_name` and `cust_email` are provided.
   - Other columns are set to `NULL` or their default values.

4. **Tip: Omitting Columns**
   - You can **omit columns** if:
     - The column is defined as **allowing NULL** values.
     - The column has a **default value**.
   - If the column **does not allow NULL** and **no default** is specified, MySQL will **throw an error**.

---

## **Step 5: Inserting Multiple Rows**
1. **Purpose**
   - Insert **multiple rows** in a single `INSERT` statement for **better performance**.

2. **Syntax**
   ```sql
   INSERT INTO table_name (column1, column2)
   VALUES (value1, value2),
          (value3, value4),
          ...;
   ```
   - Each set of values is **enclosed in parentheses** and **separated by commas**.

3. **Example: Inserting Multiple Rows**
   ```sql
   INSERT INTO customers (cust_name, cust_address, cust_city, cust_state, cust_zip, cust_country)
   VALUES ('Pep E. LaPew', '100 Main Street', 'Los Angeles', 'CA', '90046', 'USA'),
          ('M. Martian', '42 Galaxy Way', 'New York', 'NY', '11213', 'USA');
   ```
   - Two rows are inserted in a **single query**.

4. **Tip: Performance Improvement**
   - This technique improves performance because MySQL processes **multiple rows at once**.

---

## **Step 6: Inserting Data Retrieved from Another Query**
1. **Purpose**
   - Insert data **retrieved by a `SELECT` statement** into another table.

2. **Syntax**
   ```sql
   INSERT INTO target_table (column1, column2)
   SELECT column1, column2
   FROM source_table
   WHERE condition;
   ```
   - Combines `INSERT` and `SELECT` into one query.

3. **Example: Inserting Retrieved Data**
   ```sql
   INSERT INTO customers (cust_id, cust_contact, cust_email, cust_name, cust_address, cust_city, cust_state, cust_zip, cust_country)
   SELECT cust_id, cust_contact, cust_email, cust_name, cust_address, cust_city, cust_state, cust_zip, cust_country
   FROM custnew;
   ```
   - Inserts data from `custnew` table into `customers` table.

4. **Tip: Matching Column Positions**
   - Column names do **not** need to match, but the **column positions** must **align**.

---

## **Step 7: Hands-on Exercises**
### **Objective:**
- Practice inserting data using the different methods covered.

### **Tasks:**
1. **Insert a Complete Row**
   ```sql
   INSERT INTO customers
   VALUES (NULL, 'John Doe', '123 Elm Street', 'Chicago', 'IL', '60614', 'USA', NULL, NULL);
   ```
2. **Insert a Partial Row**
   ```sql
   INSERT INTO customers (cust_name, cust_email)
   VALUES ('Jane Doe', 'jane@example.com');
   ```
3. **Insert Multiple Rows**
   ```sql
   INSERT INTO customers (cust_name, cust_address, cust_city, cust_state, cust_zip, cust_country)
   VALUES ('Alice', '456 Oak Street', 'Austin', 'TX', '78701', 'USA'),
          ('Bob', '789 Pine Street', 'Denver', 'CO', '80203', 'USA');
   ```
4. **Insert Retrieved Data**
   ```sql
   INSERT INTO customers (cust_name, cust_email)
   SELECT cust_name, cust_email
   FROM custnew;
   ```

---

## **Conclusion**
By following this manual, students will learn:
- How to use `INSERT` to add new rows to tables.
- How to insert complete, partial, and multiple rows.
- How to insert data retrieved from another query.