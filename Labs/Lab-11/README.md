# **Summarizing Data**

## **Objective**
This manual will guide you through using **aggregate functions** in MySQL to summarize data, including calculating averages, counts, maximums, minimums, sums, and combining aggregate functions.

---

## **Step 1: Understanding Aggregate Functions**
1. **What Are Aggregate Functions?**
   - Aggregate functions **perform calculations** on multiple rows of data and return a **single result**.
   - They are commonly used to **summarize data** in reports or analytics.

2. **Common Aggregate Functions**
   - `AVG()`: **Average** of a numeric column.
   - `COUNT()`: **Number of rows** or non-NULL values.
   - `MAX()`: **Maximum value** in a column.
   - `MIN()`: **Minimum value** in a column.
   - `SUM()`: **Sum of numeric** column values.

3. **Important Notes**
   - Aggregate functions **ignore `NULL` values**.
   - They are often used with the `GROUP BY` clause to summarize data by groups.

---

## **Step 2: Calculating Averages Using AVG()**
1. **Purpose**
   - Calculate the **average** value of a numeric column.

2. **Syntax**
   ```sql
   SELECT AVG(column_name) AS alias_name
   FROM table_name;
   ```

3. **Example: Calculating Average Product Price**
   ```sql
   SELECT AVG(prod_price) AS average_price
   FROM products;
   ```
   - This calculates the average price of all products.

4. **Output Example**
   ```
   +--------------+
   | average_price|
   +--------------+
   |        11.94 |
   +--------------+
   ```

5. **Tip: Calculating Averages by Group**
   - Use `GROUP BY` to calculate averages by group:
     ```sql
     SELECT vend_id, AVG(prod_price) AS average_price
     FROM products
     GROUP BY vend_id;
     ```
   - This calculates the average price of products **by vendor**.

---

## **Step 3: Counting Rows and Values Using COUNT()**
1. **Purpose**
   - Count the **number of rows** or **non-NULL values** in a column.

2. **Syntax**
   ```sql
   SELECT COUNT(column_name) AS alias_name
   FROM table_name;
   ```

3. **Examples**
   - **Counting All Rows**:
     ```sql
     SELECT COUNT(*) AS total_products
     FROM products;
     ```
     - Counts **all rows** in the `products` table, including those with `NULL` values.

   - **Counting Non-NULL Values**:
     ```sql
     SELECT COUNT(prod_price) AS priced_products
     FROM products;
     ```
     - Counts only the rows with a **non-NULL price**.

4. **Output Example**
   ```
   +----------------+
   | total_products |
   +----------------+
   |             14 |
   +----------------+
   ```

5. **Tip: Counting Distinct Values**
   - Use `COUNT(DISTINCT column_name)` to count **unique values**:
     ```sql
     SELECT COUNT(DISTINCT vend_id) AS unique_vendors
     FROM products;
     ```
   - This counts the **number of unique vendors**.

---

## **Step 4: Finding Maximum and Minimum Values Using MAX() and MIN()**
1. **Purpose**
   - Find the **maximum** or **minimum** value in a column.

2. **Syntax**
   ```sql
   SELECT MAX(column_name) AS alias_name
   FROM table_name;
   ```
   ```sql
   SELECT MIN(column_name) AS alias_name
   FROM table_name;
   ```

3. **Examples**
   - **Finding Maximum Price**:
     ```sql
     SELECT MAX(prod_price) AS highest_price
     FROM products;
     ```
     - Finds the most expensive product price.

   - **Finding Minimum Price**:
     ```sql
     SELECT MIN(prod_price) AS lowest_price
     FROM products;
     ```
     - Finds the cheapest product price.

4. **Output Example**
   ```
   +--------------+
   | highest_price|
   +--------------+
   |         55.00|
   +--------------+
   ```

5. **Tip: Using MAX() and MIN() with Dates**
   - These functions can be used with dates to find the **latest** or **earliest** dates:
     ```sql
     SELECT MIN(order_date) AS first_order, MAX(order_date) AS last_order
     FROM orders;
     ```
   - This retrieves the **first** and **last order dates**.

---

## **Step 5: Summing Values Using SUM()**
1. **Purpose**
   - Calculate the **sum** of a numeric column.

2. **Syntax**
   ```sql
   SELECT SUM(column_name) AS alias_name
   FROM table_name;
   ```

3. **Example: Calculating Total Sales**
   ```sql
   SELECT SUM(quantity * item_price) AS total_sales
   FROM orderitems;
   ```
   - This calculates the **total sales** by multiplying `quantity` and `item_price`.

4. **Output Example**
   ```
   +------------+
   | total_sales|
   +------------+
   |     1495.87|
   +------------+
   ```

5. **Tip: Summing by Group**
   - Use `GROUP BY` to sum values by group:
     ```sql
     SELECT vend_id, SUM(prod_price) AS total_price
     FROM products
     GROUP BY vend_id;
     ```
   - This calculates the total price of products **by vendor**.

---

## **Step 6: Using Aggregates on Distinct Values**
1. **Purpose**
   - Apply aggregate functions to **distinct** values to avoid counting duplicates.

2. **Syntax**
   ```sql
   SELECT AGGREGATE_FUNCTION(DISTINCT column_name) AS alias_name
   FROM table_name;
   ```

3. **Example: Counting Unique Prices**
   ```sql
   SELECT COUNT(DISTINCT prod_price) AS unique_prices
   FROM products;
   ```
   - This counts the **number of unique prices** in the `products` table.

---

## **Step 7: Combining Aggregate Functions**
1. **Purpose**
   - Use **multiple aggregate functions** in a single query for detailed summaries.

2. **Example: Combining Multiple Aggregates**
   ```sql
   SELECT COUNT(*) AS total_products,
          AVG(prod_price) AS average_price,
          MIN(prod_price) AS lowest_price,
          MAX(prod_price) AS highest_price,
          SUM(prod_price) AS total_price
   FROM products;
   ```
   - This calculates the **total count**, **average price**, **minimum price**, **maximum price**, and **sum of prices**.

3. **Output Example**
   ```
   +----------------+--------------+--------------+--------------+--------------+
   | total_products | average_price| lowest_price | highest_price| total_price  |
   +----------------+--------------+--------------+--------------+--------------+
   |             14 |        11.94 |         2.50 |        55.00 |        167.13|
   +----------------+--------------+--------------+--------------+--------------+
   ```

---

## **Step 8: Hands-on Exercises**
### **Objective:**
- Practice using various aggregate functions.

### **Tasks:**
1. **Calculate Average Price by Vendor**
   ```sql
   SELECT vend_id, AVG(prod_price) AS average_price
   FROM products
   GROUP BY vend_id;
   ```
2. **Find Most Expensive and Cheapest Product**
   ```sql
   SELECT MAX(prod_price) AS highest_price, MIN(prod_price) AS lowest_price
   FROM products;
   ```
3. **Calculate Total Sales by Order**
   ```sql
   SELECT order_num, SUM(quantity * item_price) AS order_total
   FROM orderitems
   GROUP BY order_num;
   ```

---

## **Conclusion**
By following this manual, students will learn:
- How to use aggregate functions to summarize data.
- How to calculate averages, counts, maximums, minimums, and sums.
- How to combine multiple aggregate functions in one query.