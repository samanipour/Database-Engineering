# **Creating Advanced Joins**

## **Objective**
This manual will guide you through **advanced joins** in MySQL, including using table aliases, self-joins, natural joins, and outer joins. It also covers using aggregate functions with joins and how to use multiple join conditions.

---

## **Step 1: Using Table Aliases**
1. **What Are Table Aliases?**
   - **Table aliases** are temporary names given to tables within a query.
   - They **shorten SQL syntax** and **enable multiple uses of the same table** in a query.
   - Aliases are used only **during query execution** and **not** returned to the client.

2. **Why Use Table Aliases?**
   - To **simplify complex queries** by using shorter names.
   - To **avoid ambiguity** when the same table is joined multiple times.

3. **Syntax**
   ```sql
   SELECT column_name
   FROM table_name AS alias_name;
   ```
   - `AS` assigns an alias to the table.
   - The alias can be used **anywhere** in the query.

4. **Example: Using Table Aliases**
   ```sql
   SELECT cust_name, cust_contact
   FROM customers AS c, orders AS o, orderitems AS oi
   WHERE c.cust_id = o.cust_id
     AND oi.order_num = o.order_num
     AND prod_id = 'TNT2';
   ```
   - `c` is an alias for `customers`, `o` for `orders`, and `oi` for `orderitems`.

5. **Output Example**
   ```
   +---------------+--------------+
   | cust_name     | cust_contact |
   +---------------+--------------+
   | Coyote Inc.   | Y Lee        |
   | Yosemite Place| Y Sam        |
   +---------------+--------------+
   ```

6. **Tip: Aliases and JOINs**
   - Table aliases can be used in **JOIN clauses**:
     ```sql
     SELECT cust_name, cust_contact
     FROM customers AS c
     INNER JOIN orders AS o ON c.cust_id = o.cust_id
     INNER JOIN orderitems AS oi ON oi.order_num = o.order_num
     WHERE prod_id = 'TNT2';
     ```
   - This improves **readability** and **maintainability** of the query.

---

## **Step 2: Self-Joins**
1. **What Is a Self-Join?**
   - A **self-join** joins a table **with itself**.
   - It is useful for **comparing rows** within the same table.

2. **Why Use Self-Joins?**
   - To find **related rows** in the same table.
   - To **compare rows** within the same table (e.g., finding products from the same vendor).

3. **Example: Finding Products from the Same Vendor**
   ```sql
   SELECT p1.prod_id, p1.prod_name
   FROM products AS p1
   INNER JOIN products AS p2 ON p1.vend_id = p2.vend_id
   WHERE p2.prod_id = 'DTNTR';
   ```
   - `p1` and `p2` are **aliases** for the same `products` table.
   - This query finds all products made by the **same vendor** as product `DTNTR`.

4. **Output Example**
   ```
   +---------+----------------+
   | prod_id | prod_name      |
   +---------+----------------+
   | DTNTR   | Detonator      |
   | FB      | Bird seed      |
   | FC      | Carrots        |
   | SAFE    | Safe           |
   +---------+----------------+
   ```

5. **Tip: Self-Joins Instead of Subqueries**
   - Self-joins can often **replace subqueries** when retrieving data from the same table.
   - They are usually **faster** than subqueries for this purpose.

---

## **Step 3: Natural Joins**
1. **What Is a Natural Join?**
   - A **natural join** automatically joins tables based on columns with the **same names** and **compatible data types**.
   - It eliminates **duplicate columns** from the result.

2. **Why Use Natural Joins?**
   - To **simplify joins** by automatically matching columns.
   - To **avoid explicitly specifying** the join condition.

3. **Syntax**
   ```sql
   SELECT column_name
   FROM table1
   NATURAL JOIN table2;
   ```
   - `NATURAL JOIN` automatically joins on **all columns** with the same name.

4. **Example: Joining Customers and Orders**
   ```sql
   SELECT cust_name, order_num
   FROM customers
   NATURAL JOIN orders;
   ```
   - Joins `customers` and `orders` **on all columns** with the **same name**.

5. **Tip: Use with Caution**
   - Use **only** when columns have the **same name** and **data type**.
   - If columns are **not consistent**, the join may **produce incorrect results**.

---

## **Step 4: Outer Joins**
1. **What Are Outer Joins?**
   - **Outer joins** include rows with **no matching rows** in the other table.
   - Types of outer joins:
     - **LEFT OUTER JOIN**: All rows from the **left** table, matched rows from the **right**.
     - **RIGHT OUTER JOIN**: All rows from the **right** table, matched rows from the **left**.

2. **Syntax**
   ```sql
   SELECT column_name
   FROM table1
   LEFT OUTER JOIN table2
   ON table1.common_column = table2.common_column;
   ```

3. **Example: Left Outer Join**
   ```sql
   SELECT customers.cust_id, orders.order_num
   FROM customers
   LEFT OUTER JOIN orders ON customers.cust_id = orders.cust_id;
   ```
   - Lists all customers, **including those with no orders**.

4. **Output Example**
   ```
   +---------+-----------+
   | cust_id | order_num |
   +---------+-----------+
   |   10001 |     20005 |
   |   10002 |      NULL |
   |   10003 |     20006 |
   +---------+-----------+
   ```
   - Customer `10002` has **no orders**, so `order_num` is `NULL`.

5. **Tip: Right Outer Join**
   - Use `RIGHT OUTER JOIN` to **include all rows** from the **right** table.
   ```sql
   SELECT customers.cust_id, orders.order_num
   FROM customers
   RIGHT OUTER JOIN orders ON orders.cust_id = customers.cust_id;
   ```

---

## **Step 5: Using Joins with Aggregate Functions**
1. **Purpose**
   - **Summarize data** across multiple tables.
   - **Combine joins** with aggregate functions like `COUNT()`, `SUM()`, `AVG()`, `MIN()`, and `MAX()`.

2. **Example: Counting Orders per Customer**
   ```sql
   SELECT customers.cust_name,
          COUNT(orders.order_num) AS num_ord
   FROM customers
   LEFT OUTER JOIN orders ON customers.cust_id = orders.cust_id
   GROUP BY customers.cust_id;
   ```
   - This shows the **number of orders per customer**, including customers **with no orders**.

3. **Output Example**
   ```
   +----------------+---------+
   | cust_name      | num_ord |
   +----------------+---------+
   | Coyote Inc.    |       2 |
   | Mouse House    |       0 |
   +----------------+---------+
   ```

---

## **Conclusion**
By following this manual, students will learn:
- How to use **table aliases** to simplify queries.
- How to perform **self-joins** for comparing rows within the same table.
- How to use **natural joins** for automatic joins on matching columns.
- How to use **outer joins** to include rows with no matching counterparts.
- How to combine **joins with aggregate functions** for advanced data summarization.