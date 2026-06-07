# Module 13: Quiz — Constraints in PostgreSQL

> **Instructions:** Answer the following questions to check your understanding. Answers and explanations are hidden under each question button.

---

## 🔘 Multiple Choice

### Question 1
What does a `NOT NULL` constraint do?

- [ ] A) Ensures all values in the column are unique.
- [x] B) Prevents the column from containing empty (NULL) values.
- [ ] C) Automatically generates a unique ID for each row.
- [ ] D) Links the column to another table.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **B) Prevents the column from containing empty (NULL) values.**
* **Why:** The `NOT NULL` constraint enforces a rule that a specific column must always contain a valid data value. PostgreSQL will reject any `INSERT` or `UPDATE` statement that attempts to leave this field empty.
</details>

---

### Question 2
Which constraint automatically combines `NOT NULL` and `UNIQUE`?

- [ ] A) FOREIGN KEY
- [ ] B) CHECK
- [x] C) PRIMARY KEY
- [ ] D) DEFAULT

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **C) PRIMARY KEY**
* **Why:** A `PRIMARY KEY` uniquely identifies each row in a table. By definition, it implicitly applies both `NOT NULL` and `UNIQUE` rules to the designated column(s) automatically.
</details>

---

### Question 3
A `FOREIGN KEY` constraint is used to:

- [ ] A) Prevent duplicate values in a column.
- [x] B) Ensure a value in one table exists in another table.
- [ ] C) Set an automatic default value.
- [ ] D) Validate that a number is within a specific range.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **B) Ensure a value in one table exists in another table.**
* **Why:** `FOREIGN KEY` constraints maintain referential integrity. They cross-reference a column to a primary key in another table, preventing orphan records (like enrolling a student ID that does not exist).
</details>

---

### Question 4
What happens if you try to delete a row from `students` that is referenced by a `FOREIGN KEY` in `enrollments`, and the foreign key has `ON DELETE NO ACTION`?

- [ ] A) The deletion succeeds and the enrollment is deleted too.
- [ ] B) The deletion succeeds and the enrollment's student_id becomes NULL.
- [x] C) PostgreSQL rejects the deletion.
- [ ] D) The deletion succeeds but a warning is shown.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **C) PostgreSQL rejects the deletion.**
* **Why:** `NO ACTION` is the default behavior. If a dependent table (like `enrollments`) still references the parent row (the student), the database blocks the deletion to protect structural data integrity.
</details>

---

### Question 5
Which constraint would you use to ensure that a `gpa` column only accepts values between 0.0 and 4.0?

- [ ] A) NOT NULL
- [ ] B) UNIQUE
- [x] C) CHECK
- [ ] D) DEFAULT

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **C) CHECK**
* **Why:** A `CHECK` constraint allows you to specify custom boolean conditions or evaluation ranges that every column value must satisfy before being accepted into the database table.
</details>

---

### Question 6
What does `DEFAULT CURRENT_DATE` do?

- [ ] A) It makes the column automatically update to today's date every time the row is modified.
- [x] B) It sets the column to today's date if no value is provided during insertion.
- [ ] C) It prevents the column from being empty.
- [ ] D) It ensures the date is always in the future.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **B) It sets the column to today's date if no value is provided during insertion.**
* **Why:** The `DEFAULT` clause handles missing inputs during data entry. If an `INSERT` statement skips the date column, PostgreSQL dynamically evaluates `CURRENT_DATE` and saves today's system date automatically.
</details>

---

### Question 7
Can a table have more than one `PRIMARY KEY`?

- [ ] A) Yes, a table can have multiple primary keys.
- [x] B) No, a table can only have one primary key, but it can span multiple columns.
- [ ] C) Yes, but only if they are on different columns.
- [ ] D) No, and a primary key can only be on a single column.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **B) No, a table can only have one primary key, but it can span multiple columns.**
* **Why:** A table is limited to exactly one primary key configuration block. However, that primary key can combine multiple columns together—this setup is known as a **Composite Primary Key**.
</details>

---

### Question 8
What is the purpose of naming a constraint (e.g., `CONSTRAINT valid_grade CHECK ...`)?

- [ ] A) It makes the constraint run faster.
- [x] B) It allows you to reference the constraint later to modify or drop it.
- [ ] C) It is required for the constraint to work.
- [ ] D) It automatically creates an index on that column.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **B) It allows you to reference the constraint later to modify or drop it.**
* **Why:** Explicitly naming constraints keeps your database manageable. If you ever need to alter rules using `ALTER TABLE ... DROP CONSTRAINT name`, having a clear developer-assigned name is much easier than guessing a system-generated one.
</details>

---

## ✍️ Short Answer

### Question 9
Explain the difference between `ON DELETE CASCADE` and `ON DELETE SET NULL` in 2–3 sentences.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Sample Answer:** `ON DELETE CASCADE` automatically deletes all dependent rows in the child table when the referenced parent row is removed. On the other hand, `ON DELETE SET NULL` preserves the child rows but wipes the foreign key reference column, converting it into an empty `NULL` state instead.
</details>

---

### Question 10
Describe a real-world university scenario where a `CHECK` constraint would prevent a serious data error.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Sample Answer:** Placing a `CHECK (numeric_grade >= 0 AND numeric_grade <= 100)` constraint on a class ledger protects database integrity against typos. If a registrar accidentally enters `850` instead of `85`, the database blocks the entry, preventing corrupted system calculations, flawed GPA records, and invalid honors listings.
</details>
