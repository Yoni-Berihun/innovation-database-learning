# 🧱 Database Fundamentals

Welcome back! In Module 01, you learned what a database is and why it matters. You discovered that databases power everything from Netflix to your university portal. You met PostgreSQL — the powerful, free database system we'll use throughout this course.

Now it's time to peek under the hood and understand **how databases actually organize information**.

Think of this module like learning the anatomy of a car before you start driving. You don't need to be a mechanic, but understanding what the engine, wheels, and steering wheel do makes you a much better driver. Similarly, understanding tables, rows, columns, keys, and relationships will make you a much better database builder.

By the end of this module, you'll look at any database and understand its skeleton. You'll see how a university tracks 20,000 students without everything turning into chaos. And you'll start thinking like a database designer — which is one of the most valuable skills in tech.

Let's build your foundation. 🏗️

---

## 🎯 Learning Objectives

After completing this module, you will be able to:

* Explain what a **table** is and why databases use them
* Describe the difference between **rows** and **columns**
* Understand what a **record** is in database terms
* Define a **Primary Key** and explain why every table needs one
* Define a **Foreign Key** and explain how tables connect to each other
* Identify **One-to-One**, **One-to-Many**, and **Many-to-Many** relationships
* Read a simple database schema and understand how the pieces fit together
* Apply these concepts to the **University Management System**
* Feel confident that you can design the basic structure of a database

---

## 🧠 Why This Matters

Let me tell you about Sarah.

Sarah is a second-year Computer Science student. She just landed her first internship at a small tech company. On her first day, her manager says: "We need to add a feature where users can save their favorite products. Can you design the database for that?"

Sarah opens her laptop and stares at a blank screen. She knows she needs to "make a table" but she's not sure:
* What columns should it have?
* How does it connect to the existing `users` table?
* What if two users save the same product?
* How do we make sure every saved favorite belongs to a real user?

Sarah wishes she had spent more time understanding **the fundamentals**.

The concepts in this module — tables, rows, columns, primary keys, foreign keys, and relationships — are the building blocks of every database you'll ever build or work with. They appear in:
* Every job interview
* Every database design discussion
* Every line of SQL you'll write
* Every system you'll ever build

Master these now, and everything that follows becomes easier. Skip them, and you'll feel lost when things get complex.

&gt; 🎓 **Our University Management System** will be your training ground. By the end of this module, you'll understand exactly how students, instructors, departments, courses, and enrollments connect to each other. That understanding will carry you through the entire course.

---

## 📖 What are Tables?

Imagine you're moving into a new apartment. You have a lot of stuff — clothes, books, kitchen items, electronics. You could throw everything into one giant box, but finding anything would be a nightmare.

Instead, you use **separate boxes for separate things**:
* One box for clothes
* One box for books
* One box for kitchen items
* One box for electronics

A **database table** is exactly like one of those boxes. It's a structure that holds **one type of thing**.

### The University Management System Tables

In our university, we have different "types of things" to track:

| Table | What It Stores |
|-------|---------------|
| `students` | Information about each student |
| `instructors` | Information about each professor |
| `departments` | Information about each academic department |
| `courses` | Information about each course offered |
| `enrollments` | Information about which student is in which course |

&gt; **Key Rule:** Each table stores ONE type of thing. We don't mix students and courses in the same table. Just like you wouldn't put your socks and your cereal in the same box.

### What Does a Table Look Like?

A table has a **name** and a set of **columns** that define what information we store about that thing.

Here's what our `students` table might look like conceptually:
![Uploading markmap.svg…]()
<svg xmlns="http://www.w3.org/2000/svg" class="markmap mm-l29ona-13" style="width: 100%; height: 100%;"><style>.markmap{--markmap-max-width: 9999px;--markmap-a-color: #0097e6;--markmap-a-hover-color: #00a8ff;--markmap-code-bg: #f0f0f0;--markmap-code-color: #555;--markmap-highlight-bg: #ffeaa7;--markmap-table-border: 1px solid currentColor;--markmap-font: 300 16px/20px sans-serif;--markmap-circle-open-bg: #fff;--markmap-text-color: #333;--markmap-highlight-node-bg: #ff02;font:var(--markmap-font);color:var(--markmap-text-color)}.markmap-link{fill:none}.markmap-node&gt;circle{cursor:pointer}.markmap-foreign{display:inline-block}.markmap-foreign p{margin:0}.markmap-foreign a{color:var(--markmap-a-color)}.markmap-foreign a:hover{color:var(--markmap-a-hover-color)}.markmap-foreign code{padding:.25em;font-size:calc(1em - 2px);color:var(--markmap-code-color);background-color:var(--markmap-code-bg);border-radius:2px}.markmap-foreign pre{margin:0}.markmap-foreign pre&gt;code{display:block}.markmap-foreign del{text-decoration:line-through}.markmap-foreign em{font-style:italic}.markmap-foreign strong{font-weight:700}.markmap-foreign mark{background:var(--markmap-highlight-bg)}.markmap-foreign table,.markmap-foreign th,.markmap-foreign td{border-collapse:collapse;border:var(--markmap-table-border)}.markmap-foreign img{display:inline-block}.markmap-foreign svg{fill:currentColor}.markmap-foreign&gt;div{width:var(--markmap-max-width);text-align:left}.markmap-foreign&gt;div&gt;div{display:inline-block}.markmap-highlight rect{fill:var(--markmap-highlight-node-bg)}.markmap-dark .markmap{--markmap-code-bg: #1a1b26;--markmap-code-color: #ddd;--markmap-circle-open-bg: #444;--markmap-text-color: #eee}</style><g transform="translate(20.000000000000057,317.57058613295214) scale(0.5432451751250893)"><path class="markmap-link" data-depth="3" data-path="1.2.3" d="M471,-175.719C511,-175.719,511,-281.406,551,-281.406" stroke-width="1.375" stroke="rgb(44, 160, 44)"/><path class="markmap-link" data-depth="3" data-path="1.2.4" d="M471,-175.719C511,-175.719,511,-255.031,551,-255.031" stroke-width="1.375" stroke="rgb(214, 39, 40)"/><path class="markmap-link" data-depth="3" data-path="1.2.5" d="M471,-175.719C511,-175.719,511,-228.656,551,-228.656" stroke-width="1.375" stroke="rgb(148, 103, 189)"/><path class="markmap-link" data-depth="3" data-path="1.2.6" d="M471,-175.719C511,-175.719,511,-202.281,551,-202.281" stroke-width="1.375" stroke="rgb(140, 86, 75)"/><path class="markmap-link" data-depth="3" data-path="1.2.7" d="M471,-175.719C511,-175.719,511,-175.906,551,-175.906" stroke-width="1.375" stroke="rgb(23, 190, 207)"/><path class="markmap-link" data-depth="3" data-path="1.2.8" d="M471,-175.719C511,-175.719,511,-149.531,551,-149.531" stroke-width="1.375" stroke="rgb(31, 119, 180)"/><path class="markmap-link" data-depth="3" data-path="1.2.9" d="M471,-175.719C511,-175.719,511,-123.156,551,-123.156" stroke-width="1.375" stroke="rgb(255, 127, 14)"/><path class="markmap-link" data-depth="3" data-path="1.2.10" d="M471,-175.719C511,-175.719,511,-96.781,551,-96.781" stroke-width="1.375" stroke="rgb(127, 127, 127)"/><path class="markmap-link" data-depth="3" data-path="1.2.11" d="M471,-175.719C511,-175.719,511,-70.406,551,-70.406" stroke-width="1.375" stroke="rgb(188, 189, 34)"/><path class="markmap-link" data-depth="4" data-path="1.12.13.14" d="M534,-8.75C574,-8.75,574,-39.125,614,-39.125" stroke-width="1.1875" stroke="rgb(23, 190, 207)"/><path class="markmap-link" data-depth="4" data-path="1.12.13.15" d="M534,-8.75C574,-8.75,574,-10.937,614,-10.937" stroke-width="1.1875" stroke="rgb(31, 119, 180)"/><path class="markmap-link" data-depth="4" data-path="1.12.13.16" d="M534,-8.75C574,-8.75,574,15.25,614,15.25" stroke-width="1.1875" stroke="rgb(255, 127, 14)"/><path class="markmap-link" data-depth="4" data-path="1.12.13.17" d="M534,-8.75C574,-8.75,574,41.438,614,41.438" stroke-width="1.1875" stroke="rgb(44, 160, 44)"/><path class="markmap-link" data-depth="4" data-path="1.12.18.19" d="M534,102C574,102,574,72.625,614,72.625" stroke-width="1.1875" stroke="rgb(148, 103, 189)"/><path class="markmap-link" data-depth="4" data-path="1.12.18.20" d="M534,102C574,102,574,98.813,614,98.813" stroke-width="1.1875" stroke="rgb(140, 86, 75)"/><path class="markmap-link" data-depth="4" data-path="1.12.18.21" d="M534,102C574,102,574,125,614,125" stroke-width="1.1875" stroke="rgb(227, 119, 194)"/><path class="markmap-link" data-depth="4" data-path="1.12.18.22" d="M534,102C574,102,574,151.188,614,151.188" stroke-width="1.1875" stroke="rgb(127, 127, 127)"/><path class="markmap-link" data-depth="3" data-path="1.12.13" d="M454,56.813C494,56.813,494,-8.75,534,-8.75" stroke-width="1.375" stroke="rgb(188, 189, 34)"/><path class="markmap-link" data-depth="3" data-path="1.12.18" d="M454,56.813C494,56.813,494,102,534,102" stroke-width="1.375" stroke="rgb(214, 39, 40)"/><path class="markmap-link" data-depth="4" data-path="1.23.24.25" d="M858,184.094C898,184.094,898,238,938,238" stroke-width="1.1875" stroke="rgb(31, 119, 180)"/><path class="markmap-link" data-depth="3" data-path="1.23.24" d="M454,197.469C494,197.469,494,184.094,534,184.094" stroke-width="1.375" stroke="rgb(23, 190, 207)"/><path class="markmap-link" data-depth="3" data-path="1.23.26" d="M454,197.469C494,197.469,494,210.469,534,210.469" stroke-width="1.375" stroke="rgb(255, 127, 14)"/><path class="markmap-link" data-depth="2" data-path="1.2" d="M207,11.25C247,11.25,247,-175.719,287,-175.719" stroke-width="1.75" stroke="rgb(255, 127, 14)"/><path class="markmap-link" data-depth="2" data-path="1.12" d="M207,11.25C247,11.25,247,56.813,287,56.813" stroke-width="1.75" stroke="rgb(127, 127, 127)"/><path class="markmap-link" data-depth="2" data-path="1.23" d="M207,11.25C247,11.25,247,197.469,287,197.469" stroke-width="1.75" stroke="rgb(188, 189, 34)"/><g class="markmap-highlight"/><g data-depth="3" data-path="1.2.3" class="markmap-node" transform="translate(551, -302.09375)"><line stroke="#2ca02c" stroke-width="1.375" x1="-1" x2="392" y1="20.6875" y2="20.6875"/><foreignObject class="markmap-foreign" x="8" y="0" style="opacity: 1;" width="374" height="20"><div xmlns="http://www.w3.org/1999/xhtml"><div xmlns="http://www.w3.org/1999/xhtml">Explain what a <strong>table</strong> is and why databases use them</div></div></foreignObject></g><g data-depth="3" data-path="1.2.4" class="markmap-node" transform="translate(551, -275.71875)"><line stroke="#d62728" stroke-width="1.375" x1="-1" x2="390" y1="20.6875" y2="20.6875"/><foreignObject class="markmap-foreign" x="8" y="0" style="opacity: 1;" width="372" height="20"><div xmlns="http://www.w3.org/1999/xhtml"><div xmlns="http://www.w3.org/1999/xhtml">Describe the difference between <strong>rows</strong> and <strong>columns</strong></div></div></foreignObject></g><g data-depth="3" data-path="1.2.5" class="markmap-node" transform="translate(551, -249.34375)"><line stroke="#9467bd" stroke-width="1.375" x1="-1" x2="354" y1="20.6875" y2="20.6875"/><foreignObject class="markmap-foreign" x="8" y="0" style="opacity: 1;" width="336" height="20"><div xmlns="http://www.w3.org/1999/xhtml"><div xmlns="http://www.w3.org/1999/xhtml">Understand what a <strong>record</strong> is in database terms</div></div></foreignObject></g><g data-depth="3" data-path="1.2.6" class="markmap-node" transform="translate(551, -222.96875)"><line stroke="#8c564b" stroke-width="1.375" x1="-1" x2="457" y1="20.6875" y2="20.6875"/><foreignObject class="markmap-foreign" x="8" y="0" style="opacity: 1;" width="439" height="20"><div xmlns="http://www.w3.org/1999/xhtml"><div xmlns="http://www.w3.org/1999/xhtml">Define a <strong>Primary Key</strong> and explain why every table needs one</div></div></foreignObject></g><g data-depth="3" data-path="1.2.7" class="markmap-node" transform="translate(551, -196.59375)"><line stroke="#17becf" stroke-width="1.375" x1="-1" x2="500" y1="20.6875" y2="20.6875"/><foreignObject class="markmap-foreign" x="8" y="0" style="opacity: 1;" width="482" height="20"><div xmlns="http://www.w3.org/1999/xhtml"><div xmlns="http://www.w3.org/1999/xhtml">Define a <strong>Foreign Key</strong> and explain how tables connect to each other</div></div></foreignObject></g><g data-depth="3" data-path="1.2.8" class="markmap-node" transform="translate(551, -170.21875)"><line stroke="#1f77b4" stroke-width="1.375" x1="-1" x2="511" y1="20.6875" y2="20.6875"/><foreignObject class="markmap-foreign" x="8" y="0" style="opacity: 1;" width="493" height="20"><div xmlns="http://www.w3.org/1999/xhtml"><div xmlns="http://www.w3.org/1999/xhtml">Identify <strong>One-to-One</strong>, <strong>One-to-Many</strong>, and <strong>Many-to-Many</strong> relationships</div></div></foreignObject></g><g data-depth="3" data-path="1.2.9" class="markmap-node" transform="translate(551, -143.84375)"><line stroke="#ff7f0e" stroke-width="1.375" x1="-1" x2="558" y1="20.6875" y2="20.6875"/><foreignObject class="markmap-foreign" x="8" y="0" style="opacity: 1;" width="540" height="20"><div xmlns="http://www.w3.org/1999/xhtml"><div xmlns="http://www.w3.org/1999/xhtml">Read a simple database schema and understand how the pieces fit together</div></div></foreignObject></g><g data-depth="3" data-path="1.2.10" class="markmap-node" transform="translate(551, -117.46875)"><line stroke="#7f7f7f" stroke-width="1.375" x1="-1" x2="460" y1="20.6875" y2="20.6875"/><foreignObject class="markmap-foreign" x="8" y="0" style="opacity: 1;" width="442" height="20"><div xmlns="http://www.w3.org/1999/xhtml"><div xmlns="http://www.w3.org/1999/xhtml">Apply these concepts to the <strong>University Management System</strong></div></div></foreignObject></g><g data-depth="3" data-path="1.2.11" class="markmap-node" transform="translate(551, -91.09375)"><line stroke="#bcbd22" stroke-width="1.375" x1="-1" x2="498" y1="20.6875" y2="20.6875"/><foreignObject class="markmap-foreign" x="8" y="0" style="opacity: 1;" width="480" height="20"><div xmlns="http://www.w3.org/1999/xhtml"><div xmlns="http://www.w3.org/1999/xhtml">Feel confident that you can design the basic structure of a database</div></div></foreignObject></g><g data-depth="2" data-path="1.2" class="markmap-node" transform="translate(287, -196.59375)"><line stroke="#ff7f0e" stroke-width="1.75" x1="-1" x2="186" y1="20.875" y2="20.875"/><circle stroke-width="1.5" r="6" stroke="#ff7f0e" fill="var(--markmap-circle-open-bg)" cx="184" cy="20.875"/><foreignObject class="markmap-foreign" x="8" y="0" style="opacity: 1;" width="168" height="20"><div xmlns="http://www.w3.org/1999/xhtml"><div xmlns="http://www.w3.org/1999/xhtml">🎯 Learning Objectives</div></div></foreignObject></g><g data-depth="4" data-path="1.12.13.14" class="markmap-node" transform="translate(614, -59.71875)"><line stroke="#17becf" stroke-width="1.1875" x1="-1" x2="231" y1="20.59375" y2="20.59375"/><foreignObject class="markmap-foreign" x="8" y="0" style="opacity: 1;" width="213" height="20"><div xmlns="http://www.w3.org/1999/xhtml"><div xmlns="http://www.w3.org/1999/xhtml">What columns should it have?</div></div></foreignObject></g><g data-depth="4" data-path="1.12.13.15" class="markmap-node" transform="translate(614, -33.53125)"><line stroke="#1f77b4" stroke-width="1.1875" x1="-1" x2="359" y1="22.59375" y2="22.59375"/><foreignObject class="markmap-foreign" x="8" y="0" style="opacity: 1;" width="341" height="22"><div xmlns="http://www.w3.org/1999/xhtml"><div xmlns="http://www.w3.org/1999/xhtml">How does it connect to the existing <code>users</code> table?</div></div></foreignObject></g><g data-depth="4" data-path="1.12.13.16" class="markmap-node" transform="translate(614, -5.34375)"><line stroke="#ff7f0e" stroke-width="1.1875" x1="-1" x2="316" y1="20.59375" y2="20.59375"/><foreignObject class="markmap-foreign" x="8" y="0" style="opacity: 1;" width="298" height="20"><div xmlns="http://www.w3.org/1999/xhtml"><div xmlns="http://www.w3.org/1999/xhtml">What if two users save the same product?</div></div></foreignObject></g><g data-depth="4" data-path="1.12.13.17" class="markmap-node" transform="translate(614, 20.84375)"><line stroke="#2ca02c" stroke-width="1.1875" x1="-1" x2="491" y1="20.59375" y2="20.59375"/><foreignObject class="markmap-foreign" x="8" y="0" style="opacity: 1;" width="473" height="20"><div xmlns="http://www.w3.org/1999/xhtml"><div xmlns="http://www.w3.org/1999/xhtml">How do we make sure every saved favorite belongs to a real user?</div></div></foreignObject></g><g data-depth="3" data-path="1.12.13" class="markmap-node" transform="translate(534, -9.4375)"><line stroke="#bcbd22" stroke-width="1.375" x1="-1" x2="2" y1="0.6875" y2="0.6875"/><circle stroke-width="1.5" r="6" stroke="#bcbd22" fill="var(--markmap-circle-open-bg)" cx="0" cy="0.6875"/><foreignObject class="markmap-foreign" x="8" y="0" style="opacity: 1;" width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml"><div xmlns="http://www.w3.org/1999/xhtml"></div></div></foreignObject></g><g data-depth="4" data-path="1.12.18.19" class="markmap-node" transform="translate(614, 52.03125)"><line stroke="#9467bd" stroke-width="1.1875" x1="-1" x2="152" y1="20.59375" y2="20.59375"/><foreignObject class="markmap-foreign" x="8" y="0" style="opacity: 1;" width="134" height="20"><div xmlns="http://www.w3.org/1999/xhtml"><div xmlns="http://www.w3.org/1999/xhtml">Every job interview</div></div></foreignObject></g><g data-depth="4" data-path="1.12.18.20" class="markmap-node" transform="translate(614, 78.21875)"><line stroke="#8c564b" stroke-width="1.1875" x1="-1" x2="260" y1="20.59375" y2="20.59375"/><foreignObject class="markmap-foreign" x="8" y="0" style="opacity: 1;" width="242" height="20"><div xmlns="http://www.w3.org/1999/xhtml"><div xmlns="http://www.w3.org/1999/xhtml">Every database design discussion</div></div></foreignObject></g><g data-depth="4" data-path="1.12.18.21" class="markmap-node" transform="translate(614, 104.40625)"><line stroke="#e377c2" stroke-width="1.1875" x1="-1" x2="221" y1="20.59375" y2="20.59375"/><foreignObject class="markmap-foreign" x="8" y="0" style="opacity: 1;" width="203" height="20"><div xmlns="http://www.w3.org/1999/xhtml"><div xmlns="http://www.w3.org/1999/xhtml">Every line of SQL you'll write</div></div></foreignObject></g><g data-depth="4" data-path="1.12.18.22" class="markmap-node" transform="translate(614, 130.59375)"><line stroke="#7f7f7f" stroke-width="1.1875" x1="-1" x2="228" y1="20.59375" y2="20.59375"/><foreignObject class="markmap-foreign" x="8" y="0" style="opacity: 1;" width="210" height="20"><div xmlns="http://www.w3.org/1999/xhtml"><div xmlns="http://www.w3.org/1999/xhtml">Every system you'll ever build</div></div></foreignObject></g><g data-depth="3" data-path="1.12.18" class="markmap-node" transform="translate(534, 101.3125)"><line stroke="#d62728" stroke-width="1.375" x1="-1" x2="2" y1="0.6875" y2="0.6875"/><circle stroke-width="1.5" r="6" stroke="#d62728" fill="var(--markmap-circle-open-bg)" cx="0" cy="0.6875"/><foreignObject class="markmap-foreign" x="8" y="0" style="opacity: 1;" width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml"><div xmlns="http://www.w3.org/1999/xhtml"></div></div></foreignObject></g><g data-depth="2" data-path="1.12" class="markmap-node" transform="translate(287, 35.9375)"><line stroke="#7f7f7f" stroke-width="1.75" x1="-1" x2="169" y1="20.875" y2="20.875"/><circle stroke-width="1.5" r="6" stroke="#7f7f7f" fill="var(--markmap-circle-open-bg)" cx="167" cy="20.875"/><foreignObject class="markmap-foreign" x="8" y="0" style="opacity: 1;" width="151" height="20"><div xmlns="http://www.w3.org/1999/xhtml"><div xmlns="http://www.w3.org/1999/xhtml">🧠 Why This Matters</div></div></foreignObject></g><g data-depth="4" data-path="1.23.24.25" class="markmap-node" transform="translate(938, 109.40625)"><line stroke="#1f77b4" stroke-width="1.1875" x1="-1" x2="463" y1="128.59375" y2="128.59375"/><foreignObject class="markmap-foreign" x="8" y="0" style="opacity: 1;" width="445" height="128"><div xmlns="http://www.w3.org/1999/xhtml"><div xmlns="http://www.w3.org/1999/xhtml"><table data-lines="72,79">
<thead data-lines="72,73">
<tr data-lines="72,73">
<th>Table</th>
<th>What It Stores</th>
</tr>
</thead>
<tbody data-lines="74,79">
<tr data-lines="74,75">
<td><code>students</code></td>
<td>Information about each student</td>
</tr>
<tr data-lines="75,76">
<td><code>instructors</code></td>
<td>Information about each professor</td>
</tr>
<tr data-lines="76,77">
<td><code>departments</code></td>
<td>Information about each academic department</td>
</tr>
<tr data-lines="77,78">
<td><code>courses</code></td>
<td>Information about each course offered</td>
</tr>
<tr data-lines="78,79">
<td><code>enrollments</code></td>
<td>Information about which student is in which course</td>
</tr>
</tbody>
</table></div></div></foreignObject></g><g data-depth="3" data-path="1.23.24" class="markmap-node" transform="translate(534, 163.40625)"><line stroke="#17becf" stroke-width="1.375" x1="-1" x2="326" y1="20.6875" y2="20.6875"/><circle stroke-width="1.5" r="6" stroke="#17becf" fill="var(--markmap-circle-open-bg)" cx="324" cy="20.6875"/><foreignObject class="markmap-foreign" x="8" y="0" style="opacity: 1;" width="308" height="20"><div xmlns="http://www.w3.org/1999/xhtml"><div xmlns="http://www.w3.org/1999/xhtml">The University Management System Tables</div></div></foreignObject></g><g data-depth="3" data-path="1.23.26" class="markmap-node" transform="translate(534, 189.78125)"><line stroke="#ff7f0e" stroke-width="1.375" x1="-1" x2="235" y1="20.6875" y2="20.6875"/><foreignObject class="markmap-foreign" x="8" y="0" style="opacity: 1;" width="217" height="20"><div xmlns="http://www.w3.org/1999/xhtml"><div xmlns="http://www.w3.org/1999/xhtml">What Does a Table Look Like?</div></div></foreignObject></g><g data-depth="2" data-path="1.23" class="markmap-node" transform="translate(287, 176.59375)"><line stroke="#bcbd22" stroke-width="1.75" x1="-1" x2="169" y1="20.875" y2="20.875"/><circle stroke-width="1.5" r="6" stroke="#bcbd22" fill="var(--markmap-circle-open-bg)" cx="167" cy="20.875"/><foreignObject class="markmap-foreign" x="8" y="0" style="opacity: 1;" width="151" height="20"><div xmlns="http://www.w3.org/1999/xhtml"><div xmlns="http://www.w3.org/1999/xhtml">📖 What are Tables?</div></div></foreignObject></g><g data-depth="1" data-path="1" class="markmap-node" transform="translate(0, -10)"><line stroke="#1f77b4" stroke-width="2.5" x1="-1" x2="209" y1="21.25" y2="21.25"/><circle stroke-width="1.5" r="6" stroke="#1f77b4" fill="var(--markmap-circle-open-bg)" cx="207" cy="21.25"/><foreignObject class="markmap-foreign" x="8" y="0" style="opacity: 1;" width="191" height="20"><div xmlns="http://www.w3.org/1999/xhtml"><div xmlns="http://www.w3.org/1999/xhtml">🧱 Database Fundamentals</div></div></foreignObject></g></g></svg>
