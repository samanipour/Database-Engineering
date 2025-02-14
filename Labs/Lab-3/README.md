# **Retrieving Data**

## **Objective**
This manual will guide you through using the `SELECT` statement to retrieve data from a MySQL database, including retrieving individual columns, multiple columns, all columns, distinct rows, and limiting results.

---

## **Step 1: Using the SELECT Statement**
1. **What is the SELECT Statement?**
   - The `SELECT` statement is used to **retrieve information** from one or more tables in a database.
   - It is one of the most frequently used SQL statements.

2. **Basic Syntax**
   ```sql
   SELECT column_name
   FROM table_name;
   ```
   - `SELECT`: Specifies which columns to retrieve.
   - `FROM`: Specifies the table from which to retrieve the data.

3. **Example**
   ```sql
   SELECT prod_name
   FROM products;
   ```
   - This retrieves the `prod_name` column from the `products` table.

---

## **Step 2: Retrieving Individual Columns**
1. **Purpose**
   - Retrieve a **single column** of data from a table.

2. **Syntax**
   ```sql
   SELECT column_name
   FROM table_name;
   ```

3. **Example**
   ```sql
   SELECT prod_name
   FROM products;
   ```
   - This retrieves all product names from the `products` table.

4. **Output Example**
   ```
   +------------------+
   | prod_name         |
   +------------------+
   | .5 ton anvil      |
   | 1 ton anvil       |
   | 2 ton anvil       |
   | Oil can           |
   | Fuses             |
   | Sling             |
   | TNT (1 stick)     |
   | TNT (5 sticks)    |
   | Bird seed         |
   | Carrots           |
   | Safe              |
   | Detonator         |
   | JetPack 1000      |
   | JetPack 2000      |
   +------------------+
   ```

---

## **Step 3: Retrieving Multiple Columns**
1. **Purpose**
   - Retrieve **multiple columns** from a table.

2. **Syntax**
   ```sql
   SELECT column1, column2, column3
   FROM table_name;
   ```
   - Column names are separated by **commas**.

3. **Example**
   ```sql
   SELECT prod_id, prod_name, prod_price
   FROM products;
   ```

4. **Output Example**
   ```
   +---------+----------------+------------+
   | prod_id | prod_name      | prod_price |
   +---------+----------------+------------+
   | ANV01   | .5 ton anvil   | 5.99       |
   | ANV02   | 1 ton anvil    | 9.99       |
   | ANV03   | 2 ton anvil    | 14.99      |
   | OL1     | Oil can        | 8.99       |
   +---------+----------------+------------+
   ```

5. **Tip: Care with Commas**
   - Be sure to include a comma **after each column name except the last one**.
   - Adding a comma after the last column name will generate an error.

---

## **Step 4: Retrieving All Columns**
1. **Purpose**
   - Retrieve **all columns** from a table without listing them individually.

2. **Syntax**
   ```sql
   SELECT *
   FROM table_name;
   ```
   - `*` is a **wildcard** that selects all columns.

3. **Example**
   ```sql
   SELECT *
   FROM products;
   ```

4. **Output Example**
   ```
   +---------+----------------+------------+
   | prod_id | prod_name      | prod_price |
   +---------+----------------+------------+
   | ANV01   | .5 ton anvil   | 5.99       |
   | ANV02   | 1 ton anvil    | 9.99       |
   | ANV03   | 2 ton anvil    | 14.99      |
   | OL1     | Oil can        | 8.99       |
   +---------+----------------+------------+
   ```

5. **Caution: Using Wildcards**
   - Avoid using `*` unless every column is needed.
   - Retrieving unnecessary columns can **slow down performance**.

---

## **Step 5: Retrieving Distinct Rows**
1. **Purpose**
   - Retrieve **unique values** from a column, eliminating duplicate rows.

2. **Syntax**
   ```sql
   SELECT DISTINCT column_name
   FROM table_name;
   ```

3. **Example**
   ```sql
   SELECT DISTINCT vend_id
   FROM products;
   ```

4. **Output Example**
   ```
   +---------+
   | vend_id |
   +---------+
   | 1001    |
   | 1002    |
   | 1003    |
   | 1005    |
   +---------+
   ```

5. **Caution: DISTINCT Applies to All Columns**
   - `DISTINCT` affects **all columns** in the `SELECT` statement, not just the one it precedes.
   - Example:
     ```sql
     SELECT DISTINCT vend_id, prod_price
     FROM products;
     ```
     - Rows are distinct only if **both `vend_id` and `prod_price`** are unique.

---

## **Step 6: Limiting Results**
1. **Purpose**
   - **Limit the number of rows** returned by a query.

2. **Syntax**
   ```sql
   SELECT column_name
   FROM table_name
   LIMIT number_of_rows;
   ```

3. **Example: Limiting Rows**
   ```sql
   SELECT prod_name
   FROM products
   LIMIT 5;
   ```

4. **Output Example**
   ```
   +-----------------+
   | prod_name        |
   +-----------------+
   | .5 ton anvil     |
   | 1 ton anvil      |
   | 2 ton anvil      |
   | Oil can          |
   | Fuses            |
   +-----------------+
   ```

5. **Example: Pagination with LIMIT**
   ```sql
   SELECT prod_name
   FROM products
   LIMIT 5, 5;
   ```
   - `LIMIT 5, 5` returns **5 rows** starting from the **5th row**.

6. **Caution: Row Counting Starts at 0**
   - Rows are numbered starting at **0**.
   - `LIMIT 1, 1` returns the **second row**, not the first.

---

## **Step 7: Hands-on Exercises**
### **Objective:**
- Practice using `SELECT` to retrieve data in various ways.

### **Tasks:**
1. **Retrieve Individual Column**
   ```sql
   SELECT prod_name
   FROM products;
   ```
2. **Retrieve Multiple Columns**
   ```sql
   SELECT prod_id, prod_name, prod_price
   FROM products;
   ```
3. **Retrieve All Columns**
   ```sql
   SELECT *
   FROM products;
   ```
4. **Retrieve Unique Values**
   ```sql
   SELECT DISTINCT vend_id
   FROM products;
   ```
5. **Limit the Number of Rows**
   ```sql
   SELECT prod_name
   FROM products
   LIMIT 3;
   ```

---

## **Conclusion**
By following this manual, students will learn:
- How to retrieve individual and multiple columns using `SELECT`.
- How to retrieve all columns using the wildcard `*`.
- How to retrieve distinct rows using `DISTINCT`.
- How to limit the number of rows using `LIMIT`.