# **Sorting Retrieved Data**

## **Objective**
This manual will guide students through sorting data retrieved from a MySQL database using the `ORDER BY` clause, including sorting by one or more columns, by column position, and by specifying sort direction.

---

## **Step 1: Understanding the Need for Sorting Data**
1. **Why Sort Data?**
   - Data retrieved from a database is **unsorted by default**, typically appearing in the order it was added or stored in the database.
   - The order is unreliable and can change if data is updated or deleted.
   - **Sorting** helps organize the output for better readability and analysis.

2. **ORDER BY Clause**
   - The `ORDER BY` clause is used to **explicitly control** the order in which data is displayed.
   - It can sort data **alphabetically, numerically**, or by **date**.

---

## **Step 2: Sorting Data by a Single Column**
1. **Purpose**
   - Sort data by a **specific column** in ascending (default) or descending order.

2. **Syntax**
   ```sql
   SELECT column_name
   FROM table_name
   ORDER BY column_name;
   ```

3. **Example: Sorting Alphabetically**
   ```sql
   SELECT prod_name
   FROM products
   ORDER BY prod_name;
   ```
   - This sorts product names alphabetically (A to Z) by default.

4. **Output Example**
   ```
   +----------------+
   | prod_name      |
   +----------------+
   | .5 ton anvil   |
   | 1 ton anvil    |
   | 2 ton anvil    |
   | Bird seed      |
   | Carrots        |
   | Detonator      |
   | Fuses          |
   | JetPack 1000   |
   | JetPack 2000   |
   | Oil can        |
   | Safe           |
   | Sling          |
   | TNT (1 stick)  |
   | TNT (5 sticks) |
   +----------------+
   ```

5. **Note: Default Sort Order**
   - The default order is **ascending**. Use `ASC` explicitly if needed:
     ```sql
     ORDER BY column_name ASC;
     ```

---

## **Step 3: Sorting by Multiple Columns**
1. **Purpose**
   - Sort data by **more than one column** to create a more organized output.
   - Example Use Case:
     - Sorting an employee list first by **last name** and then by **first name**.

2. **Syntax**
   ```sql
   SELECT column1, column2
   FROM table_name
   ORDER BY column1, column2;
   ```
   - Columns are sorted in the order they are listed.

3. **Example: Sorting by Price and Name**
   ```sql
   SELECT prod_id, prod_price, prod_name
   FROM products
   ORDER BY prod_price, prod_name;
   ```
   - This sorts by `prod_price` first, and within each price, by `prod_name`.

4. **Output Example**
   ```
   +---------+------------+----------------+
   | prod_id | prod_price | prod_name       |
   +---------+------------+----------------+
   | FC      |       2.50 | Carrots         |
   | TNT1    |       2.50 | TNT (1 stick)   |
   | FU1     |       3.42 | Fuses           |
   | SLING   |       4.49 | Sling           |
   | ANV01   |       5.99 | .5 ton anvil    |
   | OL1     |       8.99 | Oil can         |
   | ANV02   |       9.99 | 1 ton anvil     |
   | FB      |      10.00 | Bird seed       |
   | TNT2    |      10.00 | TNT (5 sticks)  |
   +---------+------------+----------------+
   ```

5. **Tip: Sorting Sequence**
   - The sort order is applied **in sequence**.
   - If the first column has duplicate values, the second column determines the order within those duplicates.

---

## **Step 4: Sorting by Column Position**
1. **Purpose**
   - Sort data by **relative column positions** in the `SELECT` statement instead of by column names.

2. **Syntax**
   ```sql
   SELECT column1, column2, column3
   FROM table_name
   ORDER BY position_number;
   ```
   - Positions are numbered starting at **1**.

3. **Example: Sorting by Position**
   ```sql
   SELECT prod_id, prod_price, prod_name
   FROM products
   ORDER BY 2, 3;
   ```
   - `2` refers to `prod_price`, and `3` refers to `prod_name`.

4. **Output Example**
   ```
   +---------+------------+----------------+
   | prod_id | prod_price | prod_name       |
   +---------+------------+----------------+
   | ANV01   |       5.99 | .5 ton anvil    |
   | OL1     |       8.99 | Oil can         |
   | ANV02   |       9.99 | 1 ton anvil     |
   +---------+------------+----------------+
   ```

5. **Caution: Column Reordering**
   - If the order of columns in `SELECT` changes, the `ORDER BY` clause must also be updated.
   - This approach is **not recommended** for complex queries as it can lead to errors.

---

## **Step 5: Specifying Sort Direction**
1. **Purpose**
   - Control the **ascending (ASC)** or **descending (DESC)** order of sorted data.

2. **Syntax**
   ```sql
   SELECT column_name
   FROM table_name
   ORDER BY column_name DESC;
   ```
   - Use `DESC` for **descending order** (Z to A or highest to lowest).
   - Use `ASC` for **ascending order** (default).

3. **Example: Sorting by Price Descending**
   ```sql
   SELECT prod_id, prod_price, prod_name
   FROM products
   ORDER BY prod_price DESC;
   ```

4. **Output Example**
   ```
   +---------+------------+----------------+
   | prod_id | prod_price | prod_name       |
   +---------+------------+----------------+
   | JP2000  |      55.00 | JetPack 2000    |
   | SAFE    |      50.00 | Safe            |
   | JP1000  |      35.00 | JetPack 1000    |
   +---------+------------+----------------+
   ```

5. **Tip: Sorting on Multiple Columns**
   - Use `DESC` for each column as needed:
     ```sql
     ORDER BY column1 DESC, column2 ASC;
     ```

---

## **Step 6: Hands-on Exercises**
### **Objective:**
- Practice sorting data using various `ORDER BY` options.

### **Tasks:**
1. **Sort by Single Column (Alphabetically)**
   ```sql
   SELECT prod_name
   FROM products
   ORDER BY prod_name;
   ```
2. **Sort by Multiple Columns**
   ```sql
   SELECT prod_id, prod_price, prod_name
   FROM products
   ORDER BY prod_price, prod_name;
   ```
3. **Sort by Column Position**
   ```sql
   SELECT prod_id, prod_price, prod_name
   FROM products
   ORDER BY 2, 3;
   ```
4. **Sort in Descending Order**
   ```sql
   SELECT prod_id, prod_price, prod_name
   FROM products
   ORDER BY prod_price DESC;
   ```

---

## **Conclusion**
By following this manual, you will learn:
- How to sort data using `ORDER BY`.
- How to sort by one or more columns, column positions, and in descending order.
- How to create organized and readable query results.