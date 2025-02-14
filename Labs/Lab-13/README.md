# **Working with Subqueries**

## **Objective**
This manual will guide students through using **subqueries** in MySQL, including using them in `WHERE` clauses, as calculated fields, and understanding the difference between correlated and noncorrelated subqueries.

---

## **Step 1: Understanding Subqueries**
1. **What Is a Subquery?**
   - A **subquery** is a **query within another query**.
   - It is enclosed in **parentheses** and provides a value or set of values to the main query.

2. **Types of Subqueries**
   - **Noncorrelated Subquery**: Independent of the outer query.
   - **Correlated Subquery**: Depends on values from the outer query.

3. **Where Can Subqueries Be Used?**
   - In `WHERE` clauses for filtering.
   - As **calculated fields** in the `SELECT` clause.
   - In `FROM` clauses to provide a derived table.

4. **Example Structure**
   ```sql
   SELECT column_name
   FROM table_name
   WHERE column_name = (SELECT column_name FROM another_table);
   ```
   - The inner query runs **first** and provides a value for the outer query.

---

## **Step 2: Using Subqueries in WHERE Clauses**
1. **Purpose**
   - Use subqueries in `WHERE` clauses to **filter data** based on the result of another query.

2. **Syntax**
   ```sql
   SELECT column_name
   FROM table_name
   WHERE column_name = (SELECT column_name FROM another_table WHERE condition);
   ```

3. **Example: Finding Customers with Orders**
   ```sql
   SELECT cust_name, cust_id
   FROM customers
   WHERE cust_id IN (SELECT cust_id FROM orders);
   ```
   - This query finds customers who have placed **at least one order**.

4. **Output Example**
   ```
   +---------------+---------+
   | cust_name     | cust_id |
   +---------------+---------+
   | John Doe      |   10001 |
   | Jane Smith    |   10002 |
   +---------------+---------+
   ```

5. **Tip: Using Comparison Operators**
   - Subqueries can be used with comparison operators:
     ```sql
     SELECT prod_name
     FROM products
     WHERE prod_price > (SELECT AVG(prod_price) FROM products);
     ```
     - This finds products **priced above average**.

---

## **Step 3: Filtering by Subquery Results**
1. **Purpose**
   - Filter results based on **aggregated data** from another query.

2. **Example: Finding High-Value Orders**
   ```sql
   SELECT order_num, quantity * item_price AS order_total
   FROM orderitems
   WHERE quantity * item_price > (SELECT AVG(quantity * item_price) FROM orderitems);
   ```
   - This finds orders **greater than the average order value**.

3. **Output Example**
   ```
   +-----------+-------------+
   | order_num | order_total |
   +-----------+-------------+
   |     20005 |       149.87|
   |     20007 |      1000.00|
   +-----------+-------------+
   ```

4. **Tip: Using Subqueries with IN and NOT IN**
   - `IN` checks if a value is in a list of values from a subquery.
   - `NOT IN` checks if a value is **not** in the list.
     ```sql
     SELECT cust_name
     FROM customers
     WHERE cust_id NOT IN (SELECT cust_id FROM orders);
     ```
     - Finds customers who **have not** placed any orders.

---

## **Step 4: Using Subqueries as Calculated Fields**
1. **Purpose**
   - Use subqueries as **calculated fields** to provide additional data in the `SELECT` statement.

2. **Syntax**
   ```sql
   SELECT column_name,
          (SELECT AGGREGATE_FUNCTION(column_name) FROM another_table WHERE condition) AS alias_name
   FROM table_name;
   ```

3. **Example: Showing Total Sales per Customer**
   ```sql
   SELECT cust_name,
          (SELECT SUM(quantity * item_price)
           FROM orders o
           JOIN orderitems oi ON o.order_num = oi.order_num
           WHERE o.cust_id = customers.cust_id) AS total_sales
   FROM customers;
   ```
   - This calculates the **total sales** for each customer.

4. **Output Example**
   ```
   +---------------+-------------+
   | cust_name     | total_sales |
   +---------------+-------------+
   | John Doe      |       149.87|
   | Jane Smith    |      1000.00|
   +---------------+-------------+
   ```

5. **Tip: Subqueries in SELECT Clauses**
   - Subqueries can be used as calculated fields but must **return a single value**.
   - If the subquery returns **more than one value**, an error will occur.

---

## **Step 5: Correlated vs. Noncorrelated Subqueries**
1. **Noncorrelated Subqueries**
   - **Independent** of the outer query.
   - Executed **once** and the result is used by the outer query.

   **Example: Noncorrelated Subquery**
   ```sql
   SELECT prod_name
   FROM products
   WHERE prod_price > (SELECT AVG(prod_price) FROM products);
   ```
   - The inner query runs **once** to calculate the average price.

2. **Correlated Subqueries**
   - **Dependent** on the outer query.
   - Executed **once for each row** processed by the outer query.

   **Example: Correlated Subquery**
   ```sql
   SELECT cust_name
   FROM customers c
   WHERE EXISTS (SELECT 1
                 FROM orders o
                 WHERE o.cust_id = c.cust_id);
   ```
   - The inner query runs for **each customer** to check if they have any orders.

3. **Tip: When to Use Correlated Subqueries**
   - Use correlated subqueries when filtering depends on **row-specific data** from the outer query.

---

## **Step 6: Hands-on Exercises**
### **Objective:**
- Practice using subqueries in `WHERE` clauses, as calculated fields, and understand correlated subqueries.

### **Tasks:**
1. **Find Products Priced Above Average**
   ```sql
   SELECT prod_name
   FROM products
   WHERE prod_price > (SELECT AVG(prod_price) FROM products);
   ```
2. **List Customers Without Orders**
   ```sql
   SELECT cust_name
   FROM customers
   WHERE cust_id NOT IN (SELECT cust_id FROM orders);
   ```
3. **Calculate Total Sales per Customer**
   ```sql
   SELECT cust_name,
          (SELECT SUM(quantity * item_price)
           FROM orders o
           JOIN orderitems oi ON o.order_num = oi.order_num
           WHERE o.cust_id = customers.cust_id) AS total_sales
   FROM customers;
   ```
4. **Check for Customers with Orders Using EXISTS**
   ```sql
   SELECT cust_name
   FROM customers c
   WHERE EXISTS (SELECT 1
                 FROM orders o
                 WHERE o.cust_id = c.cust_id);
   ```

---

## **Conclusion**
By following this manual, students will learn:
- How to use subqueries in `WHERE` clauses for complex filtering.
- How to use subqueries as calculated fields.
- The difference between correlated and noncorrelated subqueries.