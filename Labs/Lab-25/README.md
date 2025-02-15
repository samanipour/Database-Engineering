# **Managing Transaction Processing**

## **Objective**
This manual will guide you through **managing transaction processing** in MySQL. Students will learn how to use transactions to ensure data integrity, control transactions with `START TRANSACTION`, `COMMIT`, and `ROLLBACK`, and understand the importance of savepoints and changing the default commit behavior.

---

## **Step 1: Understanding Transaction Processing**
1. **What is Transaction Processing?**
   - A **transaction** is a sequence of one or more SQL operations executed as a **single unit of work**.
   - It ensures that the **entire set of operations** is **completed successfully** or **none at all**.

2. **Why Use Transactions?**
   - To **maintain data integrity** by ensuring consistency.
   - To **prevent partial updates** that can corrupt data.
   - To **group related operations** that must be executed together.

3. **Properties of Transactions (ACID)**
   - **Atomicity**: All operations are completed or none are.
   - **Consistency**: Ensures data integrity before and after the transaction.
   - **Isolation**: Transactions are independent of each other.
   - **Durability**: Once committed, changes are permanent.

4. **Example Scenario**
   - Transferring money between two bank accounts:
     - **Debit** one account.
     - **Credit** another account.
     - Both operations must **complete successfully** or **neither should**.

---

## **Step 2: Starting a Transaction**
1. **Purpose**
   - Use `START TRANSACTION` to **begin a new transaction**.
   - It **temporarily suspends auto-commit** mode until a `COMMIT` or `ROLLBACK` is issued.

2. **Syntax**
   ```sql
   START TRANSACTION;
   ```
   - Begins a new transaction.
   - Changes are **not visible** to other users until the transaction is committed.

3. **Example: Starting a Transaction**
   ```sql
   START TRANSACTION;
   UPDATE accounts SET balance = balance - 100 WHERE acc_id = 101;
   UPDATE accounts SET balance = balance + 100 WHERE acc_id = 102;
   ```

4. **Tip: BEGIN and START TRANSACTION**
   - `BEGIN` can also be used to start a transaction.
   - `START TRANSACTION` is more **explicit** and **recommended** for clarity.

---

## **Step 3: Committing a Transaction**
1. **Purpose**
   - Use `COMMIT` to **permanently save changes** made during the transaction.
   - Once committed, changes **cannot be undone**.

2. **Syntax**
   ```sql
   COMMIT;
   ```
   - Saves all changes since the last `START TRANSACTION`.

3. **Example: Committing a Transaction**
   ```sql
   START TRANSACTION;
   UPDATE accounts SET balance = balance - 100 WHERE acc_id = 101;
   UPDATE accounts SET balance = balance + 100 WHERE acc_id = 102;
   COMMIT;
   ```
   - The balance is **transferred** between accounts, and changes are **saved permanently**.

4. **Tip: Auto-commit Mode**
   - By default, MySQL runs in **auto-commit mode**, where every `INSERT`, `UPDATE`, and `DELETE` is **committed immediately**.
   - Starting a transaction **temporarily disables auto-commit** until `COMMIT` or `ROLLBACK` is executed.

---

## **Step 4: Rolling Back a Transaction**
1. **Purpose**
   - Use `ROLLBACK` to **undo all changes** made since the transaction started.
   - It **restores the database** to the state before the `START TRANSACTION`.

2. **Syntax**
   ```sql
   ROLLBACK;
   ```
   - Cancels all changes since the last `START TRANSACTION`.

3. **Example: Rolling Back a Transaction**
   ```sql
   START TRANSACTION;
   UPDATE accounts SET balance = balance - 100 WHERE acc_id = 101;
   UPDATE accounts SET balance = balance + 100 WHERE acc_id = 102;
   ROLLBACK;
   ```
   - All updates are **reversed**, and **no changes** are saved to the database.

4. **When to Use ROLLBACK?**
   - When an **error occurs** during the transaction.
   - When a **business rule validation fails**.
   - To **manually cancel** a set of changes.

---

## **Step 5: Using Savepoints**
1. **What is a Savepoint?**
   - A **savepoint** is a **named point** within a transaction.
   - It allows **rolling back** to a specific point **without** affecting prior changes.

2. **Why Use Savepoints?**
   - To **partially undo** changes within a transaction.
   - To provide **more control** over complex transactions.

3. **Syntax**
   ```sql
   SAVEPOINT savepoint_name;
   ```
   - Creates a savepoint within the current transaction.

   ```sql
   ROLLBACK TO savepoint_name;
   ```
   - Rolls back to the specified savepoint.

   ```sql
   RELEASE SAVEPOINT savepoint_name;
   ```
   - Deletes the savepoint but **retains changes** made after it.

4. **Example: Using Savepoints**
   ```sql
   START TRANSACTION;
   UPDATE accounts SET balance = balance - 100 WHERE acc_id = 101;
   SAVEPOINT before_credit;
   UPDATE accounts SET balance = balance + 100 WHERE acc_id = 102;
   ROLLBACK TO before_credit;
   COMMIT;
   ```
   - The debit is **preserved**, but the credit is **reversed**.

5. **Tip: Using Multiple Savepoints**
   - Multiple savepoints can be created and **rolled back** to in sequence.
   - Example:
     ```sql
     SAVEPOINT step1;
     SAVEPOINT step2;
     ROLLBACK TO step1;
     ```

---

## **Step 6: Changing Default Commit Behavior**
1. **Purpose**
   - By default, MySQL **automatically commits** changes after every `INSERT`, `UPDATE`, or `DELETE`.
   - This behavior can be **disabled** to require explicit `COMMIT`.

2. **Syntax**
   ```sql
   SET autocommit = 0;
   ```
   - Disables auto-commit mode.
   - Changes must be **committed manually**.

   ```sql
   SET autocommit = 1;
   ```
   - Re-enables auto-commit mode.

3. **Example: Disabling Auto-commit**
   ```sql
   SET autocommit = 0;
   START TRANSACTION;
   UPDATE accounts SET balance = balance - 100 WHERE acc_id = 101;
   UPDATE accounts SET balance = balance + 100 WHERE acc_id = 102;
   COMMIT;
   SET autocommit = 1;
   ```
   - Changes are **not committed** until `COMMIT` is executed.
   - `autocommit` is **restored** after the transaction.

4. **Tip: Use with Caution**
   - Always **remember to `COMMIT` or `ROLLBACK`** when auto-commit is disabled.
   - Failing to do so **locks the affected rows** until the session ends.

---

## **Step 7: Hands-on Exercises**
### **Objective:**
- Practice using transactions to maintain data integrity.

### **Tasks:**
1. **Transfer Money Between Accounts**
   ```sql
   START TRANSACTION;
   UPDATE accounts SET balance = balance - 50 WHERE acc_id = 101;
   UPDATE accounts SET balance = balance + 50 WHERE acc_id = 102;
   COMMIT;
   ```
2. **Rollback on Error**
   ```sql
   START TRANSACTION;
   UPDATE accounts SET balance = balance - 50 WHERE acc_id = 101;
   UPDATE accounts SET balance = balance + 50 WHERE acc_id = 999; -- Invalid account
   ROLLBACK;
   ```
3. **Using Savepoints**
   ```sql
   START TRANSACTION;
   UPDATE accounts SET balance = balance - 50 WHERE acc_id = 101;
   SAVEPOINT halfway;
   UPDATE accounts SET balance = balance + 50 WHERE acc_id = 102;
   ROLLBACK TO halfway;
   COMMIT;
   ```

---

## **Conclusion**
By following this manual, you will learn:
- How to use transactions to ensure data integrity.
- How to control transactions using `START TRANSACTION`, `COMMIT`, and `ROLLBACK`.
- How to use savepoints for advanced transaction control.