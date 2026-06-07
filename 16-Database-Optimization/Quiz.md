# Module 16: Quiz — Database Optimization in PostgreSQL

> **Instructions:** Answer the following questions to check your understanding. Answers and explanations are hidden under each question button.

---

## 🔄 True or False

### Question 1
`SELECT *` is always the best choice because it is shorter to write.

- [ ] True
- [x] False

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **False**
* **Why:** `SELECT *` fetches every single column from disk, which wastes system memory, network bandwidth, and processing time. The best practice is to explicitly name only the columns your application actually needs.
</details>

---

### Question 2
A Cartesian product occurs when you forget to specify JOIN conditions between tables.

- [x] True
- [ ] False

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **True**
* **Why:** Omitting a `JOIN` or `WHERE` condition that links table keys causes the database engine to pair every single row of the first table with every row of the second table, creating a massive, unintended data set multiplication.
</details>

---

### Question 3
`VACUUM` removes dead rows and reclaims storage space.

- [x] True
- [ ] False

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **True**
* **Why:** Because of PostgreSQL's MVCC design, updated or deleted rows are only marked as "dead" and remain on disk. Running `VACUUM` purges these dead entries and frees up the physical storage space.
</details>

---

### Question 4
Denormalization always improves database performance with no downsides.

- [ ] True
- [x] False

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **False**
* **Why:** While denormalization speeds up complex read queries by cutting down on `JOIN` operations, it introduces significant downsides, including severe data redundancy, larger storage requirements, and high risks of data inconsistency during updates.
</details>

---

### Question 5
The N+1 query problem can be solved by using a single JOIN query instead of multiple queries in a loop.

- [x] True
- [ ] False

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **True**
* **Why:** Fetching a primary list and then executing a separate query inside an application loop triggers a massive volume of network round-trips. Replacing that pattern with one single `JOIN` query lets the database assemble the full dataset at once.
</details>

---

## 🔘 Multiple Choice

### Question 6
What does `EXPLAIN ANALYZE` do?

- [ ] A) It explains the database schema to new users.
- [x] B) It shows the query plan and actually runs the query to report real execution time.
- [ ] C) It automatically rewrites slow queries to be faster.
- [ ] D) It creates missing indexes automatically.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **B) It shows the query plan and actually runs the query to report real execution time.**
* **Why:** Adding the `ANALYZE` keyword tells the engine to fully execute the SQL statement. This measures real runtime statistics and loop variations rather than just providing initial estimations.
</details>

---

### Question 7
Which of the following is a sign that a query might need optimization?

- [ ] A) It uses `WHERE` to filter rows.
- [x] B) `EXPLAIN` shows `Seq Scan` on a large table that is frequently queried.
- [ ] C) It returns exactly 10 rows.
- [ ] D) It uses `JOIN` to combine two tables.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **B) EXPLAIN shows Seq Scan on a large table that is frequently queried.**
* **Why:** A Sequential Scan (`Seq Scan`) means the planner must read the entire table from top to bottom. Seeing this on a large table that handles high traffic implies a missing index is causing a performance bottleneck.
</details>

---

### Question 8
When should you use `LIMIT`?

- [ ] A) To delete rows from a table.
- [x] B) To restrict the number of rows returned, especially for pagination.
- [ ] C) To create a new table with limited columns.
- [ ] D) To add a constraint to a column.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **B) To restrict the number of rows returned, especially for pagination.**
* **Why:** `LIMIT` forces the engine to halt scanning once it collects a specific number of output rows. This prevents large result sets from overwhelming memory boundaries, making it essential for page-by-page browsing.
</details>

---

### Question 9
What is the main risk of denormalization?

- [ ] A) Queries become slower.
- [x] B) Data may become inconsistent if the source tables change but the denormalized copy is not updated.
- [ ] C) It requires more JOINs.
- [ ] D) It uses less disk space.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **B) Data may become inconsistent if the source tables change but the denormalized copy is not updated.**
* **Why:** Since a denormalized schema intentionally duplicates data into flat structures, changing a record in its master table leaves cached entries stale unless you deploy extra trigger systems or scheduled syncs to update them.
</details>

---

### Question 10
Which data type is most appropriate for storing a student's GPA?

- [ ] A) `TEXT`
- [ ] B) `VARCHAR(50)`
- [x] C) `NUMERIC(3,2)`
- [ ] D) `BOOLEAN`

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **C) NUMERIC(3,2)**
* **Why:** `NUMERIC(3,2)` stores highly precise decimal parameters (e.g., maximum 3 digits total, with exactly 2 digits after the decimal point). This is ideal for accurate GPA indexing and math calculations, unlike flat text rows.
</details>

---

## ✏️ Fill in the Blank

### Question 11
The command to update PostgreSQL's table statistics after a large data load is `_________ table_name;`

```text
ANALYZE
```

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** `ANALYZE`
* **Why:** The explicit keyword `ANALYZE` scans table column values to recalculate data distributions, updating the system catalogs so the query planner can choose the best execution paths.
</details>

---

### Question 12
A query that runs one initial query and then one additional query per result row is called the _______ query problem.

```text
N+1
```

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** `N+1`
* **Why:** This performance pitfall occurs when an initial query fetches 1 parent row list, and then loops over the results to fire an additional sub-query for each of the $N$ individual records.
</details>

---

## ✍️ Short Answer

### Question 13
Explain in 2–3 sentences why adding an index on every column of a table is a bad idea.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Answer:** Indexing every column introduces severe processing overhead, because every `INSERT`, `UPDATE`, or `DELETE` transaction forces the database engine to update the underlying table data and immediately rebuild every single index tree map. Additionally, it consumes massive amounts of physical disk space and can complicate path selections for the query planner, resulting in poor runtime optimization choices.
</details>

---

### Question 14
A university portal displays a list of 50,000 students. The page takes 12 seconds to load. Describe two specific optimizations you would apply to improve this.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Answer:** First, I would implement cursor-based or offset pagination by adding `LIMIT 20 OFFSET 0` to the query layout so the interface only fetches a tiny batch of rows at a time instead of all 50,000 records. Second, I would build a targeted B-tree index on the specific column used for sorting (e.g., `CREATE INDEX idx_students_last_name ON students(last_name)`) to completely eliminate the need for expensive in-memory sort scans during page reloads.
</details>
