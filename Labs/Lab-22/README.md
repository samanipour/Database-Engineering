# **Stored Procedures**

## **Objective**
This manual will guide you through **using stored procedures** in MySQL, including understanding their purpose, creating, executing, and managing stored procedures. Students will also learn about working with parameters, building intelligent procedures, and troubleshooting common issues.

---

## **Step 1: Understanding Stored Procedures**
1. **What Are Stored Procedures?**
   - A **stored procedure** is a **set of SQL statements** saved in the database.
   - It is executed as a **single unit** to perform a specific task.

2. **Why Use Stored Procedures?**
   - **Code Reusability**: Write once, reuse multiple times.
   - **Performance Improvement**: Stored procedures are **precompiled**, which speeds up execution.
   - **Security and Integrity**: Users can execute procedures without having direct access to tables.

3. **Use Cases for Stored Procedures**
   - **Data validation** before insertion or update.
   - **Automating repetitive tasks** like daily reports.
   - **Complex calculations** and **data transformations**.
   - **Conditional logic** with flow control (e.g., loops and branches).

4. **Example Scenario**
   - A procedure to **calculate total sales** for a given customer.

---

## **Step 2: Creating Stored Procedures**
1. **Purpose**
   - Create stored procedures to **automate tasks** and **simplify complex queries**.

2. **Syntax**
   ```sql
   CREATE PROCEDURE procedure_name(parameters)
   BEGIN
      SQL statement(s);
   END;
   ```
   - `procedure_name`: Name of the procedure.
   - `parameters`: Input, output, or input-output parameters.
   - `BEGIN ... END`: Marks the **body** of the procedure.

3. **Example: Calculating Total Sales for a Customer**
   ```sql
   DELIMITER //

   CREATE PROCEDURE get_customer_sales(IN cust_id INT, OUT total_sales DECIMAL(10, 2))
   BEGIN
      SELECT SUM(quantity * item_price)
      INTO total_sales
      FROM orderitems
      JOIN orders ON orderitems.order_num = orders.order_num
      WHERE orders.cust_id = cust_id;
   END //

   DELIMITER ;
   ```
   - `IN` parameter `cust_id`: Input value provided when calling the procedure.
   - `OUT` parameter `total_sales`: Output value returned after execution.
   - `SUM()` calculates **total sales** for the specified customer.

4. **Explanation of DELIMITER**
   - `DELIMITER //` is used to **change the statement delimiter**.
   - This allows writing multi-statement procedures without MySQL misinterpreting the semicolons.

5. **Tip: Using IF Statements in Stored Procedures**
   - Stored procedures can use **conditional logic**:
     ```sql
     IF condition THEN
        -- SQL Statements
     ELSE
        -- Alternative SQL Statements
     END IF;
     ```

---

## **Step 3: Executing Stored Procedures**
1. **Purpose**
   - Call stored procedures to **execute predefined tasks**.

2. **Syntax**
   ```sql
   CALL procedure_name(parameters);
   ```

3. **Example: Calling get_customer_sales**
   ```sql
   CALL get_customer_sales(1001, @total);
   SELECT @total AS total_sales;
   ```
   - `1001`: Input parameter (Customer ID).
   - `@total`: Output parameter to store the result.
   - `SELECT` displays the **total sales** calculated by the procedure.

4. **Output Example**
   ```
   +-------------+
   | total_sales |
   +-------------+
   |      149.87 |
   +-------------+
   ```

5. **Tip: Using Variables to Store Output**
   - Use `@variable_name` to **store output** from `OUT` parameters.
   - Example:
     ```sql
     CALL get_customer_sales(1002, @total);
     SELECT @total;
     ```

---

## **Step 4: Working with Parameters**
1. **Purpose**
   - Parameters allow stored procedures to **accept input**, **return output**, or **do both**.

2. **Types of Parameters**
   - `IN`: Input parameter (default). Passed to the procedure and **cannot** be changed.
   - `OUT`: Output parameter. Returned after execution and **can** be changed.
   - `INOUT`: Input and output parameter. Passed to the procedure, **can** be changed, and returned.

3. **Example: Using IN and OUT Parameters**
   ```sql
   DELIMITER //

   CREATE PROCEDURE check_inventory(IN prod_id CHAR(10), OUT stock INT)
   BEGIN
      SELECT quantity
      INTO stock
      FROM products
      WHERE prod_id = prod_id;
   END //

   DELIMITER ;
   ```
   - `IN prod_id`: Product ID to check inventory for.
   - `OUT stock`: Returns the **quantity in stock**.

4. **Calling check_inventory**
   ```sql
   CALL check_inventory('TNT2', @stock);
   SELECT @stock AS in_stock;
   ```

5. **Output Example**
   ```
   +----------+
   | in_stock |
   +----------+
   |       20 |
   +----------+
   ```

---

## **Step 5: Dropping Stored Procedures**
1. **Purpose**
   - Drop stored procedures that are **no longer needed** or require updates.

2. **Syntax**
   ```sql
   DROP PROCEDURE [IF EXISTS] procedure_name;
   ```

3. **Example: Dropping get_customer_sales**
   ```sql
   DROP PROCEDURE IF EXISTS get_customer_sales;
   ```

4. **Tip: Check for Existence**
   - Use `IF EXISTS` to **avoid errors** if the procedure does not exist.

---

## **Step 6: Building Intelligent Stored Procedures**
1. **Purpose**
   - Implement **conditional logic** and **loops** for more **complex procedures**.

2. **Example: Conditional Logic**
   ```sql
   DELIMITER //

   CREATE PROCEDURE update_inventory(IN prod_id CHAR(10), IN quantity INT)
   BEGIN
      IF quantity > 0 THEN
         UPDATE products
         SET quantity = quantity + quantity
         WHERE prod_id = prod_id;
      ELSE
         SIGNAL SQLSTATE '45000'
         SET MESSAGE_TEXT = 'Quantity must be greater than 0';
      END IF;
   END //

   DELIMITER ;
   ```
   - Uses `IF ... THEN ... ELSE ... END IF` to **check input conditions**.
   - `SIGNAL` is used to **raise custom errors**.

3. **Example: Looping Through Results**
   ```sql
   DECLARE done INT DEFAULT 0;
   DECLARE cur CURSOR FOR SELECT prod_name FROM products;
   DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = 1;
   OPEN cur;
   read_loop: LOOP
      FETCH cur INTO prod_name;
      IF done THEN
         LEAVE read_loop;
      END IF;
      -- Process each product name
   END LOOP;
   CLOSE cur;
   ```
   - Uses `CURSOR` to **iterate over query results**.

---

## **Step 7: Troubleshooting Stored Procedures**
1. **Common Issues**
   - **Syntax Errors**: Ensure proper use of `DELIMITER`, `BEGIN ... END`, and parameter declarations.
   - **Permissions**: User needs `CREATE ROUTINE` and `EXECUTE` privileges.
   - **Conflicting Names**: Avoid naming procedures the same as existing tables or columns.

2. **Debugging Tips**
   - Use `SHOW CREATE PROCEDURE procedure_name;` to **inspect stored procedure code**.
   - Use `SHOW PROCEDURE STATUS;` to **list all stored procedures**.

---

## **Step 8: Hands-on Exercises**
### **Objective:**
- Practice creating, executing, and managing stored procedures.

### **Tasks:**
1. **Create a Procedure to Calculate Total Sales**
   ```sql
   CREATE PROCEDURE calculate_sales(IN cust_id INT, OUT total DECIMAL(10, 2))
   BEGIN
      SELECT SUM(quantity * item_price)
      INTO total
      FROM orderitems
      JOIN orders ON orderitems.order_num = orders.order_num
      WHERE orders.cust_id = cust_id;
   END;
   ```
2. **Execute the Procedure**
   ```sql
   CALL calculate_sales(1001, @total);
   SELECT @total;
   ```
3. **Drop the Procedure**
   ```sql
   DROP PROCEDURE IF EXISTS calculate_sales;
   ```

---

## **Conclusion**
By following this manual, you will learn:
- How to create, execute, and manage stored procedures.
- How to use input, output, and input-output parameters.
- How to build intelligent stored procedures with conditional logic and loops.