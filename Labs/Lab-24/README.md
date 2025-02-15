# **Using Triggers**

## **Objective**
This manual will guide you through **creating and using triggers** in MySQL. Students will learn how to create triggers for different events (`INSERT`, `UPDATE`, `DELETE`), understand the use of `BEFORE` and `AFTER` triggers, and how to manage triggers in a database.

---

## **Step 1: Understanding Triggers**
1. **What Are Triggers?**
   - A **trigger** is a **set of MySQL statements** that are **automatically executed** (or fired) when a specified event occurs in a table.
   - Triggers are associated with a **specific table** and **event** (`INSERT`, `UPDATE`, or `DELETE`).

2. **Why Use Triggers?**
   - To **automate data validation** (e.g., check data integrity before insertion).
   - To **enforce business rules** consistently.
   - To **audit changes** by logging historical data.
   - To **synchronize tables** or perform **cascading updates/deletes**.

3. **Types of Triggers**
   - **BEFORE Trigger**: Executes **before** the event is performed.
     - Example: Validating data before an insert.
   - **AFTER Trigger**: Executes **after** the event is performed.
     - Example: Logging changes after an update.

4. **Trigger Events**
   - `INSERT`: Triggered when a new row is added.
   - `UPDATE`: Triggered when a row is updated.
   - `DELETE`: Triggered when a row is deleted.

---

## **Step 2: Creating a Simple Trigger**
1. **Purpose**
   - To create a trigger that **automatically logs changes** made to a table.

2. **Syntax**
   ```sql
   CREATE TRIGGER trigger_name
   {BEFORE | AFTER} {INSERT | UPDATE | DELETE}
   ON table_name
   FOR EACH ROW
   BEGIN
       -- SQL Statements;
   END;
   ```

3. **Example: Logging INSERTs**
   - This trigger **logs product inserts** by storing data into a log table.
   ```sql
   DELIMITER //

   CREATE TRIGGER after_product_insert
   AFTER INSERT ON products
   FOR EACH ROW
   BEGIN
       INSERT INTO product_log (prod_id, action, log_date)
       VALUES (NEW.prod_id, 'INSERT', NOW());
   END//

   DELIMITER ;
   ```
   - `AFTER INSERT`: Trigger executes **after a new product** is added.
   - `NEW`: Refers to the **newly inserted row**.
   - `NOW()`: Captures the **current timestamp**.

4. **Output Example**
   ```sql
   INSERT INTO products (prod_id, prod_name, prod_price)
   VALUES ('TNT3', 'Dynamite Pack', 19.99);

   SELECT * FROM product_log;
   ```
   ```
   +---------+--------+---------------------+
   | prod_id | action | log_date             |
   +---------+--------+---------------------+
   | TNT3    | INSERT | 2025-02-14 10:05:23  |
   +---------+--------+---------------------+
   ```

5. **Tip: Using OLD and NEW**
   - `NEW`: Refers to **new values** for `INSERT` and `UPDATE`.
   - `OLD`: Refers to **old values** for `UPDATE` and `DELETE`.

---

## **Step 3: Creating a BEFORE Trigger**
1. **Purpose**
   - To **validate data** before it is inserted or updated.

2. **Example: Checking Product Price**
   ```sql
   DELIMITER //

   CREATE TRIGGER before_product_insert
   BEFORE INSERT ON products
   FOR EACH ROW
   BEGIN
       IF NEW.prod_price < 0 THEN
           SIGNAL SQLSTATE '45000'
           SET MESSAGE_TEXT = 'Product price cannot be negative';
       END IF;
   END//

   DELIMITER ;
   ```
   - `BEFORE INSERT`: Trigger checks the price **before the new row is inserted**.
   - `SIGNAL`: Throws an **error** if the condition is met.

3. **Output Example**
   ```sql
   INSERT INTO products (prod_id, prod_name, prod_price)
   VALUES ('TNT4', 'Explosive Bundle', -10.00);
   ```
   ```
   ERROR 1644 (45000): Product price cannot be negative
   ```

4. **Tip: Using SIGNAL**
   - `SIGNAL` is used to **throw custom error messages** in MySQL.
   - It helps **enforce data integrity** by preventing invalid data.

---

## **Step 4: Creating an UPDATE Trigger**
1. **Purpose**
   - To **track changes** made during an update.

2. **Example: Logging Product Price Changes**
   ```sql
   DELIMITER //

   CREATE TRIGGER after_product_update
   AFTER UPDATE ON products
   FOR EACH ROW
   BEGIN
       IF OLD.prod_price != NEW.prod_price THEN
           INSERT INTO product_log (prod_id, action, log_date, old_price, new_price)
           VALUES (NEW.prod_id, 'UPDATE', NOW(), OLD.prod_price, NEW.prod_price);
       END IF;
   END//

   DELIMITER ;
   ```
   - `AFTER UPDATE`: Trigger fires **after the product is updated**.
   - `OLD` and `NEW`: Used to **compare old and new values**.

3. **Output Example**
   ```sql
   UPDATE products
   SET prod_price = 24.99
   WHERE prod_id = 'TNT3';

   SELECT * FROM product_log;
   ```
   ```
   +---------+--------+---------------------+-----------+-----------+
   | prod_id | action | log_date             | old_price | new_price |
   +---------+--------+---------------------+-----------+-----------+
   | TNT3    | UPDATE | 2025-02-14 10:15:42  |     19.99 |     24.99 |
   +---------+--------+---------------------+-----------+-----------+
   ```

---

## **Step 5: Dropping Triggers**
1. **Purpose**
   - To **delete a trigger** from the database.

2. **Syntax**
   ```sql
   DROP TRIGGER [IF EXISTS] trigger_name;
   ```

3. **Example: Dropping a Trigger**
   ```sql
   DROP TRIGGER IF EXISTS after_product_update;
   ```

4. **Tip: Checking Existing Triggers**
   - Use `SHOW TRIGGERS` to **list all triggers** in the database.
   ```sql
   SHOW TRIGGERS FROM db_name;
   ```

---

## **Step 6: Hands-on Exercises**
### **Objective:**
- Practice creating triggers for `INSERT`, `UPDATE`, and `DELETE`.

### **Tasks:**
1. **Log Deleted Products**
   ```sql
   DELIMITER //

   CREATE TRIGGER after_product_delete
   AFTER DELETE ON products
   FOR EACH ROW
   BEGIN
       INSERT INTO product_log (prod_id, action, log_date)
       VALUES (OLD.prod_id, 'DELETE', NOW());
   END//

   DELIMITER ;
   ```
2. **Validate Product Name Before Insert**
   ```sql
   DELIMITER //

   CREATE TRIGGER before_product_insert
   BEFORE INSERT ON products
   FOR EACH ROW
   BEGIN
       IF NEW.prod_name IS NULL OR NEW.prod_name = '' THEN
           SIGNAL SQLSTATE '45000'
           SET MESSAGE_TEXT = 'Product name cannot be empty';
       END IF;
   END//

   DELIMITER ;
   ```

---

## **Conclusion**
By following this manual, you will learn:
- How to create and manage triggers in MySQL.
- The difference between `BEFORE` and `AFTER` triggers.
- How to use `OLD` and `NEW` values in triggers.
- How to enforce data integrity and automate actions using triggers.