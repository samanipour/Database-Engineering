# **Using Cursors**

---

## **Step 1: Understanding Cursors**
1. **What Are Cursors?**
   - A **cursor** is a database query stored on the MySQL server.
   - It allows you to **step through rows** in a result set **one at a time**.
   - Cursors are used **within stored procedures** for **row-by-row processing**.

2. **Why Use Cursors?**
   - When you need to **process each row** individually rather than as a batch.
   - Useful for complex business logic that **depends on row-specific data**.

3. **When to Use Cursors?**
   - When performing calculations or processing that **require knowledge of previous or next rows**.
   - When **updating** or **deleting** rows based on calculations involving other rows.

4. **Limitations of Cursors**
   - Cursors are **slower** than set-based operations.
   - They **consume more memory** on the server.
   - Use cursors **only when necessary**.

---

## **Step 2: Declaring Cursors**
1. **Purpose**
   - Cursors must be **declared** before they can be used.
   - Declaring a cursor **defines the query** it will execute.

2. **Syntax**
   ```sql
   DECLARE cursor_name CURSOR
   FOR SELECT_statement;
   ```
   - The `SELECT_statement` specifies **which rows** to retrieve.

3. **Example: Declaring a Cursor for Orders**
   ```sql
   DELIMITER //

   CREATE PROCEDURE processorders()
   BEGIN
      DECLARE ordernumbers CURSOR
      FOR
      SELECT order_num FROM orders;
   END //

   DELIMITER ;
   ```
   - This declares a cursor named `ordernumbers` that **selects all order numbers** from the `orders` table.
   - It does **not** retrieve data yet; it only **defines** the query.

4. **Tip: Where to Declare Cursors**
   - Cursors must be declared **after variable declarations** but **before handlers and the cursor is opened**.

---

## **Step 3: Opening and Closing Cursors**
1. **Purpose**
   - Cursors must be **opened** to **execute the query** and **retrieve data**.
   - Cursors must be **closed** to **release resources** after processing is complete.

2. **Syntax**
   - **Open a Cursor**:
     ```sql
     OPEN cursor_name;
     ```
   - **Close a Cursor**:
     ```sql
     CLOSE cursor_name;
     ```

3. **Example: Opening and Closing a Cursor**
   ```sql
   DELIMITER //

   CREATE PROCEDURE processorders()
   BEGIN
      DECLARE ordernumbers CURSOR
      FOR
      SELECT order_num FROM orders;

      -- Open the cursor
      OPEN ordernumbers;

      -- Close the cursor
      CLOSE ordernumbers;
   END //

   DELIMITER ;
   ```
   - `OPEN ordernumbers`: Executes the query and **stores the result set**.
   - `CLOSE ordernumbers`: **Releases memory** and **resources** used by the cursor.

4. **Tip: Implicit Closing of Cursors**
   - MySQL **automatically closes** a cursor when the `END` statement is reached.
   - However, it is a **good practice** to explicitly close it.

---

## **Step 4: Fetching Data Using Cursors**
1. **Purpose**
   - `FETCH` is used to **retrieve rows** from the cursor result set **one at a time**.

2. **Syntax**
   ```sql
   FETCH cursor_name INTO variable_list;
   ```
   - The `variable_list` specifies **local variables** that store the retrieved data.

3. **Example: Fetching Data into a Variable**
   ```sql
   DELIMITER //

   CREATE PROCEDURE processorders()
   BEGIN
      -- Declare local variable to store order number
      DECLARE o INT;

      -- Declare the cursor
      DECLARE ordernumbers CURSOR
      FOR
      SELECT order_num FROM orders;

      -- Open the cursor
      OPEN ordernumbers;

      -- Fetch the first row into variable o
      FETCH ordernumbers INTO o;

      -- Close the cursor
      CLOSE ordernumbers;
   END //

   DELIMITER ;
   ```
   - `DECLARE o INT`: Declares a **local variable** to store each `order_num`.
   - `FETCH ordernumbers INTO o`: **Retrieves the next row** from the cursor result set and **stores** it in `o`.

4. **Tip: Cursor Position**
   - Each `FETCH` advances the cursor to the **next row**.
   - When no more rows are available, the **NOT FOUND** condition occurs.

---

## **Step 5: Looping Through Cursors**
1. **Purpose**
   - Loop through the cursor to **process each row** in the result set.

2. **Using REPEAT Loop with Cursors**
   - A `REPEAT` loop is commonly used for cursor processing.
   - A **continue handler** is required to **handle the end of the result set**.

3. **Syntax**
   ```sql
   DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = 1;

   REPEAT
      FETCH cursor_name INTO variable_list;
   UNTIL done END REPEAT;
   ```

4. **Example: Looping Through Orders**
   ```sql
   DELIMITER //

   CREATE PROCEDURE processorders()
   BEGIN
      -- Declare local variables
      DECLARE done BOOLEAN DEFAULT 0;
      DECLARE o INT;

      -- Declare the cursor
      DECLARE ordernumbers CURSOR
      FOR
      SELECT order_num FROM orders;

      -- Declare continue handler
      DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = 1;

      -- Open the cursor
      OPEN ordernumbers;

      -- Loop through all rows
      REPEAT
         FETCH ordernumbers INTO o;
         -- Here you can add processing logic for each row
      UNTIL done END REPEAT;

      -- Close the cursor
      CLOSE ordernumbers;
   END //

   DELIMITER ;
   ```
   - `DECLARE done BOOLEAN DEFAULT 0`: Tracks **end of the cursor**.
   - `DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = 1`: Sets `done` to **1** when no more rows are available.
   - `REPEAT ... UNTIL done END REPEAT`: Loops through **all rows** in the cursor.

5. **Tip: When to Use REPEAT vs. LOOP**
   - `REPEAT` is ideal for cursor loops as it checks the exit condition **at the end**.
   - `LOOP` can also be used with an **explicit `LEAVE` statement**.

---

## **Step 6: Hands-on Exercises**
### **Objective:**
- Practice using cursors to retrieve and process rows one at a time.

### **Tasks:**
1. **Retrieve All Order Numbers Using a Cursor**
   ```sql
   DELIMITER //

   CREATE PROCEDURE listorders()
   BEGIN
      DECLARE done BOOLEAN DEFAULT 0;
      DECLARE o INT;
      DECLARE ordernumbers CURSOR FOR SELECT order_num FROM orders;
      DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = 1;

      OPEN ordernumbers;

      REPEAT
         FETCH ordernumbers INTO o;
         SELECT o;
      UNTIL done END REPEAT;

      CLOSE ordernumbers;
   END //

   DELIMITER ;
   ```
2. **Calculate and Store Totals for All Orders**
   ```sql
   DELIMITER //

   CREATE PROCEDURE calculatetotals()
   BEGIN
      DECLARE done BOOLEAN DEFAULT 0;
      DECLARE o INT;
      DECLARE t DECIMAL(8,2);

      DECLARE ordernumbers CURSOR FOR SELECT order_num FROM orders;
      DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = 1;

      OPEN ordernumbers;

      REPEAT
         FETCH ordernumbers INTO o;
         CALL ordertotal(o, 1, t);
         INSERT INTO ordertotals(order_num, total) VALUES(o, t);
      UNTIL done END REPEAT;

      CLOSE ordernumbers;
   END //

   DELIMITER ;
   ```

---

## **Conclusion**
By following this manual, you will learn:
- How to **declare, open, fetch, and close cursors**.
- How to **loop through result sets** row by row.
- How to **combine cursors with stored procedures** for advanced processing.