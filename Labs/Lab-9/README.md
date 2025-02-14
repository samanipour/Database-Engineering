# **Creating Calculated Fields**

## **Objective**
This manual will guide you through creating **calculated fields** in MySQL, including concatenating fields, using aliases, and performing mathematical calculations directly within `SELECT` statements.

---

## **Step 1: Understanding Calculated Fields**
1. **What Are Calculated Fields?**
   - **Calculated fields** are temporary fields created within a `SELECT` statement.
   - They **don’t exist** in the database tables but are generated **on-the-fly** when a query is executed.

2. **Why Use Calculated Fields?**
   - To **combine multiple columns** into one output.
   - To **perform calculations** on column values (e.g., total prices, percentages).
   - To **reformat data** for better readability or application requirements.

3. **Example Use Cases**
   - Combining first name and last name into a full name.
   - Calculating total price as `quantity * item_price`.
   - Formatting addresses by concatenating city, state, and zip code.

---

## **Step 2: Concatenating Fields**
1. **Purpose**
   - Combine multiple columns into a single output field.

2. **Syntax**
   ```sql
   SELECT CONCAT(column1, column2, ...) AS alias_name
   FROM table_name;
   ```
   - `CONCAT()` function joins multiple columns or strings.
   - `AS` is used to **assign an alias** to the calculated field for better readability.

3. **Example: Concatenating Product Name and Price**
   ```sql
   SELECT CONCAT(prod_name, ' ($', prod_price, ')') AS product_details
   FROM products;
   ```
   - This combines `prod_name` and `prod_price` into a single column named `product_details`.

4. **Output Example**
   ```
   +--------------------------+
   | product_details          |
   +--------------------------+
   | .5 ton anvil ($5.99)     |
   | 1 ton anvil ($9.99)      |
   | 2 ton anvil ($14.99)     |
   | Oil can ($8.99)          |
   +--------------------------+
   ```

5. **Tip: Using Static Text in CONCAT()**
   - Static text (e.g., `($` and `)`) can be included in the concatenation.
   - Text must be enclosed in **single quotes ('...')**.

---

## **Step 3: Using Aliases**
1. **Purpose**
   - Give **descriptive names** to calculated fields.
   - Make the output easier to understand.

2. **Syntax**
   ```sql
   SELECT calculation AS alias_name
   FROM table_name;
   ```
   - `AS` is used to assign an alias to the calculated field.

3. **Example: Creating a Full Name Field**
   ```sql
   SELECT CONCAT(cust_fname, ' ', cust_lname) AS full_name
   FROM customers;
   ```
   - Combines `cust_fname` and `cust_lname` into a single field named `full_name`.

4. **Output Example**
   ```
   +-----------------+
   | full_name       |
   +-----------------+
   | John Doe        |
   | Jane Smith      |
   | Jim Brown       |
   +-----------------+
   ```

5. **Tip: Using Aliases Without AS**
   - `AS` is optional, but using it **improves readability**.
   - Example without `AS`:
     ```sql
     SELECT CONCAT(cust_fname, ' ', cust_lname) full_name
     FROM customers;
     ```
   - The output remains the same.

---

## **Step 4: Performing Mathematical Calculations**
1. **Purpose**
   - Perform calculations **directly within the query** without requiring application-side logic.
   - Useful for computing totals, discounts, percentages, etc.

2. **Syntax**
   ```sql
   SELECT calculation AS alias_name
   FROM table_name;
   ```
   - Basic mathematical operators:
     - `+`: Addition
     - `-`: Subtraction
     - `*`: Multiplication
     - `/`: Division

3. **Example: Calculating Total Price**
   ```sql
   SELECT prod_name,
          quantity,
          item_price,
          quantity * item_price AS total_price
   FROM orderitems;
   ```
   - Calculates the total price by multiplying `quantity` and `item_price`.

4. **Output Example**
   ```
   +------------+----------+------------+-------------+
   | prod_name  | quantity | item_price | total_price |
   +------------+----------+------------+-------------+
   | .5 ton anvil |       10 |       5.99 |        59.90 |
   | 1 ton anvil  |        3 |       9.99 |        29.97 |
   | TNT (5 sticks)|        5 |      10.00 |        50.00 |
   +------------+----------+------------+-------------+
   ```

5. **Example: Calculating Discounted Price**
   ```sql
   SELECT prod_name,
          prod_price,
          prod_price * 0.9 AS discounted_price
   FROM products;
   ```
   - Calculates a **10% discount** on each product price.

6. **Output Example**
   ```
   +--------------+------------+------------------+
   | prod_name    | prod_price | discounted_price |
   +--------------+------------+------------------+
   | .5 ton anvil |       5.99 |             5.39  |
   | 1 ton anvil  |       9.99 |             8.99  |
   | 2 ton anvil  |      14.99 |            13.49  |
   +--------------+------------+------------------+
   ```

---

## **Step 5: Combining Multiple Calculations**
1. **Purpose**
   - Perform **multiple calculations** in a single query.

2. **Example: Calculating Total and Discounted Price**
   ```sql
   SELECT prod_name,
          quantity,
          item_price,
          quantity * item_price AS total_price,
          (quantity * item_price) * 0.9 AS discounted_total
   FROM orderitems;
   ```
   - Calculates the total price and the discounted total (10% off).

3. **Output Example**
   ```
   +------------+----------+------------+-------------+------------------+
   | prod_name  | quantity | item_price | total_price | discounted_total |
   +------------+----------+------------+-------------+------------------+
   | .5 ton anvil |       10 |       5.99 |        59.90 |             53.91 |
   | 1 ton anvil  |        3 |       9.99 |        29.97 |             26.97 |
   | TNT (5 sticks)|        5 |      10.00 |        50.00 |             45.00 |
   +------------+----------+------------+-------------+------------------+
   ```

---

## **Step 6: Hands-on Exercises**
### **Objective:**
- Practice creating calculated fields with concatenation, aliases, and mathematical calculations.

### **Tasks:**
1. **Concatenate Customer Name and Email**
   ```sql
   SELECT CONCAT(cust_name, ' (', cust_email, ')') AS contact_details
   FROM customers;
   ```
2. **Calculate Total Order Amount**
   ```sql
   SELECT order_num,
          Sum(quantity * item_price) AS total_amount
   FROM orderitems
   GROUP BY order_num;
   ```
3. **Calculate Discounted Product Prices**
   ```sql
   SELECT prod_name,
          prod_price,
          prod_price * 0.85 AS discounted_price
   FROM products;
   ```

---

## **Conclusion**
By following this manual, students will learn:
- How to create calculated fields in MySQL using `CONCAT()` and mathematical operations.
- How to use aliases for better output readability.
- How to perform multiple calculations in a single query.