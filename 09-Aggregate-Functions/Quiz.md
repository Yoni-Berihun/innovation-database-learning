# ✅ Quiz — Aggregate Functions in PostgreSQL

Test your understanding before moving to the next module. There are 10 questions. Take your time.

---

## Question 1 (Multiple Choice)

What does `COUNT(*)` do?

- [ ] Adds all values in a column
- [x] Counts all rows in a table
- [ ] Finds the average of a column
- [ ] Returns the highest value

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **Counts all rows in a table**
* **Why:** `COUNT(*)` counts every single row in the table, regardless of whether the rows contain NULL values or duplicates. It is the most common way to get the total number of records.
</details>

---

## Question 2 (Multiple Choice)

Which aggregate function calculates the average of numeric values?

- [ ] SUM()
- [ ] COUNT()
- [x] AVG()
- [ ] MEAN()

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **AVG()**
* **Why:** `AVG()` is the standard SQL function used to calculate the arithmetic mean of numeric data. `MEAN()` is not a valid function name in standard SQL.
</details>

---

## Question 3 (Multiple Choice)

What does `SUM(credits)` compute?

- [ ] The number of courses
- [x] The total of all credit values
- [ ] The average credits per course
- [ ] The highest credit value

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **The total of all credit values**
* **Why:** `SUM()` adds together all the values within a numeric column across the targeted rows. In this case, it sums up the credit hours of the selected courses.
</details>

---

## Question 4 (Multiple Choice)

What is the difference between `COUNT(*)` and `COUNT(gpa)`?

- [ ] There is no difference
- [x] COUNT(*) counts all rows; COUNT(gpa) ignores rows where gpa is NULL
- [ ] COUNT(*) is faster
- [ ] COUNT(gpa) counts all rows including NULLs

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **COUNT(*) counts all rows; COUNT(gpa) ignores rows where gpa is NULL**
* **Why:** `COUNT(*)` focuses on counting table rows as a whole. `COUNT(column_name)` focuses on a specific column and strictly excludes any rows where that column's value is missing (`NULL`).
</details>

---

## Question 5 (Multiple Choice)

Which query finds the highest GPA among all students?

- [x] SELECT MAX(gpa) FROM students;
- [ ] SELECT HIGHEST(gpa) FROM students;
- [ ] SELECT TOP(gpa) FROM students;
- [ ] SELECT BIGGEST(gpa) FROM students;

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Correct Answer:** **SELECT MAX(gpa) FROM students;**
* **Why:** `MAX()` is the standard SQL keyword for finding the largest value in a column. Words like `HIGHEST`, `TOP`, and `BIGGEST` are not valid SQL aggregate functions.
</details>

---

## Question 6 (Short Answer)

Write a query to find the total budget of all departments.

```sql
SELECT SUM(budget) FROM departments;
```

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

```sql
SELECT SUM(budget) FROM departments;
```
* **Why:** `SUM(budget)` targets the numeric `budget` column and adds up all the rows inside the `departments` table to output one combined dollar amount.
</details>

---

## Question 7 (Short Answer)

Write a query to find both the lowest and highest GPA among students in a single result.

```sql
SELECT MIN(gpa) AS lowest_gpa, MAX(gpa) AS highest_gpa FROM students;
```

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

```sql
SELECT MIN(gpa) AS lowest_gpa, MAX(gpa) AS highest_gpa FROM students;
```
* **Why:** You can request multiple aggregates in a single `SELECT` statement. `MIN(gpa)` grabs the smallest value, and `MAX(gpa)` grabs the largest value from the same table scan.
</details>

---

## Question 8 (Short Answer)

Explain what happens if you try to use `SUM()` on a text column like `first_name`. Why does this error occur?

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Answer:** Running `SUM(first_name)` will cause PostgreSQL to throw a **Data Type Mismatch Error** (e.g., *function sum(character varying) does not exist*).
* **Why:** `SUM()` mathematically adds numbers together. Text data (strings) cannot be added mathematically. If you need to evaluate text columns, you must use `COUNT()` to see how many text values exist instead of attempting a sum.
</details>

---

## Question 9 (Short Answer)

The dean wants to know: how many students have a GPA of 3.5 or higher? Write the query.

```sql
SELECT COUNT(*) FROM students WHERE gpa >= 3.5;
```

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

```sql
SELECT COUNT(*) FROM students WHERE gpa >= 3.5;
```
* **Why:** The `WHERE` clause runs first to filter out any students with a GPA lower than 3.5. Then, `COUNT(*)` counts the rows that successfully passed the evaluation filter.
</details>

---

## Question 10 (Short Answer)

Describe a real-world scenario where a university would use each of these aggregate functions: `COUNT()`, `SUM()`, `AVG()`, `MIN()`, and `MAX()`.

<details>
<summary><b>🔍 Show Answer & Explanation</b></summary>

* **Answer:**
  * **COUNT():** Counting the total headcount of registered students this semester or tracking the number of courses offered.
  * **SUM():** Adding together tuition payments to see total revenue or calculating the collective financial budget across all university departments.
  * **AVG():** Tracking the overall average student body GPA or evaluating the average size of classes for capacity planning.
  * **MIN():** Finding the lowest GPA to identify students needing academic probation or identifying the earliest hire date to spot the most senior staff member.
  * **MAX():** Identifying the highest GPA to choose a valedictorian or looking up the highest department budget for financial reporting.
</details>
