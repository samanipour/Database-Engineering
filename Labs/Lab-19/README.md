# **Updating and Deleting**

## **Objective**
This manual will guide you through **updating and deleting data** in MySQL using the `UPDATE` and `DELETE` statements. It includes updating specific rows, updating multiple columns, and safely deleting rows while avoiding accidental data loss.

---

## **Step 1: Understanding Data Modification**
1. **Why Update and Delete Data?**
   - **Updating** allows modifying existing records without adding new rows.
   - **Deleting** removes data that is no longer needed, keeping the database clean.

2. **Important Considerations**
   - Be **careful with `UPDATE` and `DELETE` statements**, as they **permanently change** the data.
   - Always **use `WHERE` clauses** to specify the rows to modify or delete. **Omitting `WHERE`** affects **all rows**.

3. **Common Use Cases**
   - Updating customer information (e.g., email or address).
   - Deleting outdated or incorrect records.

4. **Caution: Risk of Data Loss**
   - Accidentally **omitting `WHERE`** clauses can **update or delete all rows** in a table.
   - Always **double-check** `UPDATE` and `DELETE` statements before executing them.

---

## **Step 2: Updating Data Using UPDATE Statement**
1. **Purpose**
   - The `UPDATE` statement is used to **modify existing rows** in a table.

2. **Syntax**
   ```sql
   UPDATE table_name
   SET column_name = new_value
   WHERE condition;
   ```
   - `table_name`: The table to update.
   - `column_name`: The column to modify.
   - `new_value`: The new value to assign.
   - `condition`: Filters the rows to be updated.

3. **Example: Updating a Single Column**
   ```sql
   UPDATE customers
   SET cust_email = 'elmer@fudd.com'
   WHERE cust_id = 10005;
   ```
   - This updates the `cust_email` for **customer 10005**.

4. **Output Example**
   ```
   Query OK, 1 row affected
   ```

5. **Tip: Updating Multiple Columns**
   - Separate each column update with a comma:
     ```sql
     UPDATE customers
     SET cust_name = 'The Fudds',
         cust_email = 'elmer@fudd.com'
     WHERE cust_id = 10005;
     ```
   - This updates both `cust_name` and `cust_email` for **customer 10005**.

---

## **Step 3: Updating Multiple Rows**
1. **Purpose**
   - Update **multiple rows** that match a condition.

2. **Example: Setting a Default Value for Multiple Rows**
   ```sql
   UPDATE customers
   SET cust_email = 'unknown@example.com'
   WHERE cust_email IS NULL;
   ```
   - This updates all rows where **`cust_email` is NULL**.

3. **Output Example**
   ```
   Query OK, 3 rows affected
   ```

4. **Tip: Be Careful with WHERE Clause**
   - Without the `WHERE` clause, **all rows** will be updated.
     ```sql
     UPDATE customers
     SET cust_email = 'test@example.com';
     ```
   - This **updates every row** in the `customers` table, which is usually **not desired**.

---

## **Step 4: Using Subqueries in UPDATE Statements**
1. **Purpose**
   - Use **subqueries** to update columns with data retrieved from another query.

2. **Example: Updating Using Subquery**
   ```sql
   UPDATE products
   SET prod_price = prod_price * 1.1
   WHERE vend_id = (SELECT vend_id FROM vendors WHERE vend_name = 'Anvils R Us');
   ```
   - This **increases prices** by **10%** for all products made by **Anvils R Us**.

3. **Tip: Ensure Subquery Returns a Single Value**
   - Subqueries in `WHERE` should **return a single value**.
   - If multiple values are returned, use `IN` instead of `=`:
     ```sql
     WHERE vend_id IN (SELECT vend_id FROM vendors WHERE vend_country = 'USA');
     ```

---

## **Step 5: Deleting Data Using DELETE Statement**
1. **Purpose**
   - The `DELETE` statement **removes rows** from a table.

2. **Syntax**
   ```sql
   DELETE FROM table_name
   WHERE condition;
   ```
   - `table_name`: The table from which to delete rows.
   - `condition`: Filters the rows to be deleted.

3. **Example: Deleting a Single Row**
   ```sql
   DELETE FROM customers
   WHERE cust_id = 10005;
   ```
   - This deletes the **customer with `cust_id` 10005**.

4. **Output Example**
   ```
   Query OK, 1 row affected
   ```

5. **Tip: Deleting Multiple Rows**
   - You can **delete multiple rows** by specifying a condition:
     ```sql
     DELETE FROM customers
     WHERE cust_email IS NULL;
     ```
   - This **deletes all customers** without an email address.

---

## **Step 6: Deleting All Rows from a Table**
1. **Purpose**
   - To **empty** a table but keep its structure intact.

2. **Example: Deleting All Rows Using DELETE**
   ```sql
   DELETE FROM customers;
   ```
   - This **deletes all rows** in the `customers` table.

3. **Example: Using TRUNCATE for Faster Deletion**
   ```sql
   TRUNCATE TABLE customers;
   ```
   - `TRUNCATE TABLE` is **faster** because it **resets the table** without logging individual row deletions.

4. **Difference Between DELETE and TRUNCATE**
   - `DELETE`: **Slower** but allows use of `WHERE` clause.
   - `TRUNCATE`: **Faster**, but **cannot** use `WHERE`. It **empties the entire table**.

---

## **Step 7: Guidelines for Safe Updates and Deletions**
1. **Always Use WHERE Clauses**
   - Avoid updating or deleting all rows **by mistake**.
   - Double-check the `WHERE` clause **before executing** the query.

2. **Preview Changes with SELECT First**
   - Before running `UPDATE` or `DELETE`, **preview the affected rows** using `SELECT`:
     ```sql
     SELECT * FROM customers WHERE cust_email IS NULL;
     ```

3. **Use Transactions for Safety**
   - Use `START TRANSACTION`, `COMMIT`, and `ROLLBACK` to **safeguard data**:
     ```sql
     START TRANSACTION;
     DELETE FROM customers WHERE cust_email IS NULL;
     ROLLBACK; -- Undo the deletion
     COMMIT;   -- Make the deletion permanent
     ```

---

## **Step 8: Hands-on Exercises**
### **Objective:**
- Practice updating and deleting data safely.

### **Tasks:**
1. **Update Email for a Specific Customer**
   ```sql
   UPDATE customers
   SET cust_email = 'new.email@example.com'
   WHERE cust_id = 10001;
   ```
2. **Delete Customers Without Email**
   ```sql
   DELETE FROM customers
   WHERE cust_email IS NULL;
   ```
3. **Increase Prices for a Specific Vendor's Products**
   ```sql
   UPDATE products
   SET prod_price = prod_price * 1.1
   WHERE vend_id = (SELECT vend_id FROM vendors WHERE vend_name = 'Anvils R Us');
   ```

---

## **Conclusion**
By following this manual, you will learn:
- How to **safely update** data using `UPDATE`.
- How to **safely delete** data using `DELETE`.
- How to **use subqueries** in `UPDATE`.
- How to **use transactions** to protect data.