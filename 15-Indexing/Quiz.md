# Module 15: Quiz — Indexing in PostgreSQL

> **Instructions:** Answer the following questions to check your understanding. Answers and explanations are hidden under each question button.

---

## 🔄 True or False

### Question 1
An index stores a complete copy of the table data.

- [ ] True
- [x] False

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **False**
* **Why:** An index does not duplicate the full table data. Instead, it stores a optimized mapping of specific column values along with physical pointers (row identifiers/TIDs) that tell PostgreSQL exactly where the corresponding row lives on disk.
</details>

---

### Question 2
Creating too many indexes can slow down INSERT and UPDATE operations.

- [x] True
- [ ] False

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **True**
* **Why:** Every index comes with a write overhead cost. Whenever you run an `INSERT`, `UPDATE`, or `DELETE` transaction, PostgreSQL must update the underlying table data and immediately restructure or re-balance all related index trees.
</details>

---

### Question 3
A multicolumn index on `(A, B)` can speed up queries that filter on `B` alone.

- [ ] True
- [x] False

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **False**
* **Why:** Multicolumn indexes are order-dependent. PostgreSQL sorts the tree by the first column (`A`), and then by the second column (`B`) within groups of `A`. A query filtering strictly on `B` alone cannot navigate this hierarchy and will default to a sequential scan.
</details>

---

### Question 4
`EXPLAIN ANALYZE` actually runs the query and shows real execution time.

- [x] True
- [ ] False

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **True**
* **Why:** While a basic `EXPLAIN` only outputs structural estimations, appending `ANALYZE` forces PostgreSQL to execute the statement entirely, capturing actual runtime measurements and loop data (use with caution on large `DELETE` operations!).
</details>

---

### Question 5
A partial index is smaller than a full index because it only indexes a subset of rows.

- [x] True
- [ ] False

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **True**
* **Why:** A partial index includes a `WHERE` clause that restricts index membership. Because it ignores records that don't match the condition, it occupies less disk space and stays highly cached.
</details>

---

## 🔘 Multiple Choice

### Question 6
What is PostgreSQL's default index type?

- [ ] A) Hash
- [x] B) B-tree
- [ ] C) GIN
- [ ] D) GiST

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **B) B-tree**
* **Why:** The B-tree (Balanced Tree) data structure is PostgreSQL's highly reliable default index type. It handles equality, range, sorting, and specific wildcard lookups efficiently.
</details>

---

### Question 7
Which of the following is the best reason to create an index?

- [ ] A) To store backup copies of table data.
- [x] B) To speed up SELECT queries that filter on a specific column.
- [ ] C) To prevent duplicate rows from being inserted.
- [ ] D) To automatically sort data in the table.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **B) To speed up SELECT queries that filter on a specific column.**
* **Why:** The primary role of an index is performance acceleration. It allows search queries to isolate target records quickly without evaluating every row in the file.
</details>

---

### Question 8
You run `EXPLAIN` and see `Seq Scan on students`. What does this mean?

- [ ] A) PostgreSQL is using an index to find the rows.
- [x] B) PostgreSQL is reading every row in the table.
- [ ] C) PostgreSQL is skipping the table entirely.
- [ ] D) PostgreSQL is using a cached result.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **B) PostgreSQL is reading every row in the table.**
* **Why:** A Sequential Scan (`Seq Scan`) represents a full table scan. The query planner has decided to read the entire table structure from top to bottom on disk, either because an index does not exist or wouldn't be useful.
</details>

---

### Question 9
What is a "covering index"?

- [ ] A) An index that covers all columns in the table.
- [x] B) An index that contains all columns needed by a query, so the table does not need to be accessed.
- [ ] C) An index that protects the table from deletion.
- [ ] D) An index that automatically updates itself.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **B) An index that contains all columns needed by a query, so the table does not need to be accessed.**
* **Why:** A covering index allows for an **Index Only Scan**. Since the index leaf nodes already hold every field requested by the `SELECT` query, PostgreSQL can return results directly without wasting time performing heap lookups on the main table blocks.
</details>

---

### Question 10
Which command rebuilds a bloated index?

- [ ] A) `REBUILD INDEX index_name;`
- [x] B) `REINDEX INDEX index_name;`
- [ ] C) `REFRESH INDEX index_name;`
- [ ] D) `RECREATE INDEX index_name;`

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **B) REINDEX INDEX index_name;**
* **Why:** Massive data updates cause index page fragmentation (bloat) over time. The explicit command `REINDEX` reconstructs a clean, fresh, defragmented version of the index tree structure.
</details>

---

## ✏️ Fill in the Blank

### Question 11
The SQL command to create an index on the `email` column of the `students` table is: `CREATE _______ idx_email ON students(email);`

```text
INDEX
```

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** `INDEX`
* **Why:** The standard ANSI SQL definition format relies on the explicit declaration `CREATE INDEX` to declare and build independent index catalog configurations.
</details>

---

### Question 12
To see whether PostgreSQL uses an index for a query, you can run `_______ ANALYZE SELECT ...`

```text
EXPLAIN
```

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** `EXPLAIN`
* **Why:** Prepended to execution statements, `EXPLAIN` commands the optimizer engine to breakdown execution costs, join strategies, and scanning metrics.
</details>

---

## ✍️ Short Answer

### Question 13
Explain in 2–3 sentences why you should not index a boolean column like `is_active` that has only two possible values.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Answer:** A boolean column exhibits low cardinality (only holding true/false statuses). An index tree under these conditions splits into two massive branches, meaning an index scan will still point to roughly half the physical database blocks. Because the overhead of maintaining the index outweighs any potential search performance gain, a standard sequential scan is almost always faster.
</details>

---

### Question 14
A university portal searches for students by `last_name` 1,000 times per day, but only adds 5 new students per day. Should you create an index on `last_name`? Why or why not?

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Answer:** Yes, you should create an index on `last_name`. The system exhibits a heavy read-to-write ratio (1,000 lookups versus only 5 insertions daily). The immense time savings provided by accelerating those 1,000 read queries far outweighs the minor processing cost introduced during the 5 student registration operations.
</details>
