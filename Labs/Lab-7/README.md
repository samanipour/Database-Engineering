# **Using Wildcard Filtering**

## **Objective**
This manual will guide you through using the `LIKE` operator with wildcards to filter data in MySQL. Students will learn how to use the percent sign (`%`) and underscore (`_`) wildcards for flexible pattern matching.

---

## **Step 1: Understanding Wildcards and the LIKE Operator**
1. **What Are Wildcards?**
   - Wildcards are **special characters** used to match part of a value in SQL.
   - They allow searching for patterns in data, rather than exact matches.

2. **Types of Wildcards in MySQL**
   - **Percent Sign (`%`)**: Matches **zero or more characters**.
   - **Underscore (`_`)**: Matches **exactly one character**.

3. **What is the LIKE Operator?**
   - The `LIKE` operator is used with wildcards to **compare a search pattern** against column values.
   - It is used in the `WHERE` clause of a `SELECT` statement.

4. **Basic Syntax**
   ```sql
   SELECT column_name
   FROM table_name
   WHERE column_name LIKE pattern;
   ```
   - `pattern` is the search pattern with wildcards.

---

## **Step 2: Using the Percent Sign (`%`) Wildcard**
1. **Purpose**
   - Matches **zero or more characters** at any position in the value.
   - Useful for searching substrings within a text.

2. **Syntax**
   ```sql
   SELECT column_name
   FROM table_name
   WHERE column_name LIKE 'pattern%';
   ```
   - `%` can be used:
     - At the end: Matches values **starting with** the pattern.
     - At the beginning: Matches values **ending with** the pattern.
     - At both ends: Matches values **containing** the pattern anywhere.

3. **Example 1: Matching the Beginning**
   ```sql
   SELECT prod_id, prod_name
   FROM products
   WHERE prod_name LIKE 'Jet%';
   ```
   - Matches product names **starting with** 'Jet'.

4. **Output Example**
   ```
   +---------+--------------+
   | prod_id | prod_name    |
   +---------+--------------+
   | JP1000  | JetPack 1000 |
   | JP2000  | JetPack 2000 |
   +---------+--------------+
   ```

5. **Example 2: Matching the End**
   ```sql
   SELECT prod_name
   FROM products
   WHERE prod_name LIKE '%anvil';
   ```
   - Matches product names **ending with** 'anvil'.

6. **Example 3: Matching Anywhere in the Value**
   ```sql
   SELECT prod_id, prod_name
   FROM products
   WHERE prod_name LIKE '%anvil%';
   ```
   - Matches product names **containing** 'anvil' anywhere.

7. **Output Example**
   ```
   +---------+--------------+
   | prod_id | prod_name    |
   +---------+--------------+
   | ANV01   | .5 ton anvil |
   | ANV02   | 1 ton anvil  |
   | ANV03   | 2 ton anvil  |
   +---------+--------------+
   ```

---

## **Step 3: Using the Underscore (`_`) Wildcard**
1. **Purpose**
   - Matches **exactly one character**.
   - Useful for finding strings with a specific character pattern.

2. **Syntax**
   ```sql
   SELECT column_name
   FROM table_name
   WHERE column_name LIKE 'pattern_';
   ```
   - `_` can be used at any position to represent **one character**.

3. **Example: Matching a Single Character**
   ```sql
   SELECT prod_id, prod_name
   FROM products
   WHERE prod_name LIKE '_ ton anvil';
   ```
   - Matches product names with **exactly one character** before ' ton anvil'.

4. **Output Example**
   ```
   +---------+-------------+
   | prod_id | prod_name   |
   +---------+-------------+
   | ANV02   | 1 ton anvil |
   | ANV03   | 2 ton anvil |
   +---------+-------------+
   ```
   - Matches '1 ton anvil' and '2 ton anvil' but **not** '.5 ton anvil' because `.5` has **two characters**.

5. **Tip: Matching Specific Patterns**
   - Use multiple underscores for specific patterns. For example:
     ```sql
     SELECT prod_name
     FROM products
     WHERE prod_name LIKE '__ ton anvil';
     ```
     - Matches product names with **exactly two characters** before ' ton anvil'.

---

## **Step 4: Combining Wildcards**
1. **Purpose**
   - Combine `%` and `_` to create **complex search patterns**.

2. **Example: Complex Pattern Matching**
   ```sql
   SELECT prod_name
   FROM products
   WHERE prod_name LIKE 'J_t%';
   ```
   - Matches product names **starting with 'J'**, followed by **any one character**, and then **any number of characters**.

3. **Output Example**
   ```
   +--------------+
   | prod_name    |
   +--------------+
   | JetPack 1000 |
   | JetPack 2000 |
   +--------------+
   ```

---

## **Step 5: Tips for Using Wildcards**
1. **Use Wildcards Judiciously**
   - Wildcard searches are **slow** as they require scanning all rows.
   - Avoid using `%` at the beginning of patterns if possible, as it results in **full table scans**.

2. **Watch for Trailing Spaces**
   - Trailing spaces can **interfere** with wildcard matching.
   - Example:
     ```sql
     SELECT prod_name
     FROM products
     WHERE prod_name LIKE '%anvil';
     ```
     - If `prod_name` has trailing spaces (e.g., '1 ton anvil '), it **won't match**.
     - Solution: Use `TRIM()` function to remove trailing spaces.

3. **NULL Values**
   - `LIKE` cannot match `NULL` values.
   - Even `LIKE '%'` will **not** match `NULL`.

4. **Use Case Sensitivity**
   - In MySQL, `LIKE` is **case-insensitive** by default for `CHAR` and `VARCHAR` columns.
   - To perform a **case-sensitive** search, use the `BINARY` keyword:
     ```sql
     SELECT prod_name
     FROM products
     WHERE BINARY prod_name LIKE 'Jet%';
     ```

---

## **Step 6: Hands-on Exercises**
### **Objective:**
- Practice using `LIKE` with `%` and `_` wildcards to filter data.

### **Tasks:**
1. **Find All Products Starting with 'T'**
   ```sql
   SELECT prod_name
   FROM products
   WHERE prod_name LIKE 'T%';
   ```
2. **Find All Products Containing 'ton'**
   ```sql
   SELECT prod_name
   FROM products
   WHERE prod_name LIKE '%ton%';
   ```
3. **Find All Products Ending with 'e'**
   ```sql
   SELECT prod_name
   FROM products
   WHERE prod_name LIKE '%e';
   ```
4. **Find Products with Exactly One Character Before ' ton anvil'**
   ```sql
   SELECT prod_name
   FROM products
   WHERE prod_name LIKE '_ ton anvil';
   ```
5. **Find Products with 'J' Followed by Any One Character and Then 't'**
   ```sql
   SELECT prod_name
   FROM products
   WHERE prod_name LIKE 'J_t%';
   ```

---

## **Conclusion**
By following this manual, students will learn:
- How to use the `LIKE` operator with `%` and `_` wildcards.
- How to create flexible search patterns.
- How to use complex pattern matching for data retrieval.
