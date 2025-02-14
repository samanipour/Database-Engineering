# **Filtering Data**

## **Objective**
This manual will guide students through using the `WHERE` clause to filter data in MySQL. Students will learn to filter by single values, nonmatches, value ranges, and NULL values.

---

## **Step 1: Using the WHERE Clause**
1. **What is the WHERE Clause?**
   - The `WHERE` clause **filters data** in a `SELECT` statement, retrieving only the rows that meet specific criteria.
   - It is placed **after the `FROM` clause** in a `SELECT` statement.

2. **Basic Syntax**
   ```sql
   SELECT column_name
   FROM table_name
   WHERE condition;
   ```
   - `WHERE` specifies the condition to filter the data.

3. **Example**
   ```sql
   SELECT prod_name, prod_price
   FROM products
   WHERE prod_price = 2.50;
   ```
   - This retrieves all products priced at `2.50`.

4. **Output Example**
   ```
   +---------------+---------------+
   | prod_name     | prod_price    |
   +---------------+---------------+
   | Carrots       |        2.50   |
   | TNT (1 stick) |        2.50   |
   +---------------+---------------+
   ```

---

## **Step 2: WHERE Clause Operators**
1. **What are Operators?**
   - Operators **join or change clauses** within a `WHERE` clause.
   - They specify the condition for filtering data.

2. **Common WHERE Clause Operators**
   - `=`: **Equality** (e.g., `WHERE prod_price = 2.50`)
   - `<>` or `!=`: **Nonequality** (e.g., `WHERE prod_price <> 2.50`)
   - `<`: **Less than** (e.g., `WHERE prod_price < 10`)
   - `<=`: **Less than or equal to** (e.g., `WHERE prod_price <= 10`)
   - `>`: **Greater than** (e.g., `WHERE prod_price > 10`)
   - `>=`: **Greater than or equal to** (e.g., `WHERE prod_price >= 10`)
   - `BETWEEN`: **Range of values** (e.g., `WHERE prod_price BETWEEN 5 AND 10`)
   - `IS NULL`: **No value** (e.g., `WHERE prod_price IS NULL`)

---

## **Step 3: Checking Against a Single Value**
1. **Purpose**
   - Filter data **exactly matching** a given value.

2. **Syntax**
   ```sql
   SELECT column_name
   FROM table_name
   WHERE column_name = value;
   ```

3. **Example**
   ```sql
   SELECT prod_name, prod_price
   FROM products
   WHERE prod_name = 'Fuses';
   ```

4. **Output Example**
   ```
   +-----------+--------------+
   | prod_name | prod_price   |
   +-----------+--------------+
   | Fuses     |        3.42  |
   +-----------+--------------+
   ```

5. **Note: Case Insensitivity**
   - By default, MySQL is **not case-sensitive** when performing matches.
   - In the example above, `fuses` and `Fuses` both match.

---

## **Step 4: Checking for Nonmatches**
1. **Purpose**
   - Filter data that **does not match** a given value.

2. **Syntax**
   ```sql
   SELECT column_name
   FROM table_name
   WHERE column_name <> value;
   ```
   - Alternatively, use `!=` instead of `<>`.

3. **Example**
   ```sql
   SELECT vend_id, prod_name
   FROM products
   WHERE vend_id <> 1003;
   ```

4. **Output Example**
   ```
   +---------+----------------+
   | vend_id | prod_name      |
   +---------+----------------+
   | 1001    | .5 ton anvil   |
   | 1001    | 1 ton anvil    |
   | 1001    | 2 ton anvil    |
   | 1002    | Fuses          |
   | 1005    | JetPack 1000   |
   | 1005    | JetPack 2000   |
   | 1002    | Oil can        |
   +---------+----------------+
   ```

5. **Tip: Using <> and != Interchangeably**
   - `!=` and `<>` are interchangeable and both mean **not equal**.

---

## **Step 5: Checking for a Range of Values**
1. **Purpose**
   - Filter data within a **specific range**.

2. **Syntax**
   ```sql
   SELECT column_name
   FROM table_name
   WHERE column_name BETWEEN low_value AND high_value;
   ```
   - This retrieves all values within the range, **including** the endpoints.

3. **Example**
   ```sql
   SELECT prod_name, prod_price
   FROM products
   WHERE prod_price BETWEEN 5 AND 10;
   ```

4. **Output Example**
   ```
   +----------------+---------------+
   | prod_name      | prod_price    |
   +----------------+---------------+
   | .5 ton anvil   |        5.99   |
   | 1 ton anvil    |        9.99   |
   | Bird seed      |       10.00   |
   | Oil can        |        8.99   |
   | TNT (5 sticks) |       10.00   |
   +----------------+---------------+
   ```

5. **Tip: BETWEEN Includes Endpoints**
   - `BETWEEN` includes both **low_value** and **high_value** in the result set.

---

## **Step 6: Checking for No Value (NULL)**
1. **Purpose**
   - Filter data where a column contains **no value** (`NULL`).

2. **Syntax**
   ```sql
   SELECT column_name
   FROM table_name
   WHERE column_name IS NULL;
   ```

3. **Example**
   ```sql
   SELECT cust_name
   FROM customers
   WHERE cust_email IS NULL;
   ```

4. **Output Example**
   ```
   +------------+
   | cust_name  |
   +------------+
   | Mouse House|
   | E Fudd     |
   +------------+
   ```

5. **Note: Using IS NULL**
   - Use `IS NULL` to check for `NULL` values.
   - **Do not** use `= NULL`, as this will not work in MySQL.

---

## **Step 7: Hands-on Exercises**
### **Objective:**
- Practice using the `WHERE` clause with different operators.

### **Tasks:**
1. **Filter by Exact Value**
   ```sql
   SELECT prod_name, prod_price
   FROM products
   WHERE prod_price = 2.50;
   ```
2. **Filter by Nonmatch**
   ```sql
   SELECT vend_id, prod_name
   FROM products
   WHERE vend_id <> 1003;
   ```
3. **Filter by Value Range**
   ```sql
   SELECT prod_name, prod_price
   FROM products
   WHERE prod_price BETWEEN 5 AND 10;
   ```
4. **Filter by NULL Value**
   ```sql
   SELECT cust_name
   FROM customers
   WHERE cust_email IS NULL;
   ```

---

## **Conclusion**
By following this manual, you will learn:
- How to filter data using the `WHERE` clause.
- How to check for equality, nonequality, value ranges, and NULL values.
- How to use `BETWEEN`, `<>`, `!=`, and `IS NULL` effectively.