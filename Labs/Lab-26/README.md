# **Globalization and Localization**

## **Objective**
This manual will guide you through understanding and working with **character sets and collation sequences** in MySQL to support globalization and localization. It covers setting up character sets and collation sequences at different levels (server, database, table, and column) and ensuring consistent data handling for international applications.

---

## **Step 1: Understanding Character Sets and Collation Sequences**
1. **What Is a Character Set?**
   - A **character set** is a **collection of characters** that can be stored in a database.
   - Examples:
     - `utf8`: Supports most characters, including Latin, Greek, and Cyrillic.
     - `utf8mb4`: Supports all Unicode characters, including emojis.
     - `latin1`: Commonly used for Western European languages.

2. **What Is a Collation Sequence?**
   - A **collation sequence** defines **how characters are compared and sorted**.
   - Each character set has one or more collation sequences.
   - Example:
     - `utf8_general_ci`: Case-insensitive comparison.
     - `utf8_bin`: Case-sensitive and binary comparison.

3. **Why Use Character Sets and Collations?**
   - To **store multilingual data**.
   - To **ensure consistent sorting** and **search behavior**.
   - To **avoid data corruption** when storing special characters (e.g., emojis).

4. **Tip: Choosing the Right Character Set**
   - Use `utf8mb4` for maximum compatibility, including emojis.
   - Use `latin1` for simple Western European languages to **save storage space**.

---

## **Step 2: Viewing Default Character Sets and Collations**
1. **Purpose**
   - Check the **default character set and collation** used by your MySQL server.

2. **View Default Settings**
   ```sql
   SHOW VARIABLES LIKE 'character_set_%';
   SHOW VARIABLES LIKE 'collation_%';
   ```
   - This shows the **default character set** and **collation** for the server.

3. **Output Example**
   ```
   +--------------------------+---------+
   | Variable_name            | Value   |
   +--------------------------+---------+
   | character_set_client     | utf8mb4 |
   | character_set_connection | utf8mb4 |
   | character_set_database   | utf8mb4 |
   | character_set_results    | utf8mb4 |
   | character_set_server     | utf8mb4 |
   | collation_connection     | utf8mb4_general_ci |
   | collation_database       | utf8mb4_general_ci |
   | collation_server         | utf8mb4_general_ci |
   +--------------------------+---------+
   ```

4. **Tip: Importance of Default Settings**
   - These settings **influence all new databases and tables** created on the server.
   - Always **verify the default settings** before creating new databases or tables.

---

## **Step 3: Setting Character Sets and Collations at Different Levels**
1. **Purpose**
   - Character sets and collations can be set at different levels:
     - **Server** level: Affects all databases and tables by default.
     - **Database** level: Affects all tables within a database.
     - **Table** level: Affects all columns within a table.
     - **Column** level: Affects only specific columns.

---

### **A. Setting at Server Level**
1. **Change Character Set and Collation Temporarily**
   ```sql
   SET NAMES 'utf8mb4' COLLATE 'utf8mb4_general_ci';
   ```
   - Affects the **current session only**.

2. **Change Character Set and Collation Permanently**
   - Edit the MySQL configuration file (e.g., `my.cnf` or `my.ini`) and add:
     ```
     [mysqld]
     character-set-server=utf8mb4
     collation-server=utf8mb4_general_ci
     ```
   - **Restart MySQL Server** for changes to take effect.

---

### **B. Setting at Database Level**
1. **Create Database with Specific Character Set and Collation**
   ```sql
   CREATE DATABASE mydatabase
   CHARACTER SET utf8mb4
   COLLATE utf8mb4_general_ci;
   ```

2. **Change Character Set and Collation of Existing Database**
   ```sql
   ALTER DATABASE mydatabase
   CHARACTER SET utf8mb4
   COLLATE utf8mb4_general_ci;
   ```
   - This **does not** change existing tables and columns automatically.

---

### **C. Setting at Table Level**
1. **Create Table with Specific Character Set and Collation**
   ```sql
   CREATE TABLE mytable (
       column1 VARCHAR(100)
   ) CHARACTER SET utf8mb4
     COLLATE utf8mb4_general_ci;
   ```

2. **Change Character Set and Collation of Existing Table**
   ```sql
   ALTER TABLE mytable
   CONVERT TO CHARACTER SET utf8mb4
   COLLATE utf8mb4_general_ci;
   ```
   - This **converts existing data** to the new character set and collation.

---

### **D. Setting at Column Level**
1. **Create Column with Specific Character Set and Collation**
   ```sql
   CREATE TABLE mytable (
       column1 VARCHAR(100) CHARACTER SET utf8mb4 COLLATE utf8mb4_bin
   );
   ```
   - This column uses `utf8mb4_bin`, which is **case-sensitive**.

2. **Change Character Set and Collation of Existing Column**
   ```sql
   ALTER TABLE mytable
   MODIFY column1 VARCHAR(100)
   CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
   ```
   - This **modifies** the character set and collation for `column1`.

---

## **Step 4: Comparing and Sorting with Collations**
1. **Purpose**
   - Collations **affect comparisons and sorting** in `SELECT` statements.

2. **Example: Case-Insensitive Comparison**
   ```sql
   SELECT * FROM mytable
   WHERE column1 = 'example' COLLATE utf8mb4_general_ci;
   ```
   - Matches **'example'**, **'Example'**, **'EXAMPLE'**, etc.

3. **Example: Case-Sensitive Comparison**
   ```sql
   SELECT * FROM mytable
   WHERE column1 = 'example' COLLATE utf8mb4_bin;
   ```
   - Matches **only** 'example' (case-sensitive).

4. **Tip: Using COLLATE in ORDER BY**
   ```sql
   SELECT * FROM mytable
   ORDER BY column1 COLLATE utf8mb4_bin;
   ```
   - Sorts data **case-sensitively**.

---

## **Step 5: Hands-on Exercises**
### **Objective:**
- Practice setting and changing character sets and collations.

### **Tasks:**
1. **Check Default Character Set and Collation**
   ```sql
   SHOW VARIABLES LIKE 'character_set_%';
   SHOW VARIABLES LIKE 'collation_%';
   ```

2. **Create Database with Specific Character Set and Collation**
   ```sql
   CREATE DATABASE testdb
   CHARACTER SET utf8mb4
   COLLATE utf8mb4_general_ci;
   ```

3. **Create Table with Case-Sensitive Column**
   ```sql
   CREATE TABLE testtable (
       name VARCHAR(100) CHARACTER SET utf8mb4 COLLATE utf8mb4_bin
   );
   ```

4. **Compare Data Case-Sensitively**
   ```sql
   SELECT * FROM testtable
   WHERE name = 'example' COLLATE utf8mb4_bin;
   ```

---

## **Conclusion**
By following this manual, you will learn:
- How to **check default character sets and collations**.
- How to **set character sets and collations** at different levels.
- How to use `COLLATE` for **case-sensitive and case-insensitive comparisons**.
- How to **sort data** using specific collations.