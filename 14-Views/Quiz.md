# Module 14: Quiz — Views in PostgreSQL

&gt; **Instructions:** Answer the following questions to check your understanding. Answers are hidden at the bottom.

---

## True or False

### Question 1
A view stores a copy of the data from the underlying tables.

- [ ] True
- [ ] False

---

### Question 2
You can always insert, update, and delete data through any view.

- [ ] True
- [ ] False

---

### Question 3
A materialized view stores query results on disk and must be refreshed manually.

- [ ] True
- [ ] False

---

### Question 4
Views can be used to hide sensitive columns from certain users.

- [ ] True
- [ ] False

---

## Multiple Choice

### Question 5
What is the correct syntax to create a view?

- A) `CREATE TABLE view_name AS SELECT ...`
- B) `CREATE VIEW view_name AS SELECT ...`
- C) `MAKE VIEW view_name AS SELECT ...`
- D) `NEW VIEW view_name AS SELECT ...`

---

### Question 6
Which of the following makes a view **non-updatable**?

- A) It selects from a single table.
- B) It uses `GROUP BY` and `COUNT()`.
- C) It has a simple `WHERE` clause.
- D) It selects specific columns by name.

---

### Question 7
What happens to a view when the underlying table data changes?

- A) The view must be recreated.
- B) The view automatically reflects the new data.
- C) The view is deleted.
- D) The view stores the old data forever.

---

### Question 8
Why would a database administrator grant access to a view instead of the underlying table?

- A) Views run faster than tables.
- B) Views can hide sensitive data and restrict what users see.
- C) Views do not require any storage space.
- D) Views automatically back up data.

---

### Question 9
Which command refreshes a materialized view?

- A) `UPDATE MATERIALIZED VIEW view_name;`
- B) `REFRESH MATERIALIZED VIEW view_name;`
- C) `RELOAD MATERIALIZED VIEW view_name;`
- D) `REBUILD MATERIALIZED VIEW view_name;`

---

### Question 10
What is the main advantage of using a view for a complex 5-table JOIN query?

- A) Views make the query run faster.
- B) Views store the data permanently so you don't need the tables.
- C) You write the complex query once and query the view simply afterward.
- D) Views automatically create indexes on the joined tables.

---

## Fill in the Blank

### Question 11
Complete the missing word: `CREATE _______ student_roster AS SELECT ... FROM students;`

---

### Question 12
To safely remove a view that might not exist, use: `DROP _______ IF EXISTS view_name;`

---

## Short Answer

### Question 13
Explain in 2–3 sentences why a view that uses `GROUP BY` and `COUNT()` cannot be used to insert new rows.

---

### Question 14
Describe a real-world university scenario where creating a view would be better than giving a staff member direct access to the `students` table.

---

## Answers

&lt;details&gt;
&lt;summary&gt;Click to reveal answers&lt;/summary&gt;

1. **False** — A view stores a query definition, not a copy of the data.
2. **False** — Only simple views (single table, no aggregates) are updatable. Complex views are read-only.
3. **True** — Materialized views store results on disk and require `REFRESH MATERIALIZED VIEW`.
4. **True** — Views can select only specific columns, hiding sensitive data from users.
5. **B** — `CREATE VIEW view_name AS SELECT ...`
6. **B** — `GROUP BY` and aggregate functions like `COUNT()` make a view non-updatable.
7. **B** — The view automatically reflects the new data because it reruns the underlying query.
8. **B** — Views can hide sensitive data and restrict what users see.
9. **B** — `REFRESH MATERIALIZED VIEW view_name;`
10. **C** — You write the complex query once and query the view simply afterward.
11. **VIEW**
12. **VIEW**
13. *(Sample Answer)* A view with `GROUP BY` and `COUNT()` produces summary rows that do not correspond to single rows in the underlying table. PostgreSQL cannot determine which underlying table rows to create or modify when you try to insert through the view.
14. *(Sample Answer)* A university help desk staff member needs to look up student contact information to send announcements. Instead of giving them access to the full `students` table (which contains sensitive data like GPA, financial status, and personal notes), the administrator creates a `student_directory` view that only shows `student_id`, `first_name`, `last_name`, and `email`. This protects sensitive data while still allowing the staff to do their job.

&lt;/details&gt;
