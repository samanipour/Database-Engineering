# **Combining Queries**

## **Objective**
This manual will guide students through **combining queries** in MySQL using the `UNION` operator. It covers how to create combined queries, rules for using `UNION`, including or eliminating duplicate rows, and sorting combined query results.

---

## **Step 1: Understanding Combined Queries**
1. **What Are Combined Queries?**
   - A **combined query** allows you to execute **multiple `SELECT` statements** and **combine their results** into a single result set.
   - It is useful for:
     - Returning **similarly structured data** from different tables.
     - **Combining multiple queries** against a single table.

2. **When to Use Combined Queries?**
   - When retrieving **related data** stored in **separate tables**.
   - When performing **multiple filters** or **conditions** on the same table.
   - When **multiple `WHERE` conditions** become complex.

3. **Tip: Using Combined Queries vs. Multiple WHERE Clauses**
   - Combining queries can **simplify complex `WHERE` conditions**.
   - It can also be **more efficient** than using multiple `OR` conditions.

---

## **Step 2: Creating Combined Queries Using UNION**
1. **What is UNION?**
   - `UNION` is used to **combine the results** of multiple `SELECT` statements into a **single result set**.
   - By default, `UNION` **removes duplicate rows** from the result.

2. **Syntax**
   ```sql
   SELECT column_name(s)
   FROM table1
   WHERE condition
   UNION
   SELECT column_name(s)
   FROM table2
   WHERE condition;
   ```
   - Each `SELECT` statement **must have the same number of columns**.
   - The columns must be of **compatible data types**.

3. **Example: Combining Products by Price and Vendor**
   ```sql
   SELECT vend_id, prod_id, prod_price
   FROM products
   WHERE prod_price <= 5
   UNION
   SELECT vend_id, prod_id, prod_price
   FROM products
   WHERE vend_id IN (1001, 1002);
   ```
   - The first `SELECT` returns products priced **5 or less**.
   - The second `SELECT` returns products made by **vendors 1001 and 1002**.

4. **Output Example**
   ```
   +---------+---------+------------+
   | vend_id | prod_id | prod_price |
   +---------+---------+------------+
   |    1003 | FC      |       2.50 |
   |    1002 | FU1     |       3.42 |
   |    1003 | SLING   |       4.49 |
   |    1003 | TNT1    |       2.50 |
   |    1001 | ANV01   |       5.99 |
   |    1001 | ANV02   |       9.99 |
   |    1001 | ANV03   |      14.99 |
   |    1002 | OL1     |       8.99 |
   +---------+---------+------------+
   ```

5. **Tip: Rules for Using UNION**
   - A `UNION` must have **two or more `SELECT` statements**.
   - Each query must have the **same number of columns**.
   - Column **data types must be compatible** (e.g., numeric types or date types).
   - Column **names do not need to be the same**, but the **order** must be the same.

---

## **Step 3: Including or Eliminating Duplicate Rows**
1. **Purpose**
   - By default, `UNION` **removes duplicate rows**.
   - Use `UNION ALL` to **include all duplicates**.

2. **Example: Using UNION ALL**
   ```sql
   SELECT vend_id, prod_id, prod_price
   FROM products
   WHERE prod_price <= 5
   UNION ALL
   SELECT vend_id, prod_id, prod_price
   FROM products
   WHERE vend_id IN (1001, 1002);
   ```
   - This query **includes duplicate rows** in the result set.

3. **Output Example**
   ```
   +---------+---------+------------+
   | vend_id | prod_id | prod_price |
   +---------+---------+------------+
   |    1003 | FC      |       2.50 |
   |    1002 | FU1     |       3.42 |
   |    1003 | SLING   |       4.49 |
   |    1003 | TNT1    |       2.50 |
   |    1001 | ANV01   |       5.99 |
   |    1001 | ANV02   |       9.99 |
   |    1001 | ANV03   |      14.99 |
   |    1002 | FU1     |       3.42 |
   |    1002 | OL1     |       8.99 |
   +---------+---------+------------+
   ```

4. **Tip: When to Use UNION vs. UNION ALL**
   - Use `UNION` to **remove duplicates**.
   - Use `UNION ALL` to **include all occurrences**, even if duplicated.

---

## **Step 4: Sorting Combined Query Results**
1. **Purpose**
   - Use `ORDER BY` to **sort the combined result set**.
   - Only **one `ORDER BY` clause** is allowed, and it **must** be at the end of the `UNION` query.

2. **Syntax**
   ```sql
   SELECT column_name(s)
   FROM table1
   WHERE condition
   UNION
   SELECT column_name(s)
   FROM table2
   WHERE condition
   ORDER BY column_name;
   ```

3. **Example: Sorting Combined Results**
   ```sql
   SELECT vend_id, prod_id, prod_price
   FROM products
   WHERE prod_price <= 5
   UNION
   SELECT vend_id, prod_id, prod_price
   FROM products
   WHERE vend_id IN (1001, 1002)
   ORDER BY vend_id, prod_price;
   ```
   - This query **sorts the results** by `vend_id` and then by `prod_price`.

4. **Output Example**
   ```
   +---------+---------+------------+
   | vend_id | prod_id | prod_price |
   +---------+---------+------------+
   |    1001 | ANV01   |       5.99 |
   |    1001 | ANV02   |       9.99 |
   |    1001 | ANV03   |      14.99 |
   |    1002 | FU1     |       3.42 |
   |    1002 | OL1     |       8.99 |
   |    1003 | TNT1    |       2.50 |
   |    1003 | FC      |       2.50 |
   |    1003 | SLING   |       4.49 |
   +---------+---------+------------+
   ```

---

## **Step 5: Hands-on Exercises**
### **Objective:**
- Practice combining queries using `UNION` and `UNION ALL`.

### **Tasks:**
1. **Combine Products by Price and Vendor**
   ```sql
   SELECT vend_id, prod_id, prod_price
   FROM products
   WHERE prod_price <= 5
   UNION
   SELECT vend_id, prod_id, prod_price
   FROM products
   WHERE vend_id IN (1001, 1002);
   ```
2. **Include Duplicate Rows with UNION ALL**
   ```sql
   SELECT vend_id, prod_id, prod_price
   FROM products
   WHERE prod_price <= 5
   UNION ALL
   SELECT vend_id, prod_id, prod_price
   FROM products
   WHERE vend_id IN (1001, 1002);
   ```

---

## **Conclusion**
By following this manual, students will learn:
- How to create combined queries using `UNION`.
- How to include or eliminate duplicate rows.
- How to sort combined query results.