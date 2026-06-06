
# ✅ Quiz — CRUD Operations in PostgreSQL

Test your understanding before moving to the next module. There are 10 questions. Take your time.

---

## Question 1 (Multiple Choice)

What does CRUD stand for?

- [ ] Create, Read, Upload, Download
- [ ] Create, Read, Update, Delete
- [ ] Copy, Run, Update, Deploy
- [ ] Connect, Retrieve, Use, Destroy

---

## Question 2 (Multiple Choice)

Which SQL command is used to add new records to a table?

- [ ] ADD
- [ ] INSERT
- [ ] CREATE
- [ ] APPEND

---

## Question 3 (Multiple Choice)

What does `SELECT * FROM students;` do?

- [ ] Deletes all students
- [ ] Updates all students
- [ ] Retrieves all columns and all rows from the students table
- [ ] Inserts a new student

---

## Question 4 (Multiple Choice)

What happens if you run `UPDATE students SET gpa = 4.0;` without a WHERE clause?

- [ ] Only the first student's GPA changes
- [ ] Nothing happens
- [ ] Every student's GPA becomes 4.0
- [ ] An error occurs

---

## Question 5 (Multiple Choice)

Which command permanently removes an entire table including its structure?

- [ ] DELETE FROM students;
- [ ] DROP TABLE students;
- [ ] REMOVE TABLE students;
- [ ] CLEAR TABLE students;

---

## Question 6 (Multiple Choice)

What is the safest practice before running a DELETE statement?

- [ ] Run SELECT * first to see all data
- [ ] Run a SELECT query with the same WHERE clause to verify what will be deleted
- [ ] Delete immediately and hope for the best
- [ ] Ask a friend to review your query

---

## Question 7 (Short Answer)

Explain the difference between DELETE and DROP. When would you use each?

_______________________________________________
_______________________________________________
_______________________________________________

---

## Question 8 (Short Answer)

Why must text values be enclosed in single quotes in SQL INSERT statements? What happens if you forget them?

_______________________________________________
_______________________________________________
_______________________________________________

---

## Question 9 (Short Answer)

Look at this SQL statement and explain what each part does:

```sql
UPDATE courses
SET credits = 4
WHERE department_id = 10;

### Question 10 (Short Answer)
A student named Hana (`student_id = 15`) has changed her email to `'hana.new@university.edu'` and her GPA has improved to `3.95`. Write the complete SQL UPDATE statement. Then explain why including `WHERE` is essential.

```sql
UPDATE students
SET email = 'hana.new@university.edu',
    gpa = 3.95
WHERE student_id = 15;
```

#### Explanation:
> 🚨 **ANSWER:** The `WHERE` clause ensures that the modifications are applied **ONLY** to Hana's specific record (`student_id = 15`). If the `WHERE` clause is omitted, the email address of **EVERY single student** in the database will be overwritten to `'hana.new@university.edu'` and everyone's GPA will be changed to `3.95`. This would cause catastrophic data corruption and loss across the entire system.

---

## ✅ Answer Key

<details>
<summary>Click to reveal answers and explanations</summary>

### Question 1
* **Correct answer:** Create, Read, Update, Delete
* **Why:** CRUD is the acronym for the four fundamental database operations. Every application uses these operations to manage data.

### Question 2
* **Correct answer:** INSERT
* **Why:** `INSERT` is the SQL command for adding new records. `ADD` is used for columns, `CREATE` makes tables, and `APPEND` is not standard SQL.

### Question 3
* **Correct answer:** Retrieves all columns and all rows from the students table
* **Why:** `SELECT` retrieves data. The `*` means all columns. `FROM students` specifies the table. This returns every column and every row.

### Question 4
* **Correct answer:** Every student's GPA becomes 4.0
* **Why:** Without `WHERE`, `UPDATE` applies to **ALL** rows. This is one of the most dangerous mistakes in SQL. Always use `WHERE` to limit updates.

### Question 5
* **Correct answer:** DROP TABLE students;
* **Why:** `DROP TABLE` removes the entire table structure and all its data permanently. `DELETE` removes only data (rows). `REMOVE` and `CLEAR` are not standard SQL commands.

### Question 6
* **Correct answer:** Run a SELECT query with the same WHERE clause to verify what will be deleted
* **Why:** This is the safest practice. It shows you exactly which records will be affected before you commit to the deletion. Running `SELECT *` shows all data, not just what you'll delete.

### Question 7
* **Sample good answer:**
  `DELETE` removes rows (data) from a table but keeps the table structure intact. Use `DELETE` when you want to remove specific records, like deleting a withdrawn student.
  `DROP` removes the entire table — both the data and the table structure. Use `DROP` only when you want to completely eliminate a table from the database, which is rare in production.
  *(Any clear distinction between removing data vs. removing structure receives full credit.)*

### Question 8
* **Sample good answer:**
  Text values must be in single quotes so PostgreSQL knows they are text data, not column names or keywords. Without quotes, PostgreSQL interprets words as identifiers (like column names), which causes an error.
  For example, `VALUES (Abel)` tries to use Abel as a column name. `VALUES ('Abel')` correctly treats it as text.
  *(Any answer mentioning text identification and error prevention receives full credit.)*

### Question 9
* **Sample good answer:**
  * `UPDATE courses` — tells PostgreSQL we're modifying the courses table
  * `SET credits = 4` — changes the credits column to 4
  * `WHERE department_id = 10` — only affects courses where the department_id is 10 (Computer Science)
  The result: All Computer Science courses now have 4 credits.
  *(Any line-by-line explanation that accurately describes the statement receives full credit.)*

### Question 10
* **Correct SQL:**
  ```sql
  UPDATE students
  SET email = 'hana.new@university.edu',
      gpa = 3.95
  WHERE student_id = 15;
  ```
* **Explanation:** The `WHERE` clause ensures only Hana's record (`student_id = 15`) is updated. Without `WHERE`, every student's email would become `'hana.new@university.edu'` and every GPA would become `3.95`, which would be a disaster.
  *(Correct SQL syntax and explanation of WHERE importance both required for full credit.)*

