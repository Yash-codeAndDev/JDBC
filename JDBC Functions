# 🚀 JDBC Stored Functions: Developer's Cheat Sheet

This guide covers the essential knowledge required for implementing database functions in Java and acing technical interviews.

---

## 📌 1. The Core Mechanism
In JDBC, functions are handled via the **`CallableStatement`** interface. Unlike standard queries, functions are designed to return a value to the calling program.

### The Standard Escape Syntax
The JDBC API uses a specific escape sequence to ensure the code works across different databases (Oracle, MySQL, PostgreSQL):

> `{ ? = call function_name(?, ?, ...) }`

* **First `?`**: Represents the return value.
* **Subsequent `?`**: Represent input parameters (`IN`).

---

## 🛠 2. Step-by-Step Implementation



| Step | Action | Method Example |
| :--- | :--- | :--- |
| **1** | Prepare the call | `connection.prepareCall("{ ? = call get_tax(?) }")` |
| **2** | Register Return Type | `cstmt.registerOutParameter(1, Types.DECIMAL)` |
| **3** | Set Input Params | `cstmt.setInt(2, 101)` |
| **4** | Execute | `cstmt.execute()` |
| **5** | Retrieve Result | `cstmt.getBigDecimal(1)` |

---

## ⚖️ 3. Interview Focus: Function vs. Procedure
*Expect this question in 90% of Java/Database interviews.*

| Feature | **Stored Function** | **Stored Procedure** |
| :--- | :--- | :--- |
| **Return Value** | Must return a single value. | Can return multiple or no values. |
| **DML Support** | Primarily for `SELECT`. | Used for `INSERT`, `UPDATE`, `DELETE`. |
| **SQL Integration** | Can be used in a `SELECT` statement. | Cannot be used in a `SELECT` statement. |
| **Transactions** | Cannot use `COMMIT` / `ROLLBACK`. | Can manage transactions internally. |

---

## ⚠️ 4. Critical Pitfalls to Avoid

1.  **Index 1 is the Return Value:** A common bug is setting the first input parameter to index 1. In `{ ? = call...}`, the return value is **always** index 1.
2.  **The `wasNull()` Check:** Java primitives cannot be null. If a DB function returns `NULL`, `getInt()` returns `0`. Always call `cstmt.wasNull()` immediately after fetching a value to verify.
3.  **Datatype Mismatch:** Ensure the `java.sql.Types` constant matches the Database return type (e.g., `VARCHAR` -> `Types.VARCHAR`).
4.  **Resource Leaks:** Always use **Try-with-Resources** to ensure the `CallableStatement` is closed, preventing cursor leaks on the DB server.

---

## 📚 5. Top Functions for Interview Coding Rounds
Be prepared to use these built-in functions within your JDBC `PreparedStatement` or `CallableStatement`:

### A. Data Transformation
* **`COALESCE(col, 'N/A')`**: Replaces `NULL` values with a default string.
* **`NULLIF(val1, val2)`**: Returns null if two values are equal (prevents divide-by-zero errors).

### B. String Manipulation
* **`TRIM()` / `UPPER()` / `LOWER()`**: Standardizing user input before database comparison.
* **`SUBSTR()` / `CONCAT()`**: Formatting data for UI display directly in the query.

### C. Analytical Functions
* **`RANK()` / `DENSE_RANK()`**: Often used in "find the 2nd highest salary" interview questions.
* **`EXTRACT(YEAR FROM date_col)`**: Essential for generating time-based reports.

---

## 💡 Pro-Tip for Senior Roles
If asked about performance, mention that **Functions** are pre-compiled on the database server. This reduces network traffic and execution time compared to sending raw SQL strings from Java.

```java
// Example of the Try-With-Resources Best Practice
try (CallableStatement cstmt = conn.prepareCall("{ ? = call check_inventory(?) }")) {
    cstmt.registerOutParameter(1, Types.BOOLEAN);
    cstmt.setInt(2, productId);
    cstmt.execute();
    boolean isAvailable = cstmt.getBoolean(1);
} catch (SQLException e) {
    e.printStackTrace();
}
