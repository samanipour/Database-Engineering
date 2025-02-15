# **Using Views**

## **Objective**
This manual will guide you through using **views** in MySQL, including creating, updating, and deleting views. It also covers how to use views to simplify complex joins, reformat retrieved data, filter unwanted data, and work with calculated fields.

---

## **Step 1: Understanding Views**
1. **What Is a View?**
   - A **view** is a **virtual table** based on the result of a `SELECT` query.
   - It **does not store data** but presents data stored in tables.
   - The view **updates automatically** when the underlying table data changes.

2. **Why Use Views?**
   - **Simplify complex queries** by hiding complexity behind a view.
   - **Improve security** by exposing only specific columns.
   - **Enhance maintainability** by centralizing complex logic.
   - **Reformat data** without changing the underlying tables.
   - **Filter unwanted data** to present only relevant information.

3. **Example Scenario**
   - Consider two tables:
     - `orders`: Stores order information, including `order_num` and `cust_id`.
     - `customers`: Stores customer details, including `cust_id` and `cust_name`.
   - To get a **list of orders with customer names**, use a view to **join** both tables.

---

## **Step 2: Creating Views**
1. **Purpose**
   - A view is created to **save a query** for reuse as a **virtual table**.

2. **Syntax**
   ```sql
   CREATE VIEW view_name AS
   SELECT column1, column2, ...
   FROM table_name
   WHERE condition;
   ```

3. **Example: Creating a View for Customer Orders**
   ```sql
   CREATE VIEW customer_orders AS
   SELECT orders.order_num, customers.cust_name
   FROM orders
   INNER JOIN customers ON orders.cust_id = customers.cust_id;
   ```
   - This view **joins `orders` and `customers`** tables to display **order numbers and customer names**.

4. **Output Example**
   ```sql
   SELECT * FROM customer_orders;
   ```
   ```
   +-----------+-------------+
   | order_num | cust_name   |
   +-----------+-------------+
   |     20005 | Coyote Inc. |
   |     20006 | Mouse House |
   +-----------+-------------+
   ```

5. **Tip: Viewing Created Views**
   - To see the views created in the database:
     ```sql
     SHOW FULL TABLES WHERE TABLE_TYPE = 'VIEW';
     ```
   - To see the definition of a specific view:
     ```sql
     SHOW CREATE VIEW customer_orders;
     ```

---

## **Step 3: Using Views to Simplify Complex Joins**
1. **Purpose**
   - Views can **simplify complex joins** by **encapsulating join logic** in a virtual table.

2. **Example: Simplifying a Join with a View**
   ```sql
   CREATE VIEW order_details AS
   SELECT o.order_num, c.cust_name, oi.prod_id, oi.quantity
   FROM orders o
   INNER JOIN customers c ON o.cust_id = c.cust_id
   INNER JOIN orderitems oi ON o.order_num = oi.order_num;
   ```
   - This view **joins `orders`, `customers`, and `orderitems`** to display order details.

3. **Using the View**
   ```sql
   SELECT * FROM order_details WHERE cust_name = 'Coyote Inc.';
   ```
   - This is **much simpler** than writing the complex join query every time.

---

## **Step 4: Using Views to Reformat Retrieved Data**
1. **Purpose**
   - Views can be used to **reformat data** without altering the underlying tables.
   - Useful for **aliasing columns** or **concatenating values**.

2. **Example: Formatting Customer Names**
   ```sql
   CREATE VIEW formatted_customers AS
   SELECT cust_id,
          CONCAT(cust_name, ' (', cust_contact, ')') AS full_contact
   FROM customers;
   ```
   - This **concatenates `cust_name` and `cust_contact`** to provide a more readable format.

3. **Using the View**
   ```sql
   SELECT * FROM formatted_customers;
   ```
   - This displays customer names **with contact details**.

---

## **Step 5: Using Views to Filter Unwanted Data**
1. **Purpose**
   - Views can **filter out sensitive or unnecessary data**, enhancing **security and usability**.

2. **Example: Hiding Sensitive Data**
   ```sql
   CREATE VIEW public_customers AS
   SELECT cust_name, cust_city, cust_country
   FROM customers;
   ```
   - This **excludes sensitive columns** such as `cust_contact` and `cust_email`.

3. **Using the View**
   ```sql
   SELECT * FROM public_customers;
   ```
   - Only **publicly relevant details** are shown.

---

## **Step 6: Using Views with Calculated Fields**
1. **Purpose**
   - Views can include **calculated fields** for on-the-fly data processing.

2. **Example: Calculating Order Totals**
   ```sql
   CREATE VIEW order_totals AS
   SELECT o.order_num,
          SUM(oi.quantity * oi.item_price) AS total_amount
   FROM orders o
   INNER JOIN orderitems oi ON o.order_num = oi.order_num
   GROUP BY o.order_num;
   ```
   - This **calculates the total amount** for each order.

3. **Using the View**
   ```sql
   SELECT * FROM order_totals;
   ```
   - Shows **order numbers and their total amounts**.

---

## **Step 7: Updating Views**
1. **Purpose**
   - Some views allow **updating data**. However, not all views are updatable.
   - A view is **updatable** if:
     - It references **only one table**.
     - It does not use **aggregates, `DISTINCT`, `GROUP BY`, `HAVING`, or `UNION`**.

2. **Example: Updating Data Through a View**
   ```sql
   CREATE VIEW editable_customers AS
   SELECT cust_id, cust_name, cust_city
   FROM customers;
   ```
   - This view **selects columns** that can be **directly updated**.

3. **Updating Data**
   ```sql
   UPDATE editable_customers
   SET cust_city = 'New City'
   WHERE cust_id = 10001;
   ```
   - This **updates the `cust_city`** column in the underlying `customers` table.

---

## **Step 8: Deleting Views**
1. **Purpose**
   - Views can be **deleted** when no longer needed using `DROP VIEW`.

2. **Syntax**
   ```sql
   DROP VIEW view_name;
   ```

3. **Example: Deleting a View**
   ```sql
   DROP VIEW customer_orders;
   ```
   - This **removes** the `customer_orders` view from the database.

---

## **Step 9: Hands-on Exercises**
### **Objective:**
- Practice creating, using, updating, and deleting views.

### **Tasks:**
1. **Create a View for Customer Orders**
   ```sql
   CREATE VIEW customer_orders AS
   SELECT orders.order_num, customers.cust_name
   FROM orders
   INNER JOIN customers ON orders.cust_id = customers.cust_id;
   ```
2. **Use the View**
   ```sql
   SELECT * FROM customer_orders;
   ```
3. **Update Data Through a View**
   ```sql
   UPDATE editable_customers
   SET cust_city = 'New City'
   WHERE cust_id = 10001;
   ```
4. **Delete the View**
   ```sql
   DROP VIEW customer_orders;
   ```

---

## **Conclusion**
By following this manual, you will learn:
- How to create, update, and delete views.
- How to use views to simplify complex joins.
- How to filter unwanted data and reformat retrieved data.
- How to use calculated fields in views.