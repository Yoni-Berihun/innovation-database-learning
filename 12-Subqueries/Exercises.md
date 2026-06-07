# Module 12: Quiz — Subqueries in PostgreSQL

> **Instructions:** Answer the following questions to check your understanding. Answers and explanations are hidden under each question button.

---

## 🔘 Multiple Choice

### Question 1
What is a subquery in SQL?

- [ ] A) A query that runs faster than a normal query.
- [x] B) A query nested inside another SQL statement.
- [ ] C) A special column type used for storing calculations.
- [ ] D) A separate database template file.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **B) A query nested inside another SQL statement.**
* **Why:** A subquery (also known as an inner or nested query) is a standard SELECT statement embedded within an outer query's WHERE, FROM, HAVING, or SELECT clause to calculate data dynamically.
</details>

---

### Question 2
What is the golden rule when using a subquery with comparison operators like `=`, `>`, or `<` inside a `WHERE` clause?

- [ ] A) It must return at least three columns.
- [ ] B) It can return as many rows as possible.
- [x] C) It must return exactly one column and one row (a single scalar value).
- [ ] D) It cannot contain a WHERE clause of its own.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **C) It must return exactly one column and one row (a single scalar value).**
* **Why:** Basic comparison operators can only compare a column against a single value. If your subquery returns multiple rows or columns, PostgreSQL will throw a runtime cardinality error.
</details>

---

### Question 3
If a subquery returns multiple rows (a list of values), which operator should you use in the outer query's `WHERE` clause instead of `=`?

- [ ] A) LIKE
- [x] B) IN
- [ ] C) BETWEEN
- [ ] D) AND

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **B) IN**
* **Why:** The `IN` operator checks if a value matches any element in a list returned by the subquery, making it perfect for handling multi-row subquery outputs.
</details>

---

### Question 4
When you place a subquery inside the `FROM` clause to act as a temporary calculated table, what is strictly required?

- [ ] A) It must not use GROUP BY.
- [x] B) You must give the subquery an explicit table alias.
- [ ] C) It must return only one row.
- [ ] D) It can only query the departments table.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **B) You must give the subquery an explicit table alias.**
* **Why:** PostgreSQL treats a subquery in the `FROM` clause as a derived table. The outer query needs a name (alias) to reference the columns inside that subquery block.
</details>

---

### Question 5
What is a **Correlated Subquery**?

- [ ] A) A subquery that returns a corrupted dataset.
- [x] B) A subquery that references and depends on columns from the outer query.
- [ ] C) A subquery that runs completely independent of the outer query.
- [ ] D) A query that contains more than five levels of nesting.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **B) A subquery that references and depends on columns from the outer query.**
* **Why:** In a correlated subquery, the inner query uses values from the outer query, meaning it executes repeatedly—once for each row processed by the outer query.
</details>

---

## 🔄 True or False

### Question 6
If a subquery evaluated by a `NOT IN` operator returns even a single `NULL` value, the entire query may return an empty result set unexpectedly.

- [x] True
- [ ] False

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **True**
* **Why:** In SQL logic, comparing any value to `NULL` using `NOT IN` results in `UNKNOWN`. This invalidates the entire condition, which is why using `NOT EXISTS` or filtering out `NULL`s is a recommended best practice.
</details>

---

### Question 7
Subqueries can often be rewritten as `JOIN`s, and `JOIN`s are generally more efficient for combining multiple columns from different tables.

- [x] True
- [ ] False

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **True**
* **Why:** While subqueries are excellent for single calculated or aggregated values, `JOIN` operations allow database engines to optimize path lookups more efficiently when merging multiple columns.
</details>

---

## ✍️ Short Answer

### Question 8
Explain what happens behind the scenes when PostgreSQL executes this query:
```sql
SELECT title FROM courses 
WHERE instructor_id = (SELECT instructor_id FROM instructors WHERE last_name = 'Turing');
```

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Answer:** First, PostgreSQL runs the inner subquery to look up the unique `instructor_id` belonging to the instructor with the last name `'Turing'`. Once it gets that specific single ID number (e.g., `10`), it substitutes it into the outer query, executing `SELECT title FROM courses WHERE instructor_id = 10;` to return the matching course titles.
</details>

---

### Question 9
What error will occur if you run the following query, and how would you fix it?
```sql
SELECT first_name FROM students 
WHERE department_id = (SELECT department_id FROM departments WHERE name LIKE '%Science%');
```

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Answer:** Since the subquery looks for departments using `LIKE '%Science%'`, it will likely return multiple rows (e.g., Computer Science, Data Science). Because it uses the `=` operator, PostgreSQL will throw a **Subquery returned more than one row** error. 
* **Fix:** Change the `=` operator to `IN`:
  `SELECT first_name FROM students WHERE department_id IN (SELECT department_id FROM departments WHERE name LIKE '%Science%');`
</details>

---

## 🖥️ Code Reading

### Question 10
Look at the following query. Describe in plain words what specific data this report is trying to pull from the database.

```sql
SELECT first_name, last_name 
FROM students 
WHERE student_id NOT IN (
    SELECT student_id FROM enrollments
);
```

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Answer:** This query **finds all students who are NOT currently enrolled in any courses**. The inner query compiles a complete list of all student IDs present in the `enrollments` table, and the outer query filters the main directory to return only those students whose IDs are entirely absent from that registration log list.
</details>
