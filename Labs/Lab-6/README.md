# **Advanced Data Filtering**

## **Objective**
This manual will guide you through advanced data filtering techniques in MySQL using the `WHERE` clause, including combining conditions with `AND` and `OR`, using parentheses to control order of evaluation, and applying `IN` and `NOT` operators.

---

## **Step 1: Combining WHERE Clauses**
1. **Why Combine WHERE Clauses?**
   - In Chapter 6, filtering was done using a **single criterion**.
   - Often, more complex filtering is required using **multiple criteria**.
   - This can be done using:
     - `AND` to **combine conditions** that all must be true.
     - `OR` to **combine conditions** where at least one must be true.

2. **Using the AND Operator**
   - **Purpose:** Combine multiple conditions where **all** conditions must be met.
   - **Syntax:**
     ```sql
     SELECT column_name
     FROM table_name
     WHERE condition1 AND condition2;
     ```

3. **Example: Filtering with AND**
   ```sql
   SELECT prod_id, prod_price, prod_name
   FROM products
   WHERE vend_id = 1003 AND prod_price <= 10;
   ```
   - This retrieves products made by vendor `1003` that cost `10` or less.

4. **Output Example**
   ```
   +---------+------------+----------------+
   | prod_id | prod_price | prod_name       |
   +---------+------------+----------------+
   | FB      |      10.00 | Bird seed       |
   | FC      |       2.50 | Carrots         |
   | SLING   |       4.49 | Sling            |
   | TNT1    |       2.50 | TNT (1 stick)   |
   | TNT2    |      10.00 | TNT (5 sticks)  |
   +---------+------------+----------------+
   ```

5. **Tip: Using Multiple AND Conditions**
   - Multiple conditions can be combined using multiple `AND` keywords:
     ```sql
     WHERE condition1 AND condition2 AND condition3;
     ```

---

## **Step 2: Using the OR Operator**
1. **Purpose**
   - Combine multiple conditions where **at least one** condition must be true.

2. **Syntax**
   ```sql
   SELECT column_name
   FROM table_name
   WHERE condition1 OR condition2;
   ```

3. **Example: Filtering with OR**
   ```sql
   SELECT prod_name, prod_price
   FROM products
   WHERE vend_id = 1002 OR vend_id = 1003;
   ```
   - This retrieves products made by either vendor `1002` or `1003`.

4. **Output Example**
   ```
   +----------------+------------+
   | prod_name      | prod_price |
   +----------------+------------+
   | Detonator      |      13.00 |
   | Bird seed      |      10.00 |
   | Carrots        |       2.50 |
   | Fuses          |       3.42 |
   | Oil can        |       8.99 |
   | Safe           |      50.00 |
   | Sling          |       4.49 |
   | TNT (1 stick)  |       2.50 |
   | TNT (5 sticks) |      10.00 |
   +----------------+------------+
   ```

5. **Tip: Using Multiple OR Conditions**
   - Multiple conditions can be combined using multiple `OR` keywords:
     ```sql
     WHERE condition1 OR condition2 OR condition3;
     ```

---

## **Step 3: Understanding the Order of Evaluation**
1. **Why is Order Important?**
   - `AND` and `OR` have different **precedence** levels.
   - In MySQL, `AND` is evaluated **before** `OR`.

2. **Problem Example: Incorrect Order of Evaluation**
   ```sql
   SELECT prod_name, prod_price
   FROM products
   WHERE vend_id = 1002 OR vend_id = 1003 AND prod_price >= 10;
   ```
   - This is evaluated as:
     ```
     WHERE vend_id = 1002 OR (vend_id = 1003 AND prod_price >= 10);
     ```
   - It retrieves:
     - Products made by vendor `1002` **regardless of price**.
     - Products made by vendor `1003` **only if price is 10 or more**.

3. **Solution: Using Parentheses**
   - **Parentheses** can be used to explicitly control order of evaluation.
   - Corrected Example:
     ```sql
     SELECT prod_name, prod_price
     FROM products
     WHERE (vend_id = 1002 OR vend_id = 1003) AND prod_price >= 10;
     ```

4. **Output Example**
   ```
   +----------------+------------+
   | prod_name      | prod_price |
   +----------------+------------+
   | Detonator      |      13.00 |
   | Bird seed      |      10.00 |
   | Safe           |      50.00 |
   | TNT (5 sticks) |      10.00 |
   +----------------+------------+
   ```

5. **Tip: Always Use Parentheses**
   - Always use parentheses when combining `AND` and `OR`.
   - This ensures the correct order of evaluation.

---

## **Step 4: Using the IN Operator**
1. **Purpose**
   - Match **any of multiple values**.
   - It’s more readable and efficient than using multiple `OR` conditions.

2. **Syntax**
   ```sql
   SELECT column_name
   FROM table_name
   WHERE column_name IN (value1, value2, ...);
   ```

3. **Example: Using IN**
   ```sql
   SELECT prod_name, prod_price
   FROM products
   WHERE vend_id IN (1002, 1003)
   ORDER BY prod_name;
   ```
   - This retrieves products made by vendors `1002` or `1003`.

4. **Output Example**
   ```
   +----------------+------------+
   | prod_name      | prod_price |
   +----------------+------------+
   | Bird seed      |      10.00 |
   | Carrots        |       2.50 |
   | Detonator      |      13.00 |
   | Fuses          |       3.42 |
   | Oil can        |       8.99 |
   | Safe           |      50.00 |
   +----------------+------------+
   ```

---

## **Step 5: Using the NOT Operator**
1. **Purpose**
   - **Negate** a condition.
   - Used to filter data **not matching** a condition.

2. **Syntax**
   ```sql
   SELECT column_name
   FROM table_name
   WHERE NOT condition;
   ```

3. **Example: Using NOT with IN**
   ```sql
   SELECT prod_name, prod_price
   FROM products
   WHERE vend_id NOT IN (1002, 1003)
   ORDER BY prod_name;
   ```
   - This retrieves products **not** made by vendors `1002` or `1003`.

4. **Output Example**
   ```
   +--------------+------------+
   | prod_name    | prod_price |
   +--------------+------------+
   | .5 ton anvil |       5.99 |
   | 1 ton anvil  |       9.99 |
   | 2 ton anvil  |      14.99 |
   | JetPack 1000 |      35.00 |
   | JetPack 2000 |      55.00 |
   +--------------+------------+
   ```

---

## **Conclusion**
By following this manual, students will learn:
- How to combine multiple conditions using `AND` and `OR`.
- How to use parentheses to control order of evaluation.
- How to use `IN` to match multiple values.
- How to use `NOT` to negate conditions.