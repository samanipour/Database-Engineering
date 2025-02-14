# **Using Data Manipulation Functions**

## **Objective**
This manual will guide you through using **data manipulation functions** in MySQL, including text manipulation, date and time manipulation, and numeric manipulation functions.

---

## **Step 1: Understanding Data Manipulation Functions**
1. **What Are Data Manipulation Functions?**
   - **Data manipulation functions** are built-in functions in MySQL used to **convert, format, and manipulate data**.
   - They can be used in `SELECT` statements, `WHERE` clauses, and other parts of SQL queries.

2. **Types of Data Manipulation Functions**
   - **Text Manipulation Functions**: Manipulate strings (e.g., trimming, concatenation, case conversion).
   - **Date and Time Manipulation Functions**: Handle date and time values (e.g., extracting date parts, formatting dates).
   - **Numeric Manipulation Functions**: Perform mathematical operations (e.g., rounding, absolute values).

---

## **Step 2: Using Text Manipulation Functions**
1. **Purpose**
   - Modify or analyze text strings within SQL queries.

2. **Common Text Manipulation Functions**
   - `UPPER()`: Converts a string to uppercase.
   - `LOWER()`: Converts a string to lowercase.
   - `CONCAT()`: Joins two or more strings together.
   - `TRIM()`: Removes leading and trailing spaces.
   - `SUBSTRING()`: Extracts a substring from a string.
   - `LENGTH()`: Returns the length of a string.
   - `REPLACE()`: Replaces occurrences of a substring with another string.

---

### **2.1 Converting Case**
1. **UPPER() and LOWER()**
   - `UPPER()` converts text to uppercase.
   - `LOWER()` converts text to lowercase.

2. **Syntax**
   ```sql
   SELECT UPPER(column_name), LOWER(column_name)
   FROM table_name;
   ```

3. **Example: Converting Vendor Names to Uppercase**
   ```sql
   SELECT vend_name, UPPER(vend_name) AS vend_name_upcase
   FROM vendors
   ORDER BY vend_name;
   ```
   - This converts all vendor names to uppercase while preserving the original case in the `vend_name` column.

4. **Output Example**
   ```
   +----------------+------------------+
   | vend_name      | vend_name_upcase |
   +----------------+------------------+
   | ACME           | ACME             |
   | Anvils R Us    | ANVILS R US      |
   | Furball Inc.   | FURBALL INC.     |
   | Jet Set        | JET SET          |
   +----------------+------------------+
   ```

---

### **2.2 Concatenating Strings**
1. **Purpose**
   - Combine multiple text strings into one output.

2. **Syntax**
   ```sql
   SELECT CONCAT(string1, string2, ...) AS alias_name
   FROM table_name;
   ```

3. **Example: Concatenating Customer Name and City**
   ```sql
   SELECT CONCAT(cust_name, ' (', cust_city, ')') AS customer_details
   FROM customers;
   ```
   - This combines customer names with their city in parentheses.

4. **Output Example**
   ```
   +----------------------+
   | customer_details     |
   +----------------------+
   | John Doe (New York)  |
   | Jane Smith (Chicago) |
   | Jim Brown (Boston)   |
   +----------------------+
   ```

---

### **2.3 Trimming Spaces**
1. **Purpose**
   - Remove unwanted spaces from strings.

2. **Functions**
   - `TRIM()`: Removes spaces from both sides.
   - `LTRIM()`: Removes spaces from the left.
   - `RTRIM()`: Removes spaces from the right.

3. **Example: Removing Spaces from Product Names**
   ```sql
   SELECT TRIM(prod_name) AS trimmed_name
   FROM products;
   ```
   - This removes spaces from both sides of product names.

---

### **2.4 Extracting Substrings**
1. **Purpose**
   - Extract part of a string.

2. **Syntax**
   ```sql
   SELECT SUBSTRING(column_name, start, length) AS alias_name
   FROM table_name;
   ```
   - `start`: Starting position (1-based index).
   - `length`: Number of characters to extract.

3. **Example: Extracting First 3 Letters of Product Names**
   ```sql
   SELECT SUBSTRING(prod_name, 1, 3) AS short_name
   FROM products;
   ```

---

## **Step 3: Using Date and Time Manipulation Functions**
1. **Purpose**
   - Extract, format, and calculate date and time values.

2. **Common Date and Time Functions**
   - `CURDATE()`: Returns the current date.
   - `CURTIME()`: Returns the current time.
   - `NOW()`: Returns the current date and time.
   - `DATE()`: Extracts the date part from a datetime value.
   - `YEAR()`, `MONTH()`, `DAY()`: Extract parts of a date.
   - `DATE_ADD()`, `DATE_SUB()`: Adds or subtracts intervals from dates.
   - `DATE_FORMAT()`: Formats date values.

---

### **3.1 Extracting Date Parts**
1. **Purpose**
   - Extract specific parts of a date (year, month, day).

2. **Syntax**
   ```sql
   SELECT YEAR(column_name), MONTH(column_name), DAY(column_name)
   FROM table_name;
   ```

3. **Example: Extracting Year and Month from Order Dates**
   ```sql
   SELECT order_num, YEAR(order_date) AS order_year, MONTH(order_date) AS order_month
   FROM orders;
   ```

---

### **3.2 Calculating Date Differences**
1. **Purpose**
   - Calculate the difference between two dates.

2. **Syntax**
   ```sql
   SELECT DATEDIFF(date1, date2) AS difference
   FROM table_name;
   ```

3. **Example: Calculating Days Since Order Date**
   ```sql
   SELECT order_num, DATEDIFF(CURDATE(), order_date) AS days_since_order
   FROM orders;
   ```

---

## **Step 4: Using Numeric Manipulation Functions**
1. **Purpose**
   - Perform mathematical calculations on numeric data.

2. **Common Numeric Functions**
   - `ABS()`: Returns the absolute value.
   - `ROUND()`: Rounds a number to a specified decimal place.
   - `CEIL()` or `CEILING()`: Rounds up to the nearest integer.
   - `FLOOR()`: Rounds down to the nearest integer.
   - `MOD()`: Returns the remainder of a division.
   - `RAND()`: Returns a random number.

3. **Example: Rounding Product Prices**
   ```sql
   SELECT prod_name, prod_price, ROUND(prod_price, 1) AS rounded_price
   FROM products;
   ```

---

## **Step 5: Hands-on Exercises**
### **Objective:**
- Practice using various data manipulation functions.

### **Tasks:**
1. **Convert Customer Names to Uppercase**
   ```sql
   SELECT UPPER(cust_name) AS uppercase_name
   FROM customers;
   ```
2. **Concatenate Product Name and Price**
   ```sql
   SELECT CONCAT(prod_name, ' - $', prod_price) AS product_details
   FROM products;
   ```
3. **Extract Year from Order Date**
   ```sql
   SELECT order_num, YEAR(order_date) AS order_year
   FROM orders;
   ```
4. **Calculate Days Since Last Order**
   ```sql
   SELECT order_num, DATEDIFF(CURDATE(), order_date) AS days_since_order
   FROM orders;
   ```

---

## **Conclusion**
By following this manual, students will learn:
- How to manipulate strings using text functions.
- How to work with dates and times in MySQL.
- How to perform calculations using numeric functions.