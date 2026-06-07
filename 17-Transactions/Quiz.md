# Module 17: Quiz — Transactions in PostgreSQL

> **Instructions:** Answer the following questions to check your understanding. Answers and explanations are hidden under each question button.

---

## 🔄 True or False

### Question 1
A transaction ensures that either all operations succeed, or none of them are saved.

- [x] True
- [ ] False

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **True**
* **Why:** This represents the core principle of **Atomicity** (the 'A' in ACID). Multiple database updates are securely bound together as a single atomic unit, ensuring an absolute all-or-nothing execution footprint.
</details>

---

### Question 2
`ROLLBACK` permanently saves all changes made during a transaction.

- [ ] True
- [x] False

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **False**
* **Why:** `ROLLBACK` wipes out all uncommitted data modifications made during the transaction, safely returning the database to its pre-transaction state. To save changes permanently, you must use `COMMIT`.
</details>

---

### Question 3
A savepoint allows you to roll back part of a transaction without undoing everything.

- [x] True
- [ ] False

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **True**
* **Why:** Savepoints act as localized checkpoints within an open transaction block. They allow application workflows to perform partial rollbacks to a specific point without losing preceding valid operations.
</details>

---

### Question 4
In PostgreSQL, `Read Committed` is the default transaction isolation level.

- [x] True
- [ ] False

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **True**
* **Why:** `Read Committed` is PostgreSQL's default isolation level. It ensures that queries within a transaction can only observe data that was physically committed before the query execution loop began.
</details>

---

### Question 5
A deadlock occurs when two transactions wait forever for each other to release locks.

- [x] True
- [ ] False

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **True**
* **Why:** A deadlock represents a circular lock dependency dependency block. Since neither transaction can proceed without the other releasing its exclusive row lock, PostgreSQL steps in automatically to terminate one of the connections.
</details>

---

## 🔘 Multiple Choice

### Question 6
What does the `A` in ACID stand for?

- [ ] A) Availability
- [x] B) Atomicity
- [ ] C) Association
- [ ] D) Authentication

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **B) Atomicity**
* **Why:** **Atomicity** guarantees that a compound transaction block behaves as a single indivisible operation, ensuring any system crash mid-execution completely rolls back data changes.
</details>

---

### Question 7
Which SQL command ends a transaction and makes all changes permanent?

- [ ] A) `BEGIN`
- [ ] B) `ROLLBACK`
- [x] C) `COMMIT`
- [ ] D) `SAVEPOINT`

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **C) COMMIT**
* **Why:** Executing `COMMIT` commands the database engine to flush uncommitted logs directly onto hard drives, closing the block and making updates visible to all external connections permanently.
</details>

---

### Question 8
You start a transaction, insert a row, and then run `ROLLBACK`. What happens to the inserted row?

- [ ] A) It is saved permanently.
- [x] B) It is discarded and never existed in the database.
- [ ] C) It is saved but marked as temporary.
- [ ] D) It is moved to a backup table.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **B) It is discarded and never existed in the database.**
* **Why:** Running `ROLLBACK` purges the volatile transaction log buffer immediately, ensuring the uncommitted insertion is completely discarded without leaving traces in physical tables.
</details>

---

### Question 9
Which isolation level prevents a transaction from seeing changes made by other transactions after it started?

- [ ] A) Read Committed
- [x] B) Repeatable Read
- [ ] C) Read Uncommitted
- [ ] D) Serializable

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **B) Repeatable Read**
* **Why:** The `Repeatable Read` isolation level freezes data visibility for the duration of the transaction. Re-running the identical select scan later guarantees identical results, even if separate external queries committed updates in the meantime.
</details>

---

### Question 10
What is the best way to avoid deadlocks in most applications?

- [ ] A) Use only one table per transaction.
- [x] B) Access tables in the same order and keep transactions short.
- [ ] C) Never use transactions.
- [ ] D) Always use the Serializable isolation level.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **B) Access tables in the same order and keep transactions short.**
* **Why:** Enforcing a strict, consistent sequence of table access pathways across all application code modules prevents circular locking dependencies from developing, successfully cutting down deadlock frequencies.
</details>

---

## ✏️ Fill in the Blank

### Question 11
The SQL command to start a transaction block is `_________`.

```text
BEGIN
```

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** `BEGIN` (or `START TRANSACTION`)
* **Why:** The keyword `BEGIN` instructs PostgreSQL to open a private, isolated execution space for managing the ensuing sequence of write queries.
</details>

---

### Question 12
The four ACID properties are Atomicity, Consistency, _________, and Durability.

```text
Isolation
```

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** `Isolation`
* **Why:** **Isolation** defines data visibility boundaries, ensuring concurrently running database updates operate safely without leaking uncommitted data into other transactions.
</details>

---

## ✍️ Short Answer

### Question 13
Explain in 2–3 sentences why a bank transfer (deduct $100 from Account A, add $100 to Account B) must be wrapped in a transaction.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Answer:** A bank transfer demands strict financial integrity. If the host platform suffers a hardware failure immediately after deducting money from Account A but before adding it to Account B, the $100 vanishes entirely from the system. Wrapping both steps inside a transaction ensures that either both adjustments are successfully saved together, or the entire operation rolls back to keep balances correct.
</details>

---

### Question 14
Describe a scenario in the University Management System where using a `SAVEPOINT` would be better than rolling back an entire transaction.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Answer:** Imagine a registrar uploading a massive text spreadsheet containing 500 new student enrollment lines within a single transaction block. If entry number 412 contains a data typo that triggers a constraint check error, rolling back the entire transaction forces the assistant to abort and re-upload the entire batch from the beginning. Using savepoints after every valid row entry allows the application to catch individual validation exceptions, roll back strictly that single bad row, and safely proceed with uploading the remaining lines.
</details>
