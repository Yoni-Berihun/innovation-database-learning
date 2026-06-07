# Module 11: Quiz — GROUP BY & HAVING in PostgreSQL

> **Instructions:** Answer the following questions to check your understanding. Answers and explanations are hidden under each question button.

---

## 🔘 Multiple Choice

### Question 1
What does `GROUP BY` do?

- [ ] A) Sorts rows in descending order.
- [x] B) Organizes rows into groups based on column values.
- [ ] C) Deletes duplicate rows from a table.
- [ ] D) Joins two tables together.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **B) Organizes rows into groups based on column values.**
* **Why:** The `GROUP BY` clause gathers rows that share identical values in the specified column(s) into distinct, unique summary clusters or groups.
</details>

---

### Question 2
Which clause is used to filter groups after they have been created?

- [ ] A) WHERE
- [ ] B) ORDER BY
- [x] C) HAVING
- [ ] D) LIMIT

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **C) HAVING**
* **Why:** `HAVING` acts as a post-grouping filter condition. It evaluates the computed groups after the `GROUP BY` clause has organized the data rows.
</details>

---

### Question 3
What is the main difference between `WHERE` and `HAVING`?

- [ ] A) `WHERE` is faster than `HAVING`.
- [x] B) `WHERE` filters rows before grouping; `HAVING` filters groups after grouping.
- [ ] C) `WHERE` can use aggregate functions; `HAVING` cannot.
- [ ] D) There is no difference.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **B) `WHERE` filters rows before grouping; `HAVING` filters groups after grouping.**
* **Why:** This represents the strict execution order in SQL. `WHERE` filters out raw single rows early in the pipeline, whereas `HAVING` processes the resulting groups after aggregations are calculated.
</details>

---

### Question 4
Which of the following is an aggregate function?

- [ ] A) `GROUP BY`
- [ ] B) `ORDER BY`
- [x] C) `COUNT()`
- [ ] D) `WHERE`

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **C) `COUNT()`**
* **Why:** `COUNT()` is an aggregate function because it takes multiple data inputs and performs a summary mathematical computation. `GROUP BY`, `ORDER BY`, and `WHERE` are structural clauses.
</details>

---

### Question 5
You want to find departments with more than 50 students. Which query is correct?

- [ ] A) `SELECT department_id FROM students WHERE COUNT(*) > 50;`
- [x] B) `SELECT department_id, COUNT(*) FROM students GROUP BY department_id HAVING COUNT(*) > 50;`
- [ ] C) `SELECT department_id, COUNT(*) FROM students HAVING COUNT(*) > 50;`
- [ ] D) `SELECT department_id FROM students GROUP BY department_id WHERE COUNT(*) > 50;`

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **B) `SELECT department_id, COUNT(*) FROM students GROUP BY department_id HAVING COUNT(*) > 50;`**
* **Why:** This represents the exact standard syntax order. It explicitly states the non-aggregated column, implements grouping, calculates total rows, and deploys `HAVING` to filter groups containing more than 50 students.
</details>

---

### Question 6
Can you use `HAVING` without `GROUP BY`?

- [ ] A) Yes, always.
- [ ] B) No, `HAVING` only works with `GROUP BY`.
- [x] C) Yes, but it treats all rows as one single group.
- [ ] D) Only if you use `WHERE` first.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **C) Yes, but it treats all rows as one single group.**
* **Why:** Running `HAVING` without a `GROUP BY` statement collapses all table rows into one giant global partition. While valid in ANSI SQL, this is rarely useful in standard reporting tasks.
</details>

---

### Question 7
Which query calculates the average GPA per department?

- [ ] A) `SELECT department_id, AVG(gpa) FROM students;`
- [x] B) `SELECT department_id, AVG(gpa) FROM students GROUP BY department_id;`
- [ ] C) `SELECT AVG(gpa) FROM students WHERE department_id;`
- [ ] D) `SELECT department_id, AVG(gpa) FROM students HAVING department_id;`

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **B) `SELECT department_id, AVG(gpa) FROM students GROUP BY department_id;`**
* **Why:** To fetch a metric calculated *per* entity, you must attach a `GROUP BY` clause targeting that entity identifier (`department_id`). This ensures the average values are computed accurately for each separate department.
</details>

---

### Question 8
What happens if you select a column that is not in `GROUP BY` and not inside an aggregate function?

- [ ] A) PostgreSQL automatically groups by that column.
- [x] B) PostgreSQL returns an error.
- [ ] C) PostgreSQL ignores that column.
- [ ] D) PostgreSQL duplicates the rows.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **B) PostgreSQL returns an error.**
* **Why:** PostgreSQL will throw a strict syntax execution error. If a column is unaggregated and missing from the `GROUP BY` clause, the relational database layout cannot determine which row value to show in the grouped summary row.
</details>

---

## ✍️ Short Answer

### Question 9
Explain in 2–3 sentences why you would use `HAVING` instead of `WHERE` when filtering grouped data.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Answer:** You must use `HAVING` instead of `WHERE` when filtering parameters rely entirely on computed aggregate functions like `COUNT(*)` or `AVG(gpa)`. `WHERE` clauses run early to filter individual source rows before any grouping happens, making them completely unaware of group totals. `HAVING` runs later in the execution timeline, allowing you to filter computed group summaries safely.
</details>

---

### Question 10
Describe a real-world university scenario where you would use `GROUP BY` with `COUNT()` and `HAVING` together.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Answer:** A university housing office can deploy this combination during campus facility planning to isolate high-density residential zones. They can group housing data by dormitory identifier (`GROUP BY dorm_id`), count total active occupants (`COUNT(*)`), and use `HAVING COUNT(*) > 100` to extract a list of heavily populated dorms that require additional maintenance resources.
</details>
