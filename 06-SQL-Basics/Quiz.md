
# ✅ Quiz — SQL Basics

Test your understanding before moving to the next module. There are 10 questions. Take your time.

---

## Question 1 (Multiple Choice)

What does SQL stand for?

- [ ] Simple Query Language
- [ ] Structured Query Language
- [ ] System Query Language
- [ ] Standard Question Language

---

## Question 2 (Multiple Choice)

Which SQL statement is used to retrieve data from a database?

- [ ] GET
- [ ] FETCH
- [ ] SELECT
- [ ] RETRIEVE

---

## Question 3 (Multiple Choice)

What does `SELECT * FROM students;` do?

- [ ] Deletes all students
- [ ] Updates all students
- [ ] Retrieves all columns and all rows from the students table
- [ ] Inserts a new student

---

## Question 4 (Multiple Choice)

Which SQL statement adds new data to a table?

- [ ] ADD
- [ ] INSERT
- [ ] CREATE
- [ ] APPEND

---

## Question 5 (Multiple Choice)

What happens if you run `UPDATE students SET gpa = 4.0;` without a WHERE clause?

- [ ] Only the first student's GPA changes
- [ ] Nothing happens
- [ ] Every student's GPA becomes 4.0
- [ ] An error occurs

---

## Question 6 (Multiple Choice)

In an INSERT statement, why must text values be in single quotes?

- [ ] To make them look nice
- [ ] So PostgreSQL knows they are text, not column names or numbers
- [ ] To encrypt the data
- [ ] Single quotes are optional

---

## Question 7 (Short Answer)

Explain the purpose of the WHERE clause in SQL. Give one example using the students table.

_______________________________________________
_______________________________________________
_______________________________________________

---

## Question 8 (Short Answer)

Why is it dangerous to run DELETE without a WHERE clause? What should you do before running a DELETE statement?

_______________________________________________
_______________________________________________
_______________________________________________

---

## Question 9 (Short Answer)

Look at this SQL statement and explain what each part does:
### Question 10 (Short Answer)
A student named Mulugeta has changed his email. His `student_id` is 5. Write the complete SQL statement to update his email to `'mulugeta.new@university.edu'`. Then explain why including `WHERE` is essential.

```sql
UPDATE students
SET email = 'mulugeta.new@university.edu'
WHERE student_id = 5;
```

#### Explanation:
> 🚨 **ANSWER:** The `WHERE` clause ensures that the change is applied **ONLY** to the specific row belonging to Mulugeta (`student_id = 5`). If the `WHERE` clause is omitted, the email address of **EVERY single student** in the entire database will be overwritten and changed to `'mulugeta.new@university.edu'`, leading to critical data loss and corruption.

---

## ✅ Answer Key

<details>
<summary>Click to reveal answers and explanations</summary>

### Question 1
* **Correct answer:** Structured Query Language
* **Why:** SQL stands for Structured Query Language. It's the standardized language for managing and querying data in relational databases.

### Question 2
* **Correct answer:** SELECT
* **Why:** `SELECT` is the SQL statement used to retrieve data. `GET`, `FETCH`, and `RETRIEVE` are not standard SQL commands for data retrieval.

### Question 3
* **Correct answer:** Retrieves all columns and all rows from the students table
* **Why:** `SELECT *` means "select all columns." `FROM students` specifies the table. This returns every column and every row.

### Question 4
* **Correct answer:** INSERT
* **Why:** `INSERT` adds new records to a table. `ADD` is used for columns, `CREATE` makes tables, and `APPEND` is not a standard SQL data command.

### Question 5
* **Correct answer:** Every student's GPA becomes 4.0
* **Why:** Without `WHERE`, `UPDATE` applies to **ALL** rows. This is one of the most dangerous mistakes in SQL. Always use `WHERE` to limit updates to specific records.

### Question 6
* **Correct answer:** So PostgreSQL knows they are text, not column names or numbers
* **Why:** Single quotes tell PostgreSQL "this is text data." Without quotes, PostgreSQL interprets words as column names or keywords. Numbers don't need quotes.

### Question 7
* **Sample good answer:**
  The `WHERE` clause filters results so only rows matching a condition are affected. It limits which records are retrieved, updated, or deleted.
  *Example:* `SELECT * FROM students WHERE gpa > 3.5;` retrieves only students with a GPA greater than 3.5.
  *(Any clear explanation with a valid example receives full credit.)*

### Question 8
* **Sample good answer:**
  Running `DELETE` without `WHERE` removes every record in the table. There is no undo button in SQL, so this data is permanently lost.
  Before running `DELETE`, you should:
  1. Run a `SELECT` query with the same `WHERE` clause to confirm which records will be deleted.
  2. Make sure you have a backup if working with important data.
  3. Double-check your `WHERE` condition.
  *(Any answer mentioning permanent data loss and the SELECT verification step receives full credit.)*

### Question 9
* **Sample good answer:**
  * `INSERT INTO departments` — tells PostgreSQL we're adding data to the departments table
  * `(department_id, name, building, budget)` — lists the columns we're providing values for
  * `VALUES` — introduces the actual data
  * `(90, 'Economics', 'Business Hall', 450000)` — the actual values, in the same order as the columns
  The result is a new department record with these values.
  *(Any line-by-line explanation that accurately describes the statement receives full credit.)*

### Question 10
* **Correct SQL:**
  ```sql
  UPDATE students
  SET email = 'mulugeta.new@university.edu'
  WHERE student_id = 5;
  ```
* **Explanation:** The `WHERE` clause ensures only Mulugeta's record (`student_id = 5`) is updated. Without `WHERE`, every student's email would change to `'mulugeta.new@university.edu'`, which would be a disaster.
  *(Correct SQL syntax and explanation of WHERE importance both required for full credit.)*

</details>

---

## 📊 Self-Assessment


💬 **Remember:** `SELECT`, `INSERT`, `UPDATE`, and `DELETE` are the four pillars of database interaction. Every application you use — from Instagram to your banking app — runs these commands thousands of times per second. You now speak the language of data. That's a genuine superpower. Keep building on it! 🚀

```sql
INSERT INTO departments (department_id, name, building, budget)
VALUES (90, 'Economics', 'Business Hall', 450000);
