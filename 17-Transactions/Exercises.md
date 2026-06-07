# Module 17: Exercises — Transactions in PostgreSQL

> **Instructions:** Write the SQL or explanation for each exercise. Use the University Management System schema. Test your queries in a local PostgreSQL instance (psql, pgAdmin, or any online PostgreSQL environment).

---

## 🟢 Easy Exercises

### Exercise 1
**Task:** Write a transaction that inserts a new enrollment for `student_id = 1` in `course_id = 102` for semester 'Fall 2025', then commits the change.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
BEGIN;

INSERT INTO enrollments (student_id, course_id, semester)
VALUES (1, 102, 'Fall 2025');

COMMIT;
```
* **Explanation:** `BEGIN` launches the private transactional workspace. `COMMIT` ensures that the insertion is written permanently into physical storage logs.
</details>

---

### Exercise 2
**Task:** Write a transaction that attempts to insert a new enrollment, but then rolls it back. Verify that the enrollment was NOT saved by querying the `enrollments` table afterward.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
BEGIN;

INSERT INTO enrollments (student_id, course_id, semester)
VALUES (99, 999, 'Fall 2025');

ROLLBACK;

-- Verify: this should return no rows for the attempted insert
SELECT * FROM enrollments WHERE student_id = 99;
```
* **Explanation:** `ROLLBACK` wipes the uncommitted transactional log block, completely undoing the insertion as if it never occurred.
</details>

---

### Exercise 3
**Task:** Write a transaction that enrolls `student_id = 2` in `course_id = 101` and decreases the course's `seats_available` by 1. Assume `courses` has a `seats_available` column.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
BEGIN;

INSERT INTO enrollments (student_id, course_id, semester)
VALUES (2, 101, 'Fall 2025');

UPDATE courses
SET seats_available = seats_available - 1
WHERE course_id = 101;

COMMIT;
```
* **Explanation:** This wraps both the row insert and the numerical count modification within a single structural boundary block.
</details>

---

### Exercise 4
**Task:** Explain in one sentence why the enrollment and seat update in Exercise 3 must be in the same transaction.

<details>
<summary><b>🔍 Show Answer</b></summary>

* **Answer:** If the enrollment record is saved successfully but the seat count decrement fails due to an unexpected system crash, the database will contain corrupted class ledger data.
</details>

---

### Exercise 5
**Task:** Write a transaction that attempts to update a student's GPA to 5.0 (which violates a CHECK constraint), catches the error by rolling back, and leaves the database unchanged.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
BEGIN;

UPDATE students
SET gpa = 5.0
WHERE student_id = 1;

-- This will fail automatically due to CHECK constraint validation (gpa <= 4.0)
-- PostgreSQL marks the transactional loop as aborted.
-- You must manually or programmatically issue:

ROLLBACK;

-- Verify the GPA remained untouched:
SELECT gpa FROM students WHERE student_id = 1;
```
</details>

---

## 🟡 Medium Exercises

### Exercise 6
**Task:** Write a transaction that:
1. Inserts a grade for `enrollment_id = 500` (A, 95).
2. Creates a savepoint named `sp1`.
3. Attempts to insert a grade for `enrollment_id = 501` with an invalid letter grade 'Z'.
4. Rolls back to `sp1` when the insert fails.
5. Inserts the correct grade for `enrollment_id = 501` (B, 85).
6. Commits.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
BEGIN;

-- Step 1
INSERT INTO grades (enrollment_id, letter_grade, numeric_grade)
VALUES (500, 'A', 95);

-- Step 2
SAVEPOINT sp1;

-- Step 3 & 4 (Simulated execution exception rollback step)
-- In real database adapters, app catch exception blocks trigger:
ROLLBACK TO SAVEPOINT sp1;

-- Step 5
INSERT INTO grades (enrollment_id, letter_grade, numeric_grade)
VALUES (501, 'B', 85);

-- Step 6
COMMIT;
```
</details>

---

### Exercise 7
**Task:** Write a transaction that transfers `student_id = 3` from department 1 to department 2. Update the student's `department_id` and adjust both departments' student counts. Commit only if all steps succeed.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
BEGIN;

-- Update student's target tracking ID
UPDATE students
SET department_id = 2
WHERE student_id = 3;

-- Decrease old department inventory ledger
UPDATE departments
SET student_count = student_count - 1
WHERE department_id = 1;

-- Increase new department inventory ledger
UPDATE departments
SET student_count = student_count + 1
WHERE department_id = 2;

COMMIT;
```
</details>

---

### Exercise 8
**Task:** A professor is uploading 3 grades in one transaction. Use savepoints so that if one grade fails, only that grade is skipped and the others are still committed.
* Grade 1: enrollment_id=500, A, 95
* Grade 2: enrollment_id=501, Z, 88 (invalid - should be skipped)
* Grade 3: enrollment_id=502, B, 85

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
BEGIN;

-- Grade 1
INSERT INTO grades (enrollment_id, letter_grade, numeric_grade)
VALUES (500, 'A', 95);

SAVEPOINT after_grade_1;

-- Grade 2 (invalid execution block)
-- Application handler code registers the constraint exception and runs:
ROLLBACK TO SAVEPOINT after_grade_1;

-- Grade 3
INSERT INTO grades (enrollment_id, letter_grade, numeric_grade)
VALUES (502, 'B', 85);

COMMIT;
```
* **Note:** In pure SQL terminal consoles, a hard constraint violation aborts the entire transaction wrapper block. Savepoints operate most cleanly when managed within backend application codes (like Python or Node.js) where exception catch blocks can gracefully deploy targeted `ROLLBACK TO SAVEPOINT` directives.
</details>

---

### Exercise 9
**Task:** Write a transaction that handles a student dropping a course:
1. Delete the enrollment record.
2. Increase the course's `seats_available` by 1.
3. Decrease the student's `total_credits` by the course credits.
*Use a savepoint before the credit update in case the student has already been refunded.*

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
BEGIN;

-- Step 1: Remove enrollment record mapping
DELETE FROM enrollments
WHERE student_id = 1 AND course_id = 101;

-- Step 2: Increment inventory room
UPDATE courses
SET seats_available = seats_available + 1
WHERE course_id = 101;

-- Establish a clean checkpoint
SAVEPOINT before_credit_update;

-- Step 3: Decrease the student's credit balance
UPDATE students
SET total_credits = total_credits - 3
WHERE student_id = 1;

-- If application parameters dictate bypassing credit modification loops:
-- ROLLBACK TO SAVEPOINT before_credit_update;

COMMIT;
```
</details>

---

### Exercise 10
**Task:** Explain why you would wrap a grade finalization process (updating 30 student grades at once) in a single transaction rather than 30 separate transactions.

<details>
<summary><b>🔍 Show Answer</b></summary>

* **Answer:** Wrapping all 30 grade adjustments inside a single transaction boundary ensures atomic enforcement. If a data crash isolates the environment after 15 loops execute, processing individual commits would cause data corruption (half the class having updated profiles while the rest remain old). A single transaction block guarantees an absolute all-or-nothing execution footprint.
</details>

---

## 🔴 Challenging Exercises

### Exercise 11
**Task:** Open two separate psql terminal windows. In Session A, start a transaction, check `seats_available` for `course_id = 101`, and pause (do not commit). In Session B, start a transaction and try to check the same seat count. Describe what happens and why.

<details>
<summary><b>🔍 Show Answer</b></summary>

* **Expected Behavior:**
  * **Session A:**
    ```sql
    BEGIN;
    SELECT seats_available FROM courses WHERE course_id = 101;
    -- Returns: 5 (Stays open without committing)
    ```
  * **Session B:**
    ```sql
    BEGIN;
    SELECT seats_available FROM courses WHERE course_id = 101;
    -- Returns: 5
    ```
* **Why:** Under PostgreSQL's default **Read Committed** isolation layout, Session B reads the last known physically committed baseline value (`5`). Session A's active changes remain strictly isolated from external visibility until a hard `COMMIT` transaction is finalized.
</details>

---

### Exercise 12
**Task:** Write a transaction that enrolls a student only if a seat is available, checking `seats_available > 0` inside the `UPDATE` clause's condition to explicitly prevent course overbooking scenarios.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
BEGIN;

-- Atomically check and adjust inventory limits first
UPDATE courses
SET seats_available = seats_available - 1
WHERE course_id = 101 AND seats_available > 0;

-- Programmatic Application Layer Check:
-- If the system registers rows_affected = 0, it means the class is fully booked.
-- The application catch logic should fire a ROLLBACK statement.
-- Otherwise, if a seat is locked, continue the enrollment:

INSERT INTO enrollments (student_id, course_id, semester)
VALUES (1, 101, 'Fall 2025');

COMMIT;
```
</details>

---

### Exercise 13
**Task:** Imagine a banking component within the university schema that transfers a tuition fee of 500 Birr from a student's `student_accounts` balance to the institution's `university_ledger`. Design a safe transaction block featuring step checks and a fallback rollback mechanism if the student has insufficient funds.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
BEGIN;

