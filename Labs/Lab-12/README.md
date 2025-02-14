# **Grouping Data**

## **Objective**
This manual will guide you through using the **GROUP BY** and **HAVING** clauses in MySQL to group data and filter groups. Students will learn how to group data for summarization, filter groups, and combine grouping with sorting and data summarization.

---

## **Step 1: Understanding Data Grouping**
1. **What is Data Grouping?**
   - **Grouping** enables you to **divide data into logical sets** for summarization.
   - It allows performing aggregate calculations **on each group** instead of the entire table.

2. **When to Use Grouping?**
   - To get **summaries** of subsets of data, like:
     - Count of products per vendor.
     - Total sales per month.
     - Average price per category.

3. **Aggregate Functions with Grouping**
   - `COUNT()`: Count rows or non-NULL values.
   - `SUM()`: Total sum of numeric values.
   - `AVG()`: Average value of a numeric column.
   - `MAX()`: Maximum value.
   - `MIN()`: Minimum value.

4. **Example Without Grouping**
   ```sql
   SELECT COUNT(*) AS num_prods
   FROM products
   WHERE vend_id = 1003;
   ```
   - This returns the **total number of products** offered by vendor `1003`.

5. **Example With Grouping**
   ```sql
   SELECT vend_id, COUNT(*) AS num_prods
   FROM products
   GROUP BY vend_id;
   ```
   - This returns the **number of products per vendor**.

6. **Output Example**
   ```
   +---------+-------------+
   | vend_id | num_prods    |
   +---------+-------------+
   |     1001 |           3 |
   |     1002 |           2 |
   |     1003 |           7 |
   |     1005 |           2 |
   +---------+-------------+
   ```

---

## **Step 2: Creating Groups Using GROUP BY**
1. **Purpose**
   - `GROUP BY` **groups rows** with the same values into summary rows.
   - It is commonly used with **aggregate functions**.

2. **Syntax**
   ```sql
   SELECT column_name, AGGREGATE_FUNCTION(column_name)
   FROM table_name
   GROUP BY column_name;
   ```

3. **Example: Grouping by Vendor ID**
   ```sql
   SELECT vend_id, COUNT(*) AS num_prods
   FROM products
   GROUP BY vend_id;
   ```
   - Groups products by `vend_id` and **counts the number of products** for each vendor.

4. **Output Example**
   ```
   +---------+-------------+
   | vend_id | num_prods    |
   +---------+-------------+
   |     1001 |           3 |
   |     1002 |           2 |
   |     1003 |           7 |
   |     1005 |           2 |
   +---------+-------------+
   ```

5. **Tip: Grouping by Multiple Columns**
   - You can **nest groups** by using multiple columns in `GROUP BY`:
     ```sql
     SELECT vend_id, prod_price, COUNT(*) AS num_prods
     FROM products
     GROUP BY vend_id, prod_price;
     ```
   - This groups data **first by `vend_id`**, then **by `prod_price`** within each vendor.

---

## **Step 3: Filtering Groups Using HAVING**
1. **Purpose**
   - `HAVING` **filters groups** after grouping, similar to `WHERE`, but for groups.
   - Use `HAVING` to filter groups based on aggregate values.

2. **Syntax**
   ```sql
   SELECT column_name, AGGREGATE_FUNCTION(column_name)
   FROM table_name
   GROUP BY column_name
   HAVING AGGREGATE_FUNCTION(column_name) condition;
   ```

3. **Example: Filtering Groups with HAVING**
   ```sql
   SELECT vend_id, COUNT(*) AS num_prods
   FROM products
   GROUP BY vend_id
   HAVING COUNT(*) >= 2;
   ```
   - This filters the groups to **only include vendors** who have **2 or more products**.

4. **Output Example**
   ```
   +---------+-------------+
   | vend_id | num_prods    |
   +---------+-------------+
   |     1001 |           3 |
   |     1002 |           2 |
   |     1003 |           7 |
   |     1005 |           2 |
   +---------+-------------+
   ```

5. **Difference Between WHERE and HAVING**
   - `WHERE` **filters rows** before grouping.
   - `HAVING` **filters groups** after grouping.
   - Example Using Both:
     ```sql
     SELECT vend_id, COUNT(*) AS num_prods
     FROM products
     WHERE prod_price >= 10
     GROUP BY vend_id
     HAVING COUNT(*) >= 2;
     ```
     - `WHERE`: Filters rows with `prod_price` **greater than or equal to 10**.
     - `HAVING`: Filters groups with **2 or more products**.

---

## **Step 4: Grouping and Sorting Data**
1. **Purpose**
   - Use `ORDER BY` to **sort grouped data**.
   - `GROUP BY` **groups rows**, but the output is **not necessarily sorted**.

2. **Syntax**
   ```sql
   SELECT column_name, AGGREGATE_FUNCTION(column_name)
   FROM table_name
   GROUP BY column_name
   ORDER BY column_name;
   ```

3. **Example: Grouping and Sorting by Product Count**
   ```sql
   SELECT vend_id, COUNT(*) AS num_prods
   FROM products
   GROUP BY vend_id
   ORDER BY num_prods DESC;
   ```
   - This **groups by `vend_id`** and **sorts the result** by the number of products in **descending order**.

4. **Output Example**
   ```
   +---------+-------------+
   | vend_id | num_prods    |
   +---------+-------------+
   |     1003 |           7 |
   |     1001 |           3 |
   |     1002 |           2 |
   |     1005 |           2 |
   +---------+-------------+
   ```

---

## **Step 5: Combining Grouping and Data Summarization**
1. **Purpose**
   - Combine grouping with **data summarization** for detailed reports.

2. **Example: Summarizing Sales by Month**
   ```sql
   SELECT YEAR(order_date) AS order_year,
          MONTH(order_date) AS order_month,
          COUNT(*) AS orders_placed
   FROM orders
   GROUP BY order_year, order_month
   ORDER BY orders_placed DESC;
   ```
   - This **groups orders by year and month** and **counts the number of orders** placed in each month.

3. **Output Example**
   ```
   +------------+-------------+---------------+
   | order_year | order_month | orders_placed |
   +------------+-------------+---------------+
   |       2023 |           9 |             3 |
   |       2023 |          10 |             2 |
   +------------+-------------+---------------+
   ```

---

## **Step 6: Hands-on Exercises**
### **Objective:**
- Practice grouping data with `GROUP BY`, filtering groups with `HAVING`, and sorting grouped data with `ORDER BY`.

### **Tasks:**
1. **Group Products by Vendor and Count Products**
   ```sql
   SELECT vend_id, COUNT(*) AS num_prods
   FROM products
   GROUP BY vend_id;
   ```
2. **Filter Vendors with More Than 3 Products**
   ```sql
   SELECT vend_id, COUNT(*) AS num_prods
   FROM products
   GROUP BY vend_id
   HAVING COUNT(*) > 3;
   ```

---

## **Conclusion**
By following this manual, students will learn:
- How to use `GROUP BY` to group data.
- How to filter groups using `HAVING`.
- How to combine grouping with sorting and summarization.