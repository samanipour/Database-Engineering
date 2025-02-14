# **Full-Text Searching**

## **Objective**
This manual will guide students through **Full-Text Searching** in MySQL, including performing full-text searches, using query expansion, and performing Boolean text searches. Students will learn how to search text fields efficiently using the `MATCH()` and `AGAINST()` functions.

---

## **Step 1: Understanding Full-Text Searching**
1. **What Is Full-Text Searching?**
   - **Full-text searching** is used to **search textual data** for specific words or phrases.
   - It is **optimized for searching large text fields**, like product descriptions or blog posts.

2. **Why Use Full-Text Searching?**
   - To find **relevant results** based on keyword matching.
   - To provide **search functionality** similar to search engines.
   - To **rank results** by relevance.

3. **Full-Text Indexes**
   - Full-text searches require a **full-text index** on the column(s) being searched.
   - Supported in **InnoDB** and **MyISAM** storage engines.

4. **Basic Syntax**
   ```sql
   SELECT column_name
   FROM table_name
   WHERE MATCH(column_name) AGAINST('search_term');
   ```

5. **Example: Simple Full-Text Search**
   ```sql
   SELECT prod_name
   FROM products
   WHERE MATCH(prod_name) AGAINST('anvil');
   ```
   - This searches the `prod_name` column for the word **anvil**.

---

## **Step 2: Setting Up Full-Text Indexes**
1. **Purpose**
   - To enable full-text searching, the column(s) must have a **full-text index**.

2. **Syntax: Creating Full-Text Index**
   ```sql
   CREATE FULLTEXT INDEX index_name
   ON table_name(column_name);
   ```

3. **Example: Creating a Full-Text Index on Product Names**
   ```sql
   CREATE FULLTEXT INDEX idx_prod_name
   ON products(prod_name);
   ```
   - Creates a full-text index on the `prod_name` column in the `products` table.

4. **Tip: Creating Indexes on Multiple Columns**
   - You can create a full-text index on **multiple columns**:
     ```sql
     CREATE FULLTEXT INDEX idx_prod_desc
     ON products(prod_name, prod_desc);
     ```
   - This allows searching both the **name and description**.

---

## **Step 3: Performing Full-Text Searches Using MATCH() and AGAINST()**
1. **Purpose**
   - `MATCH()` specifies the column(s) to be searched.
   - `AGAINST()` specifies the **search term**.

2. **Syntax**
   ```sql
   SELECT column_name
   FROM table_name
   WHERE MATCH(column_name) AGAINST('search_term');
   ```

3. **Example: Searching for a Specific Product**
   ```sql
   SELECT prod_name
   FROM products
   WHERE MATCH(prod_name) AGAINST('anvil');
   ```
   - This searches for the word **anvil** in the `prod_name` column.

4. **Example: Searching Multiple Columns**
   ```sql
   SELECT prod_name, prod_desc
   FROM products
   WHERE MATCH(prod_name, prod_desc) AGAINST('explosive');
   ```
   - This searches both `prod_name` and `prod_desc` columns for **explosive**.

5. **Output Example**
   ```
   +--------------+--------------------------+
   | prod_name    | prod_desc                |
   +--------------+--------------------------+
   | TNT (5 pack) | Highly explosive sticks   |
   | Detonator    | Remote control for TNT    |
   +--------------+--------------------------+
   ```

---

## **Step 4: Using Natural Language Mode**
1. **Purpose**
   - By default, MySQL uses **Natural Language Mode**.
   - It searches for words **without Boolean operators**.

2. **Example: Natural Language Search**
   ```sql
   SELECT prod_name
   FROM products
   WHERE MATCH(prod_name) AGAINST('anvil');
   ```
   - This searches for the word **anvil** in the `prod_name` column.

3. **Output Example**
   ```
   +--------------+
   | prod_name    |
   +--------------+
   | .5 ton anvil |
   | 1 ton anvil  |
   +--------------+
   ```

4. **Tip: Relevance Ranking**
   - Results are **ranked by relevance**.
   - Higher relevance is given to results with **more occurrences** of the search term.

---

## **Step 5: Using Boolean Mode for Advanced Searches**
1. **Purpose**
   - **Boolean Mode** allows the use of **Boolean operators** for more control.
   - Supports:
     - `+` **Must include** this word.
     - `-` **Must not include** this word.
     - `>` **Increases relevance**.
     - `<` **Decreases relevance**.
     - `*` **Wildcard** at the end of a word.
     - `""` **Exact phrase**.

2. **Syntax**
   ```sql
   SELECT column_name
   FROM table_name
   WHERE MATCH(column_name) AGAINST('search_term' IN BOOLEAN MODE);
   ```

3. **Example: Boolean Search for Exact Phrase**
   ```sql
   SELECT prod_name
   FROM products
   WHERE MATCH(prod_name) AGAINST('"1 ton anvil"' IN BOOLEAN MODE);
   ```
   - Searches for the **exact phrase** "1 ton anvil".

4. **Example: Excluding a Word**
   ```sql
   SELECT prod_name
   FROM products
   WHERE MATCH(prod_name) AGAINST('anvil -ton' IN BOOLEAN MODE);
   ```
   - Searches for **anvil** but **excludes** results containing **ton**.

5. **Output Example**
   ```
   +--------------+
   | prod_name    |
   +--------------+
   | Mini anvil   |
   +--------------+
   ```

---

## **Step 6: Using Query Expansion**
1. **Purpose**
   - **Query Expansion** increases the chances of finding relevant matches.
   - It **automatically searches** for words related to the search term.

2. **Syntax**
   ```sql
   SELECT column_name
   FROM table_name
   WHERE MATCH(column_name) AGAINST('search_term' WITH QUERY EXPANSION);
   ```

3. **Example: Searching with Query Expansion**
   ```sql
   SELECT prod_name
   FROM products
   WHERE MATCH(prod_name) AGAINST('explosive' WITH QUERY EXPANSION);
   ```
   - This searches for **explosive** and **related terms**.

---

## **Step 7: Hands-on Exercises**
### **Objective:**
- Practice using full-text searches, Boolean mode, and query expansion.

### **Tasks:**
1. **Create a Full-Text Index on Product Descriptions**
   ```sql
   CREATE FULLTEXT INDEX idx_prod_desc
   ON products(prod_desc);
   ```
2. **Search for a Specific Product**
   ```sql
   SELECT prod_name
   FROM products
   WHERE MATCH(prod_name) AGAINST('anvil');
   ```
3. **Boolean Search for Explosives but Exclude Detonators**
   ```sql
   SELECT prod_name
   FROM products
   WHERE MATCH(prod_name, prod_desc) AGAINST('explosive -detonator' IN BOOLEAN MODE);
   ```
4. **Use Query Expansion for Related Terms**
   ```sql
   SELECT prod_name
   FROM products
   WHERE MATCH(prod_name) AGAINST('explosive' WITH QUERY EXPANSION);
   ```

---

## **Conclusion**
By following this manual, students will learn:
- How to set up full-text indexes.
- How to perform full-text searches using `MATCH()` and `AGAINST()`.
- How to use Boolean mode for advanced searches.
- How to use query expansion for more relevant results.