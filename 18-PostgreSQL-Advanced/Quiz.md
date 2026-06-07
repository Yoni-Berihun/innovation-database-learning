# Module 18: Quiz — PostgreSQL Advanced Features

> **Instructions:** Answer the following questions to check your understanding. Answers and explanations are hidden under each question button.

---

## 🔄 True or False

### Question 1
A CTE (Common Table Expression) creates a permanent table that stays in the database after the query finishes.

- [ ] True
- [x] False

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **False**
* **Why:** A CTE is strictly temporary. It only exists in memory for the duration of that single query execution loop and is immediately discarded afterward.
</details>

---

### Question 2
Window functions collapse rows into summary rows, just like `GROUP BY`.

- [ ] True
- [x] False

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **False**
* **Why:** This is the core difference. `GROUP BY` collapses individual rows into a single summary row. Window functions perform calculations across rows while keeping every original row fully intact in the final output.
</details>

---

### Question 3
A PostgreSQL function can return a table with multiple rows.

- [x] True
- [ ] False

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **True**
* **Why:** By using the `RETURNS TABLE (...)` syntax combined with the `RETURN QUERY` statement, a PL/pgSQL function can return a full multi-row, multi-column dataset.
</details>

---

### Question 4
A `BEFORE UPDATE` trigger runs after the row has already been changed in the table.

- [ ] True
- [x] False

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **False**
* **Why:** A `BEFORE UPDATE` trigger fires *before* the modifications are officially written to disk. This allows the trigger function to intercept, validate, or modify the incoming data fields (via the `NEW` record structure) ahead of time.
</details>

---

### Question 5
`DENSE_RANK()` handles ties without leaving gaps in the ranking sequence.

- [x] True
- [ ] False

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **True**
* **Why:** If two rows tie for 2nd place, `DENSE_RANK()` gives them both a `2` and moves directly to `3` for the next row (1, 2, 2, 3). In contrast, `RANK()` leaves a gap and skips to `4` (1, 2, 2, 4).
</details>

---

## 🔘 Multiple Choice

### Question 6
What keyword starts a Common Table Expression?

- [ ] A) `CTE`
- [x] B) `WITH`
- [ ] C) `AS`
- [ ] D) `TEMP`

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **B) WITH**
* **Why:** The `WITH` keyword tells the PostgreSQL query parser that you are launching a Common Table Expression block to define one or more named temporary result sets.
</details>

---

### Question 7
Which clause in a window function defines how to group rows into separate windows?

- [ ] A) `ORDER BY`
- [ ] B) `GROUP BY`
- [x] C) `PARTITION BY`
- [ ] D) `WINDOW`

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **C) PARTITION BY**
* **Why:** `PARTITION BY` divides the target rows into distinct operational windows (subsets). The window function then runs independently inside each separate partition (e.g., grouping by department).
</details>

---

### Question 8
What is the main difference between a function and a procedure in PostgreSQL?

- [ ] A) Functions are faster than procedures.
- [x] B) Functions return a value; procedures do not have to.
- [ ] C) Procedures can only insert data; functions can only select data.
- [ ] D) Functions are written in SQL; procedures are written in PL/pgSQL.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **B) Functions return a value; procedures do not have to.**
* **Why:** A function must always declare a `RETURNS` type and push back a result value, allowing it to be used inside standard `SELECT` statements. Procedures focus on structural actions, omit the returns header, and are called using `CALL`.
</details>

---

### Question 9
Which window function assigns a unique sequential integer to each row, with no ties?

- [ ] A) `RANK()`
- [ ] B) `DENSE_RANK()`
- [x] C) `ROW_NUMBER()`
- [ ] D) `NTILE()`

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **C) ROW_NUMBER()**
* **Why:** `ROW_NUMBER()` outputs a continuous, unique sequential counter (1, 2, 3...) row-by-row based on the window order, completely ignoring ties.
</details>

---

### Question 10
What does `NEW` represent in a `BEFORE INSERT` trigger?

- [ ] A) The old values of the row before the insert.
- [x] B) The new values of the row being inserted.
- [ ] C) The name of the table.
- [ ] D) The timestamp of the transaction.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **B) The new values of the row being inserted.**
* **Why:** In trigger design architectures, the special record variable `NEW` holds the incoming data row payload for `INSERT` or `UPDATE` events, allowing you to read or alter fields before saving them.
</details>

---

## ✏️ Fill in the Blank

### Question 11
The SQL clause used to define a named temporary result set is `_________`.

```text
WITH
```

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** `WITH` (or `WITH clause` / `Common Table Expression` / `CTE`)
* **Why:** Initiating your queries using the standard `WITH` keyword establishes a named, transient result framework that shields logic from heavy nested syntax loops.
</details>

---

### Question 12
In a window function, the `_________` clause determines the order of rows within each window.

```text
ORDER BY
```

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** `ORDER BY`
* **Why:** The `ORDER BY` clause inside the `OVER (...)` boundary tells the window function exactly how to sequence rows chronologically or numerically before computing outputs (like ranks or cumulative totals).
</details>

---

## ✍️ Short Answer

### Question 13
Explain in 2–3 sentences why you would use a CTE instead of a nested subquery.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Answer:** A CTE significantly improves query readability by breaking complex database logic down into clean, named, step-by-step code modules. Unlike deeply nested traditional subqueries, which read from the "inside-out" and force you to duplicate code blocks, a CTE lets you define a calculation once and clean reference it multiple times throughout the main outer query.
</details>

---

### Question 14
Describe a real-world university scenario where a trigger would be more reliable than having application code enforce a rule.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Answer:** Imagine a data logging rule where an `updated_at` timestamp column must refresh automatically whenever a student record changes. Enforcing this rule inside a database trigger is far more reliable because it executes directly on the database engine, capturing changes regardless of whether a student profile is modified by the web portal, the registrar's internal tool, or a direct administrative script. If five different backend applications interact with the `students` table, a trigger guarantees 100% compliance without requiring every single application team to write the exact same application-level logic.
</details>
