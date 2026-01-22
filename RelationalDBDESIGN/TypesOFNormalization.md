
---

# **Database Normalization: Easy Guide**

## **What is Normalization?**

**Definition:**
Normalization is a systematic way to organize data in a database to **reduce redundancy** (duplicate data) and **prevent inconsistencies**.

**Purpose:**

* Avoid duplicate data.
* Prevent inconsistent data (e.g., “Jon” in one place, “John” in another).
* Eliminate update anomalies (problems during insert, update, or delete operations).

**Anomaly:**
An unintended problem in a database caused by poor design. Types:

1. **Insert anomaly** – Cannot add data without adding unrelated info. (student+course for adding a new course you also have to provide a student name , solution is split the table to easily insert a new course)
2. **Update anomaly** – Changing one piece of data requires multiple updates. (If the same info is stored in multiple places, you must update all of them. so split the table and reuse it)
3. **Delete anomaly** – Deleting something deletes important info by mistake (deleting a couse can delete a student also if Student+Course)

---

# **Normalization Types (Simplified)**

| Normal Form                   | What it fixes                                                                |
| ----------------------------- | ---------------------------------------------------------------------------- |
| 1NF (First Normal Form)       | Eliminates **repeating groups** and ensures **atomic values**.               |
| 2NF (Second Normal Form)      | Eliminates **partial dependency** (depends only on part of a composite key). |
| 3NF (Third Normal Form)       | Eliminates **transitive dependency** (non-key depends on another non-key).   |
| BCNF (Boyce-Codd Normal Form) | Fixes special cases where 3NF still has anomalies.                           |

---

# **1NF (First Normal Form)**

### **Definition:**

A table is in **1NF** if:

1. Each column contains **atomic values** (single value per cell).
2. Each column stores **same type of data**.
3. Each row has a **unique identifier** (primary key)(use the composite key).
4. No **repeating groups** of columns.

---

### **Why 1NF?**

**Problems without 1NF:**

* Hard to search for specific values.
* Difficult to update (risk of mistakes).
* Data inconsistency (e.g., Physics vs physics).
* Constraints (like unique, foreign key) cannot be enforced.
* Indexing fails → slower queries.
* Insert, update, delete anomalies appear.

---

### **Example of Unnormalized Table**

| StudentID | StudentName | Courses       | PhoneNumbers |
| --------- | ----------- | ------------- | ------------ |
| 1         | Alice       | Math, Physics | 98765, 91234 |
| 2         | Bob         | Chemistry     | 99887        |

**Problems:**

* **Courses** column has multiple values → not atomic.
* **PhoneNumbers** column has multiple values → not atomic.
* Difficult to search, update, and maintain.

---

### **Solution: Make Data Atomic**

Break data into separate rows:

| StudentID | Course    | PhoneNumber |
| --------- | --------- | ----------- |
| 1         | Math      | 98765       |
| 1         | Physics   | 91234       |
| 2         | Chemistry | 99887       |

**Primary Key:** `(StudentID, Course, PhoneNumber)` → ensures **unique rows**.

---

### **Same Column Should Store Same Type of Data**

* Each column = **one type of fact**
* Example:

**Bad design:**

| ProductID | PriceInfo     |
| --------- | ------------- |
| 1         | 100           |
| 2         | 100 (10% off) |

**Better design:**

| ProductID | Price | Discount |
| --------- | ----- | -------- |
| 1         | 100   | NULL     |
| 2         | 100   | 10%      |

**Explanation:**

* Column should be predictable.
* Helps database enforce rules and indexing.
* Makes updates and queries easier.

---

### **No Repeating Groups**

* Avoid having columns like `Phone1, Phone2, Phone3`.
* Create a separate table with a foreign key:

| StudentID | PhoneNumber |
| --------- | ----------- |
| 1         | 98765       |
| 1         | 91234       |
| 2         | 99887       |

**Why?**

* Easier to delete or update numbers.
* Keeps data organized and scalable.

---

### ✅ **Key Takeaways for 1NF**

1. Columns must have **atomic values**.
2. Columns must contain **same type of data**.
3. Each table must have a **primary key**.
4. Avoid **repeating groups**.
5. Makes **search, update, delete operations reliable and fast**.

---

This version is **interview-ready** because you can:

* Explain why normalization exists in simple words.
* Show examples with problems and solutions.
* Mention primary keys and atomicity clearly.
* Link everything to **real-life operations** (search, update, delete).

---

