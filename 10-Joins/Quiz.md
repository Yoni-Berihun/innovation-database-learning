#  Quiz — SQL Joins

> **Instructions:** Answer the following questions to check your understanding. Answers are hidden under each question button.

---

## 🔘 Multiple Choice

### Question 1
What does an `INNER JOIN` return?

- [ ] A) All rows from both tables, filling in NULL where there is no match.
- [x] B) Only rows that have matching values in both tables.
- [ ] C) All rows from the left table and matching rows from the right table.
- [ ] D) All rows from the right table and matching rows from the left table.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **B) Only rows that have matching values in both tables.**
* **Why:** An `INNER JOIN` acts as a strict intersection. It evaluates the common key column and filters out any rows that do not have a corresponding match in both datasets.
</details>

---

### Question 2
You want to list all students and their enrolled courses. Students with no enrollments should still appear in the list. Which join should you use?

- [ ] A) INNER JOIN
- [x] B) LEFT JOIN
- [ ] C) RIGHT JOIN
- [ ] D) FULL JOIN

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **B) LEFT JOIN**
* **Why:** A `LEFT JOIN` preserves every record from the left (primary) table—in this case, `students`. If a student has no matching row in `enrollments`, they remain in the result with `NULL` placeholders.
</details>

---

### Question 3
Which of the following is TRUE about a `FULL JOIN`?

- [ ] A) It is the same as an INNER JOIN.
- [ ] B) It returns only unmatched rows from both tables.
- [x] C) It returns all rows from both tables, matching where possible and filling with NULL otherwise.
- [ ] D) It is not supported in PostgreSQL.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **C) It returns all rows from both tables, matching where possible and filling with NULL otherwise.**
* **Why:** A `FULL JOIN` combines the behaviors of both a `LEFT JOIN` and a `RIGHT JOIN`. It yields a complete set of records from both sides, matching pairs where a link exists and injecting `NULL`s where it fails.
</details>

---

### Question 4
In the query below, which table is the "left" table?

```sql
SELECT * FROM students s LEFT JOIN enrollments e ON s.student_id = e.student_id;  
```

- [ ] A) enrollments
- [x] B) students
- [ ] C) Neither, LEFT JOIN has no left table.
- [ ] D) Both are left tables.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **B) students**
* **Why:** The table that appears immediately after the `FROM` keyword and before the `JOIN` keyword is always designated as the "left" table in SQL syntax layout processing.
</details>

---

### Question 5
You run a `RIGHT JOIN` between instructors (left) and courses (right). What happens to a course that has no instructor assigned?

- [ ] A) It is excluded from the results.
- [x] B) It appears with NULL values for the instructor columns.
- [ ] C) The query returns an error.
- [ ] D) It appears twice.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **B) It appears with NULL values for the instructor columns.**
* **Why:** Because `courses` is on the right side of a `RIGHT JOIN`, all course records are guaranteed to be kept. Unassigned courses will display empty `NULL` values for any field originating from the `instructors` table.
</details>

---

## 🔄 True or False

### Question 6
A `LEFT JOIN` can be rewritten as a `RIGHT JOIN` by simply swapping the order of the tables in the query.

- [x] True
- [ ] False

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **True**
* **Why:** The relationship `Table_A LEFT JOIN Table_B` is logically identical to `Table_B RIGHT JOIN Table_A`. Swapping table positions while shifting the join direction achieves the same output dataset.
</details>

---

### Question 7
Using multiple `INNER JOIN`s in a single query will only return rows where all joined tables have matching data.

- [x] True
- [ ] False

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **True**
* **Why:** Each sequential `INNER JOIN` acts as an independent data filter. If a row fails to find a match at any point along the execution chain, it is completely dropped from the final output table.
</details>

---

### Question 8
`FULL JOIN` is commonly used in everyday application queries.

- [ ] True
- [x] False

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **False**
* **Why:** `FULL JOIN` operations are rarely deployed in standard application workflows. They are typically reserved for database administrators performing system audits, data-quality checks, or tracking down orphan records.
</details>

---

## ✍️ Short Answer

### Question 9
Explain in your own words (2–3 sentences) the difference between `INNER JOIN` and `LEFT JOIN`.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Sample Answer:** An `INNER JOIN` only outputs rows where matching keys exist in both tables, discarding unmatched data. In contrast, a `LEFT JOIN` retains all records from the first table regardless of a match, filling missing right-side variables with `NULL` markers.
</details>

---

### Question 10
Describe a real-world university scenario where you would use a `FULL JOIN` on the `instructors` and `departments` tables.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Sample Answer:** A school registrar can deploy a `FULL JOIN` during an academic department reorganization or database audit. This helps quickly extract a single list pinpointing both newly hired instructors who haven't been assigned to a department yet, AND newly created departments that have no staff attached to them.
</details>

---

## 🖥️ Code Reading

### Question 11
Look at the following query. What will be the output? Will Diana Prince appear?

```sql
SELECT
  s.first_name,
  s.last_name,
  g.numeric_grade
FROM students s
INNER JOIN enrollments e
  ON s.student_id = e.student_id
INNER JOIN grades g
  ON e.enrollment_id = g.enrollment_id;
```

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **Diana Prince will NOT appear.**
* **Why:** The transaction relies on sequential `INNER JOIN` clauses. Because Diana Prince has zero records logged inside the `enrollments` table, she is filtered out immediately during the evaluation of the very first join condition.
</details>

---

### Question 12
What is the purpose of table aliases (like `s`, `e`, `g`) in the query above?

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** Table aliases shorten reference declarations, improving query legibility and preventing **ambiguity compiler errors** when distinct tables contain columns with the exact same name (such as `student_id`).
</details>
