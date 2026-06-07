# 🔄 Transactions in PostgreSQL

&gt; **Module 17** | **Skill Level:** Beginner | **Prerequisite:** Modules 01–16  
&gt; **Estimated Time:** 3–4 hours

---

## 🎯 Learning Objectives

By the end of this module, you will be able to:

1. Explain what a transaction is and why it matters.
2. Describe the four ACID properties in simple terms.
3. Use `BEGIN`, `COMMIT`, and `ROLLBACK` to control transactions.
4. Use `SAVEPOINT` to create rollback points within a transaction.
5. Understand basic concurrency concepts and why isolation matters.
6. Name the common isolation levels and know when to use them.
7. Identify common transaction pitfalls like long-running transactions and deadlocks.
8. Apply transactions to real-world university management scenarios.

---

## 🧠 Why Transactions Matter

Imagine you are enrolling in a course at your university. The system must do three things:
1. Check if a seat is available.
2. Insert your enrollment record.
3. Decrease the available seats by one.

What if step 2 succeeds but step 3 fails because of a server crash? Now you are enrolled, but the course still shows an available seat. Another student might enroll, and the course ends up overbooked.

**A transaction prevents this.** It wraps all three steps into one unit: **either all succeed, or none do.**

&gt; 💡 **Internship Tip:** Every real-world application that handles money, inventory, or critical data uses transactions. Interviewers expect you to understand ACID and how to use `BEGIN/COMMIT/ROLLBACK`.

---

## 📖 What Is a Transaction?

A **transaction** is a group of one or more SQL operations that are treated as a single unit of work.

### The All-or-Nothing Rule
- **All operations succeed** → `COMMIT` (save everything permanently).
- **Any operation fails** → `ROLLBACK` (undo everything, as if nothing happened).

### Beginner-Friendly Analogy: Cooking a Meal

You are cooking dinner for friends. The meal has three parts: rice, chicken, and salad.

- You start cooking the rice.
- You start cooking the chicken.
- Oh no! You burned the rice.

**Without transactions:** You serve the burned rice with good chicken and salad. Your friends are unhappy.

**With transactions:** You throw everything away, order pizza, and start fresh. The entire meal is either perfect or cancelled. No partial meals allowed.

A database transaction works the same way. All operations inside the transaction must succeed, or the whole thing is cancelled.

---

## 📖 The ACID Properties

ACID is an acronym for the four guarantees that transactions provide.

| Property | What It Means | University Example |
|----------|---------------|-------------------|
| **Atomicity** | All operations complete, or none do. | Student enrollment: check seat, insert record, update seat count. All three happen, or none happen. |
| **Consistency** | The database moves from one valid state to another. | A student's GPA cannot be negative. A transaction that tries to set GPA to -1.0 is rejected. |
| **Isolation** | Concurrent transactions do not interfere with each other. | Two students enrolling in the last seat at the same time. Only one succeeds. |
| **Durability** | Once committed, changes survive power failures. | After grades are finalized, they stay saved even if the server restarts. |

### Simple Memory Trick
&gt; **A**ll **C**hanges **I**n **D**atabases are safe.

---

## 📖 Using Transactions in PostgreSQL

### Basic Syntax

```sql
BEGIN;                          -- Start the transaction

-- Your SQL operations here
INSERT INTO enrollments (student_id, course_id, semester)
VALUES (1, 101, 'Fall 2025');

UPDATE courses
SET seats_available = seats_available - 1
WHERE course_id = 101;

COMMIT;                         -- Save everything permanently

### If Something Goes Wrong: ROLLBACK
```sql
BEGIN;

INSERT INTO enrollments (student_id, course_id, semester)
VALUES (1, 101, 'Fall 2025');

-- Oh no! We realize the student is not eligible.
ROLLBACK;                       -- Undo everything. No enrollment was saved.
```

### Text Diagram: Transaction Flow
```text
┌─────────────────────────────────────────┐
│              BEGIN                      │
│  ┌─────────────────────────────────┐  │
│  │  Step 1: Check seat available   │  │
│  │  Step 2: Insert enrollment     │  │
│  │  Step 3: Update seat count     │  │
│  └─────────────────────────────────┘  │
│              │                          │
│      ┌───────┴───────┐                  │
│      ▼               ▼                  │
│   COMMIT          ROLLBACK              │
│   (Save all)      (Undo all)            │
└─────────────────────────────────────────┘
```

---

## 📖 Real-World University Scenarios

### Scenario 1: Enrolling a Student in a Course
```sql
BEGIN;

-- Step 1: Check if a seat is available
SELECT seats_available FROM courses WHERE course_id = 101;

-- Step 2: Insert the enrollment record
INSERT INTO enrollments (student_id, course_id, semester)
VALUES (1, 101, 'Fall 2025');

-- Step 3: Decrease available seats
UPDATE courses
SET seats_available = seats_available - 1
WHERE course_id = 101;

COMMIT;
```
* **Why a transaction?** If the server crashes after step 2 but before step 3, the student is enrolled but the seat count remains wrong. A transaction ensures both happen together.

### Scenario 2: Transferring a Student to Another Department
```sql
BEGIN;

-- Step 1: Update the student's department
UPDATE students
SET department_id = 2
WHERE student_id = 1;

-- Step 2: Update old department student count
UPDATE departments
SET student_count = student_count - 1
WHERE department_id = 1;

-- Step 3: Update new department student count
UPDATE departments
SET student_count = student_count + 1
WHERE department_id = 2;

COMMIT;
```
* **Why a transaction?** If step 1 succeeds but step 2 fails, the student is transferred to department 2, but both departments will display incorrect headcounts. A transaction keeps everything consistent.

### Scenario 3: Dropping a Course
```sql
BEGIN;

-- Step 1: Delete the enrollment
DELETE FROM enrollments
WHERE student_id = 1 AND course_id = 101;

-- Step 2: Free up the seat
UPDATE courses
SET seats_available = seats_available + 1
WHERE course_id = 101;

-- Step 3: Update student's total credits
UPDATE students
SET total_credits = total_credits - 3
WHERE student_id = 1;

COMMIT;
```
* **Why a transaction?** If step 1 succeeds but step 3 fails, the student loses the course access but still keeps the credits counted. A transaction prevents this inconsistency.

---

## 📖 Savepoints

A savepoint is a marker inside a transaction. You can roll back to a savepoint without rolling back the entire transaction.

### Syntax
```sql
BEGIN;

-- Operation 1
INSERT INTO grades (enrollment_id, letter_grade, numeric_grade)
VALUES (500, 'A', 95);

SAVEPOINT after_first_grade;    -- Create a savepoint

-- Operation 2 (might fail)
INSERT INTO grades (enrollment_id, letter_grade, numeric_grade)
VALUES (501, 'B+', 88);

-- Oh no! This grade is wrong.
ROLLBACK TO SAVEPOINT after_first_grade;  -- Undo only operation 2

-- Operation 3 (corrected)
INSERT INTO grades (enrollment_id, letter_grade, numeric_grade)
VALUES (501, 'B', 85);

COMMIT;  -- Saves operation 1 and operation 3
```

### Real-World Use Case: Batch Grade Upload
A professor uploads 100 grades at the end of the semester. If grade #47 has an invalid letter grade, they don't want to abort and re-upload all 100. They use a savepoint to roll back just the single bad grade row and continue the rest of the execution batch.

---

## 📖 Concurrency Basics

### What Happens When Two Users Modify the Same Data?
Imagine two students trying to enroll in the very last seat of a popular course at exactly the same time.

#### Without Isolation:
1. Student A reads: `seats_available = 1`
2. Student B reads: `seats_available = 1` *(at the same time!)*
3. Student A enrolls: `seats_available = 0`
4. Student B enrolls: `seats_available = -1` (**OVERBOOKED!**)

#### With Isolation:
1. Student A's transaction locks the targeted course row.
2. Student B must wait until Student A commits or rolls back.
3. Student B's scan then reads `seats_available = 0` and is blocked from enrolling.

*PostgreSQL handles this automatically using row-level locking and isolation levels.*

---

## 📖 Isolation Levels

Isolation levels control how much one transaction can see of another transaction's work while it is still in progress.



| Level | What It Means | Use When |
| :--- | :--- | :--- |
| **Read Committed** *(default)* | A transaction only sees data that was committed before it started. | Most standard applications. Good balance of safety and performance. |
| **Repeatable Read** | Within a transaction, reading the same data twice gives the same result, even if another transaction changed it. | Financial or auditing reports where transactional consistency matters. |
| **Serializable** | The strictest level. Transactions behave as if they ran sequentially one at a time. | Critical multi-step operations where any potential inconsistency is unacceptable. |

### Example: Read Committed vs. Repeatable Read
```sql
-- Transaction 1 starts
BEGIN;
SELECT gpa FROM students WHERE student_id = 1;  -- Returns 3.5

-- Meanwhile, Transaction 2 commits an update:
UPDATE students SET gpa = 3.7 WHERE student_id = 1;
COMMIT;

-- Transaction 1 reads again:
SELECT gpa FROM students WHERE student_id = 1;
-- Read Committed: Returns 3.7 (sees the committed change)
-- Repeatable Read: Returns 3.5 (does not see the change within this transaction)
```
💡 **Beginner Rule:** Stick with **Read Committed** (the default) until you have a specific architectural reason to change it.

---

## 📖 Common Transaction Pitfalls



| Pitfall | What Happens | How to Avoid |
| :--- | :--- | :--- |
| **Long-running transactions** | A transaction stays open for hours, holding locks and blocking other users. | Keep transactions short. Handle application logic outside the block. |
| **Deadlocks** | Two transactions wait for each other to release locks. PostgreSQL kills one. | Access tables in the exact same order. Keep transactions short. |
| **Forgetting to end blocks** | The transaction stays open indefinitely, holding locks. | Always end transactions explicitly with `COMMIT` or `ROLLBACK`. |
| **Monolithic batches** | A transaction with thousands of operations becomes slow and highly risky. | Break massive data loads into smaller, manageable transaction batches. |

---

## 🌍 Real-World Applications

### Banking: Money Transfer
```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE account_id = 'A';
UPDATE accounts SET balance = balance + 100 WHERE account_id = 'B';
COMMIT;
```
***Why:** If the server crashes after the first update, money vanishes. A transaction guarantees that either both accounts are balanced or no changes persist.*

### E-Commerce: Placing an Order
1. Check if the item is in stock.
2. Reserve the item in the inventory ledger.
3. Charge the payment gateway.
4. Create the final order dispatch record.
*All four steps must succeed as a single unit, or the customer is never charged and the item remains available in stock.*

### University: Finalizing Grades
At the end of the semester, a professor updates 30 grades. If the system crashes after 15 updates, half the class has new grades processed and the other half does not. A transaction ensures all grades are finalized together.

---

## ⚠️ Common Beginner Mistakes



| Mistake | Example | Fix |
| :--- | :--- | :--- |
| **Forgetting COMMIT** | Running `BEGIN; INSERT ...;` and walking away | Always end with `COMMIT;` or `ROLLBACK;`. |
| **Transaction overhead on SELECT** | `BEGIN; SELECT * FROM students; COMMIT;` | Unnecessary. Single `SELECT` statements do not need manual transaction wrapping. |
| **Not handling application errors** | App crashes mid-transaction, leaving locks open | Use try/catch blocks to trigger an automatic `ROLLBACK` on error. |
| **Ignoring deadlock pathways** | Two transactions update the same rows in opposite order | Ensure applications access database tables in a consistent order. |

---

## ✅ Quick Check Questions

1. What is a transaction, and why is the "all or nothing" rule important?
2. What do the four letters in ACID stand for?
3. What is the difference between `COMMIT` and `ROLLBACK`?
4. What is a savepoint, and when would you use one?
5. Why is isolation important when two users try to enroll in the last seat of a course?
6. What is the default isolation level in PostgreSQL?
7. What is a deadlock, and how can you avoid it?
8. Why should transactions be kept short?

---

## 🎯 Key Takeaways

* 📦 **Atomic Wrapping:** A transaction wraps multiple SQL operations into one cohesive unit that succeeds or fails together.
* 🛡️ **ACID Guardrails:** ACID guarantees that database transactions are **A**tomic, **C**onsistent, **I**solated, and **D**urable.
* 🕹️ **Control Flow:** Use `BEGIN` to start, `COMMIT` to save changes permanently, and `ROLLBACK` to undo everything.
* 📍 **Partial Rollbacks:** Savepoints let you roll back a specific part of a transaction without losing the preceding commands.
* 🚦 **Visibility Filters:** Isolation levels control what changes one transaction can observe of an in-flight operation.

## 📚 Learning Resources

### 🎥 YouTube Videos





| Title | Why Useful | Link |
| :--- | :--- | :--- |
| **SQL Transactions Explained by Fireship** | Fast, visual, covers `BEGIN`/`COMMIT`/`ROLLBACK` in 10 minutes. | [Watch on YouTube](https://youtube.com) |
| **Database Transactions & ACID by freeCodeCamp** | Deep dive into theoretical ACID properties with practical examples. | [Watch on YouTube](https://youtube.com) |
| **PostgreSQL Transactions by Programming with Mosh** | Clear, beginner-friendly, and highly practical syntax guide. | [Watch on YouTube](https://youtube.com) |
| **ACID Properties Explained by Amigoscode** | Project-based architectural layout mirroring real-world context. | [Watch on YouTube](https://youtube.com) |

### 📄 Official Documentation
* [PostgreSQL 16: BEGIN](https://postgresql.org) — Technical manual syntax layout for launching manual block transactions.
* [PostgreSQL 16: COMMIT](https://postgresql.org) — Reference blueprints for permanently saving open write streams.
* [PostgreSQL 16: ROLLBACK](https://postgresql.org) — Framework guide for completely undoing open modification trees.
* [PostgreSQL 16: SAVEPOINT](https://postgresql.org) — Instructions for establishing custom checkpoints inside code lines.
* [PostgreSQL 16: Transaction Isolation](https://postgresql.org) — Deep analysis of standard data isolation concurrency boundaries.

### 🌍 Articles
* **"SQL Transactions Explained" by W3Schools** — Highly accessible text-based explanations coupled with simple, try-it-out script playgrounds.
* **"ACID Properties in DBMS" by GeeksforGeeks** — Granular academic breakdown detailing architectural states using flowchart blocks.
* **"PostgreSQL Transactions" by PostgreSQL Tutorial** — Real-world code patterns written explicitly around relational data workflows.

### 🧪 Practice Platforms





| Platform | Why Useful | Link |
| :--- | :--- | :--- |
| **pgExercises** | PostgreSQL-specific target sandboxes mapping out complex data transactions. | [pgexercises.com](https://pgexercises.com) |
| **SQLBolt** | Interactive browser tutorials evaluating table adjustment scripts line-by-line. | [sqlbolt.com](https://sqlbolt.com) |
| **HackerRank SQL** | Problem solving platforms testing analytical multi-step query thinking. | [hackerrank.com/domains/sql](https://hackerrank.com) |
