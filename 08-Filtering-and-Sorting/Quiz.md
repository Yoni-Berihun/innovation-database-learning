# ✅ Quiz — SQL Joins in PostgreSQL

Test your understanding before moving to the next module. There are 10 questions. Take your time.

---

## Question 1 (Multiple Choice)

What does a SQL JOIN do?

- [ ] Deletes data from multiple tables
- [ ] Combines rows from two or more tables based on related columns
- [ ] Creates a new table in the database
- [ ] Sorts data across multiple tables

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **Combines rows from two or more tables based on related columns**
* **Why:** A `JOIN` clause is used to combine rows from two or more tables, based on a related column between them (like foreign keys). It does not create or delete tables.
</details>

---

## Question 2 (Multiple Choice)

What is the difference between INNER JOIN and LEFT JOIN?

- [ ] There is no difference
- [ ] INNER JOIN returns only matching rows; LEFT JOIN returns all rows from the left table with NULL for unmatched right rows
- [ ] LEFT JOIN is faster than INNER JOIN
- [ ] INNER JOIN can only join two tables, LEFT JOIN can join more

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **INNER JOIN returns only matching rows; LEFT JOIN returns all rows from the left table with NULL for unmatched right rows**
* **Why:** `INNER JOIN` creates a strict match where data must exist in both tables. `LEFT JOIN` preserves all records from the first (left) table, even if no matching record is found in the second (right) table.
</details>

---

## Question 3 (Multiple Choice)

In this query, what does `s` represent?

```sql
SELECT s.first_name 
FROM students s 
JOIN enrollments e ON s.student_id = e.student_id;
```

- [ ] A column name
- [ ] A table alias (shortcut name)
- [ ] A built-in SQL function
- [ ] A database schema

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **A table alias (shortcut name)**
* **Why:** `s` is a table alias for `students`. It acts as a temporary shortcut name to make the query shorter and easier to read, especially when referencing columns from multiple tables.
</details>

---

## Question 4 (Multiple Choice)

What will happen if you run a query with a `JOIN` but forget the `ON` clause?

- [ ] PostgreSQL automatically guesses the matching columns
- [ ] It returns a Syntax Error or creates a Cross Join (every row matched with every row)
- [ ] It deletes all records from both tables
- [ ] The query runs normally and returns only matching records

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **It returns a Syntax Error or creates a Cross Join (every row matched with every row)**
* **Why:** In standard ANSI SQL syntax, omitting the `ON` clause results in a syntax error. If comma-based join syntax is used without a condition, it produces a Cartesian Product (Cross Join), which is highly inefficient.
</details>

---

## Question 5 (Multiple Choice)

Which JOIN type should you use if you want to find all students, including those who are NOT currently enrolled in any courses?

- [ ] INNER JOIN
- [ ] LEFT JOIN (with students as the left table)
- [ ] RIGHT JOIN (with enrollments as the left table)
- [ ] CROSS JOIN

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **LEFT JOIN (with students as the left table)**
* **Why:** A `LEFT JOIN` ensures all records from the left table (`students`) are kept in the results. If a student has no corresponding rows in the `enrollments` table, the enrollment columns will simply display as `NULL`.
</details>

---

## Question 6 (Multiple Choice)

Look at the following condition: `ON students.student_id = enrollments.student_id`. What are these columns typically called?

- [ ] Primary Key and Foreign Key
- [ ] Default Key and Static Key
- [ ] Input and Output columns
- [ ] Index and Constraint columns

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **Primary Key and Foreign Key**
* **Why:** Joins typically link the `Primary Key` of one table (e.g., `student_id` in `students`) to the `Foreign Key` of another table (e.g., `student_id` in `enrollments`) to establish relational integrity.
</details>

---

## Question 7 (Short Answer)

Write a complete SQL query using `INNER JOIN` to retrieve the `first_name` from the `students` table and the `course_id` from the `enrollments` table. Match them using `student_id`.

```sql
___________________
```

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

```sql
SELECT students.first_name, enrollments.course_id
FROM students
INNER JOIN enrollments ON students.student_id = enrollments.student_id;
```
* **Why:** This query explicitly requests columns from both tables, targets the base table (`students`), joins the related table (`enrollments`), and uses the `ON` keyword to link their matching ID fields.
</details>

---

## Question 8 (Short Answer)

What specific value will appear in the columns of the right table during a `LEFT JOIN` if no matching row is found?

```text
___________________
```

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** `NULL`
* **Why:** In SQL, `NULL` represents missing or unknown data. When a row on the left side of a `LEFT JOIN` has no match on the right side, the database fills all right-side columns with `NULL`.
</details>

---

## Question 9 (Multiple Choice)

Can you join more than two tables together in a single SQL query?

- [ ] No, SQL only allows joining two tables at a time
- [ ] Yes, by adding consecutive JOIN and ON clauses
- [ ] Yes, but only if all tables have the exact same number of rows
- [ ] Yes, but it requires a special multi-table plug-in

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **Yes, by adding consecutive JOIN and ON clauses**
* **Why:** You can link as many tables as needed in a single query by chaining `JOIN table_name ON condition` commands. The database processes them step-by-step from top to bottom.
</details>

---

## Question 10 (Short Answer)

Look at this query:
```sql
SELECT courses.title, departments.name
FROM courses
RIGHT JOIN departments ON courses.department_id = departments.department_id;
```
If there is a department called "Art" that currently has zero courses assigned to it, will "Art" appear in the query results? Explain why.

```text
___________________
```

- [ ] A column in the students table
- [x] A table alias for students
- [ ] A database name
- [ ] A function

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **A table alias for students**
* **Why:** `s` is a table alias. It acts as a temporary shortcut name for the `students` table to keep the query short and clean when referencing its columns.
</details>

---

- [ ] A column in the students table
- [x] A table alias for students
- [ ] A database name
- [ ] A function

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **A table alias for students**
* **Why:** `s` is a table alias. It acts as a temporary shortcut name for the `students` table to keep the query short, clean, and efficient when referencing its columns.
</details>

---

## Question 11 (Multiple Choice)

What happens if you omit the ON clause in a JOIN?

- [ ] The query returns an error
- [ ] The query returns a Cartesian product (every row joined to every row)
- [ ] The query automatically finds matching columns
- [ ] The query returns only the first table

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **The query returns an error**
* **Why:** In standard ANSI SQL (using the explicit `JOIN` keyword), omitting the `ON` clause results in a **Syntax Error** in PostgreSQL. If you use the older comma-separated implicit join syntax without a `WHERE` clause, it results in a Cartesian product (Cross Join).
</details>

---

## Question 12 (Multiple Choice)

When would you use LEFT JOIN instead of INNER JOIN?

- [ ] When you want to hide rows with no matches
- [ ] When you want all rows from the first table, even without matches in the second
- [ ] When you want faster query performance
- [ ] When you only need columns from the second table

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **When you want all rows from the first table, even without matches in the second**
* **Why:** A `LEFT JOIN` preserves all rows from the left (first) table. If there is no matching row in the right (second) table, the database still keeps the left row and fills the right table's columns with `NULL`.
</details>

---

## Question 13 (Short Answer)

Write a query to join students and enrollments using INNER JOIN. Show student first_name, last_name, and course_id.

```sql
SELECT s.first_name, s.last_name, e.course_id
FROM students s
INNER JOIN enrollments e ON s.student_id = e.student_id;
```

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

```sql
SELECT s.first_name, s.last_name, e.course_id
FROM students s
INNER JOIN enrollments e ON s.student_id = e.student_id;
```
* **Explanation:** This query selects columns from both tables, uses aliases (`s` and `e`), links the tables via `INNER JOIN`, and uses the common column `student_id` to match the records accurately.
</details>

---

## Question 14 (Short Answer)

Explain why you would use table aliases in a JOIN query. Give a brief example.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Answer:** Table aliases make queries shorter, cleaner, and easier to read. More importantly, they prevent **ambiguity errors** when two or more tables share identical column names (like `student_id`).
* **Example:**
  Instead of writing:
  `SELECT students.first_name FROM students JOIN enrollments ON students.student_id = enrollments.student_id;`
  You can write cleanly with aliases:
  `SELECT s.first_name FROM students s JOIN enrollments e ON s.student_id = e.student_id;`
</details>

---

## Question 15 (Short Answer)

You need to find all courses and their instructors, including courses that have no instructor assigned yet. Should you use INNER JOIN or LEFT JOIN? Why?

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Answer:** You should use a **LEFT JOIN** (assuming `courses` is the left/first table).
* **Why:** An `INNER JOIN` would completely hide and remove any course that does not have an instructor assigned. A `LEFT JOIN` ensures **all courses** remain in the results, leaving the instructor columns blank (`NULL`) for courses without a teacher.
</details>

---

## Question 16 (Short Answer)

Write a query that joins students, enrollments, and courses to show:
* student first_name and last_name
* course title
* enrollment status

Use table aliases.

```sql
SELECT s.first_name, s.last_name, c.title, e.status
FROM students s
JOIN enrollments e ON s.student_id = e.student_id
JOIN courses c ON e.course_id = c.course_id;
```

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

```sql
SELECT s.first_name, s.last_name, c.title, e.status
FROM students s
JOIN enrollments e ON s.student_id = e.student_id
JOIN courses c ON e.course_id = c.course_id;
```
* **Explanation:** This is a 3-table join. We start from `students` (`s`), join the bridging table `enrollments` (`e`) via `student_id`, and then join `courses` (`c`) via `course_id` to fetch the course title.
</details>

---

## Question 17 (Short Answer)

Describe what this query does, line by line:

```sql
SELECT s.first_name, s.last_name, c.title
FROM students s
LEFT JOIN enrollments e ON s.student_id = e.student_id
LEFT JOIN courses c ON e.course_id = c.course_id
WHERE e.enrollment_id IS NULL;
```

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Line-by-line Breakdown:**
  * `SELECT s.first_name, s.last_name, c.title`: Instructs the database to retrieve the student's first name, last name, and the course title.
  * `FROM students s`: Sets `students` as the main (left) table and gives it an alias `s`.
  * `LEFT JOIN enrollments e ON s.student_id = e.student_id`: Joins `enrollments` keeping all students, matching them on their ID. Students with no enrollments will have `NULL` values in enrollment fields.
  * `LEFT JOIN courses c ON e.course_id = c.course_id`: Joins `courses` based on `course_id` to get the titles.
  * `WHERE e.enrollment_id IS NULL;`: This filters the output to only display rows where the enrollment record does not exist.
* **Overall Result:** This query **finds all students who are NOT enrolled in any courses**. It isolates rows where the `LEFT JOIN` failed to find a match.
</details>
