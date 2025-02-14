# **Joining Tables**

## **Objective**
This manual will guide you through **joining tables** in MySQL, including using `INNER JOIN`, joining multiple tables, and understanding the importance of the `WHERE` clause in joins. Students will learn how to create efficient joins to retrieve related data from multiple tables.

---

## **Step 1: Understanding Joins**
1. **What Are Joins?**
   - **Joins** are used to combine rows from two or more tables based on a **related column**.
   - They allow retrieving data stored across multiple tables in a **single query**.

2. **Why Use Joins?**
   - In relational databases, data is **split across multiple tables** to avoid redundancy.
   - Joins allow combining this data **on-the-fly** to get meaningful results.

3. **Types of Joins in MySQL**
   - **INNER JOIN**: Returns only the matching rows.
   - **LEFT JOIN**: Returns all rows from the left table and matched rows from the right table.
   - **RIGHT JOIN**: Returns all rows from the right table and matched rows from the left table.
   - **CROSS JOIN**: Returns the **Cartesian product** of both tables.

4. **Example Scenario**
   - Consider two tables:
     - `products`: Stores product details, including `prod_id` and `vend_id`.
     - `vendors`: Stores vendor details, including `vend_id` and `vend_name`.
   - To get product names along with their vendor names, **join** both tables using `vend_id`.

---

## **Step 2: Creating a Basic Join**
1. **Purpose**
   - Combine data from two tables **based on a related column**.

2. **Syntax**
   ```sql
   SELECT column_name(s)
   FROM table1, table2
   WHERE table1.common_column = table2.common_column;
   ```
   - This is a **basic join** using the `WHERE` clause.

3. **Example: Joining Products and Vendors**
   ```sql
   SELECT prod_name, vend_name
   FROM products, vendors
   WHERE products.vend_id = vendors.vend_id;
   ```
   - Joins `products` and `vendors` tables using the `vend_id` column.
   - Displays **product names** along with their **vendor names**.

4. **Output Example**
   ```
   +----------------+-------------+
   | prod_name      | vend_name   |
   +----------------+-------------+
   | .5 ton anvil   | Anvils R Us |
   | 1 ton anvil    | Anvils R Us |
   | TNT (5 sticks) | ACME        |
   +----------------+-------------+
   ```

5. **Tip: Using Fully Qualified Column Names**
   - Use fully qualified column names (`table_name.column_name`) to **avoid ambiguity** when multiple tables have columns with the same name.

---

## **Step 3: Using INNER JOIN**
1. **Purpose**
   - An `INNER JOIN` returns **only the rows** that have matching values in both tables.

2. **Syntax**
   ```sql
   SELECT column_name(s)
   FROM table1
   INNER JOIN table2
   ON table1.common_column = table2.common_column;
   ```

3. **Example: Using INNER JOIN for Products and Vendors**
   ```sql
   SELECT prod_name, vend_name
   FROM products
   INNER JOIN vendors
   ON products.vend_id = vendors.vend_id;
   ```
   - This explicitly specifies the **join condition** using `ON`.

4. **Output Example**
   ```
   +----------------+-------------+
   | prod_name      | vend_name   |
   +----------------+-------------+
   | .5 ton anvil   | Anvils R Us |
   | 1 ton anvil    | Anvils R Us |
   | TNT (5 sticks) | ACME        |
   +----------------+-------------+
   ```

5. **Tip: Why Use INNER JOIN?**
   - It is **clearer** and more **maintainable** than using a basic join with `WHERE`.
   - It ensures that **no rows** are returned if **no match** is found.

---

## **Step 4: Joining Multiple Tables**
1. **Purpose**
   - Combine data from **more than two tables** in a single query.

2. **Syntax**
   ```sql
   SELECT column_name(s)
   FROM table1
   INNER JOIN table2 ON table1.common_column = table2.common_column
   INNER JOIN table3 ON table2.other_column = table3.other_column;
   ```

3. **Example: Joining Products, Vendors, and Order Items**
   ```sql
   SELECT prod_name, vend_name, quantity
   FROM vendors
   INNER JOIN products ON vendors.vend_id = products.vend_id
   INNER JOIN orderitems ON orderitems.prod_id = products.prod_id
   WHERE order_num = 20005;
   ```
   - Joins `vendors`, `products`, and `orderitems` tables.
   - Retrieves **product names**, **vendor names**, and **quantity** for order number `20005`.

4. **Output Example**
   ```
   +----------------+-------------+----------+
   | prod_name      | vend_name   | quantity |
   +----------------+-------------+----------+
   | .5 ton anvil   | Anvils R Us |       10 |
   | 1 ton anvil    | Anvils R Us |        3 |
   | TNT (5 sticks) | ACME        |        5 |
   +----------------+-------------+----------+
   ```

5. **Tip: No Limit to Joins**
   - MySQL allows **joining multiple tables** without any limit. However, **performance** can be impacted, so avoid unnecessary joins.

---

## **Step 5: Importance of the WHERE Clause**
1. **Purpose**
   - In a basic join, the `WHERE` clause is used to **specify the join condition**.
   - It acts as a **filter** to include only rows that **match** the join condition.

2. **Example: Join Using WHERE Clause**
   ```sql
   SELECT prod_name, vend_name
   FROM products, vendors
   WHERE products.vend_id = vendors.vend_id
   ORDER BY vend_name, prod_name;
   ```

3. **Output Example**
   ```
   +----------------+-------------+
   | prod_name      | vend_name   |
   +----------------+-------------+
   | Bird seed      | ACME        |
   | Carrots        | ACME        |
   | Detonator      | ACME        |
   | .5 ton anvil   | Anvils R Us |
   | 1 ton anvil    | Anvils R Us |
   +----------------+-------------+
   ```

4. **Tip: Avoiding Cartesian Products**
   - If a join condition is **not** specified, a **Cartesian product** occurs, multiplying all rows from both tables.
   - Example:
     ```sql
     SELECT vend_name, prod_name
     FROM vendors, products;
     ```
     - This matches **every row** from `vendors` with **every row** from `products`, which is usually **not desirable**.

---

## **Step 6: Hands-on Exercises**
### **Objective:**
- Practice joining tables using different join types.

### **Tasks:**
1. **Join Products and Vendors**
   ```sql
   SELECT prod_name, vend_name
   FROM products
   INNER JOIN vendors ON products.vend_id = vendors.vend_id;
   ```
2. **Join Products, Vendors, and Order Items**
   ```sql
   SELECT prod_name, vend_name, quantity
   FROM vendors
   INNER JOIN products ON vendors.vend_id = products.vend_id
   INNER JOIN orderitems ON orderitems.prod_id = products.prod_id
   WHERE order_num = 20005;
   ```
3. **Use WHERE Clause for Basic Join**
   ```sql
   SELECT prod_name, vend_name
   FROM products, vendors
   WHERE products.vend_id = vendors.vend_id;
   ```

---

## **Conclusion**
By following this manual, students will learn:
- How to create basic joins and `INNER JOIN`s.
- How to join multiple tables in a single query.
- The importance of using the `WHERE` clause to avoid Cartesian products.