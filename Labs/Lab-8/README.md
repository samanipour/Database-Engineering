# **Searching Using Regular Expressions**

## **Objective**
This manual will guide you through using **regular expressions** in MySQL `WHERE` clauses for advanced data filtering. Students will learn about character matching, OR matches, matching specific characters, using ranges, escaping special characters, using character classes, matching multiple instances, and using anchors.

---

## **Step 1: Understanding Regular Expressions in MySQL**
1. **What Are Regular Expressions?**
   - **Regular expressions** are patterns used to match text within strings.
   - They provide **greater control** over data filtering compared to `LIKE` and wildcards.

2. **MySQL and Regular Expressions**
   - In MySQL, regular expressions are used in `WHERE` clauses with the `REGEXP` keyword.
   - Example:
     ```sql
     SELECT prod_name
     FROM products
     WHERE prod_name REGEXP '1000';
     ```
     - This retrieves product names containing '1000' anywhere in the text.

3. **Difference Between LIKE and REGEXP**
   - `LIKE` matches the **entire** column value unless wildcards are used.
   - `REGEXP` matches **substrings** within column values.

---

## **Step 2: Basic Character Matching**
1. **Purpose**
   - Match text **literally** or using **special characters** for flexible pattern matching.

2. **Syntax**
   ```sql
   SELECT column_name
   FROM table_name
   WHERE column_name REGEXP 'pattern';
   ```

3. **Example: Literal Match**
   ```sql
   SELECT prod_name
   FROM products
   WHERE prod_name REGEXP '1000';
   ```
   - Matches any product name containing '1000'.

4. **Example: Using Special Characters**
   ```sql
   SELECT prod_name
   FROM products
   WHERE prod_name REGEXP '.000';
   ```
   - The `.` character matches **any single character**. This retrieves:
     ```
     +---------------+
     | prod_name     |
     +---------------+
     | JetPack 1000  |
     | JetPack 2000  |
     +---------------+
     ```

---

## **Step 3: Performing OR Matches**
1. **Purpose**
   - Match **one of several patterns** using the `|` operator.

2. **Syntax**
   ```sql
   SELECT column_name
   FROM table_name
   WHERE column_name REGEXP 'pattern1|pattern2';
   ```

3. **Example: OR Matching**
   ```sql
   SELECT prod_name
   FROM products
   WHERE prod_name REGEXP '1000|2000';
   ```
   - Matches product names containing either '1000' or '2000':
     ```
     +---------------+
     | prod_name     |
     +---------------+
     | JetPack 1000  |
     | JetPack 2000  |
     +---------------+
     ```

4. **Tip: More Than Two OR Conditions**
   - Use multiple OR conditions:
     ```sql
     WHERE column_name REGEXP '1000|2000|3000';
     ```

---

## **Step 4: Matching One of Several Characters**
1. **Purpose**
   - Match **specific characters** using sets enclosed in square brackets `[]`.

2. **Syntax**
   ```sql
   SELECT column_name
   FROM table_name
   WHERE column_name REGEXP '[characters]';
   ```

3. **Example: Character Set Matching**
   ```sql
   SELECT prod_name
   FROM products
   WHERE prod_name REGEXP '[123] Ton';
   ```
   - Matches product names containing '1 Ton', '2 Ton', or '3 Ton':
     ```
     +--------------+
     | prod_name    |
     +--------------+
     | 1 ton anvil  |
     | 2 ton anvil  |
     +--------------+
     ```

4. **Tip: Negating a Character Set**
   - Use `[^]` to **exclude** specific characters:
     ```sql
     WHERE column_name REGEXP '[^123] Ton';
     ```
     - Matches any product name ending in 'Ton' except '1 Ton', '2 Ton', or '3 Ton'.

---

## **Step 5: Matching Ranges**
1. **Purpose**
   - Match characters **within a range**.

2. **Syntax**
   ```sql
   SELECT column_name
   FROM table_name
   WHERE column_name REGEXP '[start-end]';
   ```
   - Example ranges:
     - `[0-9]`: Matches any **digit**.
     - `[a-z]`: Matches any **lowercase letter**.
     - `[A-Z]`: Matches any **uppercase letter**.

3. **Example: Numeric Range Matching**
   ```sql
   SELECT prod_name
   FROM products
   WHERE prod_name REGEXP '[1-5] Ton';
   ```
   - Matches product names containing '1 Ton', '2 Ton', or '.5 Ton':
     ```
     +---------------+
     | prod_name     |
     +---------------+
     | .5 ton anvil  |
     | 1 ton anvil   |
     | 2 ton anvil   |
     +---------------+
     ```

---

## **Step 6: Escaping Special Characters**
1. **Purpose**
   - Match **special characters** by escaping them with double backslashes (`\\`).

2. **Special Characters in Regular Expressions**
   - `.`, `|`, `[`, `]`, `-`, `^`, and others have **special meanings**.
   - To match them literally, **escape** them using double backslashes.

3. **Example: Escaping a Dot (.)**
   ```sql
   SELECT vend_name
   FROM vendors
   WHERE vend_name REGEXP '\\.';
   ```
   - Matches vendor names containing a literal period (e.g., 'Furball Inc.').

---

## **Step 7: Matching Character Classes**
1. **Purpose**
   - Match **predefined sets** of characters using character classes.

2. **Common Character Classes**
   - `[:alnum:]`: Any letter or digit (`[a-zA-Z0-9]`)
   - `[:alpha:]`: Any letter (`[a-zA-Z]`)
   - `[:digit:]`: Any digit (`[0-9]`)
   - `[:space:]`: Any whitespace

3. **Example: Matching Digits**
   ```sql
   SELECT prod_name
   FROM products
   WHERE prod_name REGEXP '[[:digit:]]';
   ```
   - Matches product names containing **any digit**.

---

## **Step 8: Matching Multiple Instances**
1. **Purpose**
   - Match **repetitions** of patterns using repetition metacharacters.

2. **Repetition Metacharacters**
   - `*`: Zero or more matches
   - `+`: One or more matches
   - `?`: Zero or one match
   - `{n}`: Exactly `n` matches
   - `{n,}`: At least `n` matches
   - `{n,m}`: Between `n` and `m` matches

3. **Example: Matching Optional Characters**
   ```sql
   SELECT prod_name
   FROM products
   WHERE prod_name REGEXP '\\([0-9] sticks?\\)';
   ```
   - Matches both 'TNT (1 stick)' and 'TNT (5 sticks)'.

---

## **Conclusion**
By following this manual, students will learn:
- How to use `REGEXP` for complex pattern matching.
- How to perform OR matches, match specific characters, escape special characters, and use character classes.
- How to control repetitions and use anchors for precise text matching.