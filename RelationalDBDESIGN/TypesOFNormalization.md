
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

# 2NF (Second Normal Form)

---

## **Definition:**

A table is in **Second Normal Form (2NF)** if:

1. It is already in **1NF** (no repeating groups, atomic values).
2. **All non-key columns must fully depend on the entire primary key**, not just part of it.

✅ In simple words: **No partial dependency** of a non-key attribute on part of a composite primary key.

---

## **Why 2NF Exists?**

* In 1NF, sometimes a table has a **composite primary key** (more than one column).
* Some attributes might depend **only on part of the key**, causing **redundancy and update anomalies**.
* 2NF **removes partial dependency** by splitting the table into multiple tables.

---

## **Example of Partial Dependency (Problem)**

Suppose we have an **Orders Table**:

| OrderID | BookID | BookTitle     | Quantity | Price |
| ------- | ------ | ------------- | -------- | ----- |
| 101     | B01    | Python Basics | 2        | 500   |
| 101     | B02    | Java Basics   | 1        | 600   |
| 102     | B01    | Python Basics | 1        | 500   |

**Primary Key:** `(OrderID, BookID)`

**Problem (Partial Dependency):**

* `Quantity` → depends on `(OrderID, BookID)` → ✅ Good
* `BookTitle` → depends **only on BookID** → ❌ Partial dependency
* `Price` → depends **only on BookID** → ❌ Partial dependency

**Why is this a problem?**

* Redundancy: Python Basics and Price 500 repeated in multiple rows.
* Update anomaly: If book title changes, must update multiple rows.
* Insert anomaly: Cannot add a new book without an order.

---

## **Solution (2NF) – Split Table**

### **1. Orders Table** (Details about order items)

| OrderID | BookID | Quantity |
| ------- | ------ | -------- |
| 101     | B01    | 2        |
| 101     | B02    | 1        |
| 102     | B01    | 1        |

**Primary Key:** `(OrderID, BookID)`

* `Quantity` depends on both → ✅ OK

### **2. Books Table** (Details about books)

| BookID | BookTitle     | Price |
| ------ | ------------- | ----- |
| B01    | Python Basics | 500   |
| B02    | Java Basics   | 600   |

**Primary Key:** `BookID`

* `BookTitle` and `Price` depend **only on BookID** → ✅ OK

---

## **Key Points About 2NF**

1. Must be in **1NF first**.
2. Remove **partial dependency** (columns depending only on part of a composite key).
3. Helps avoid:

   * **Redundancy** (repeating data)
   * **Update anomalies** (must update same info multiple times)
   * **Insert anomalies** (cannot add new data without unrelated info)
4. Usually involves **splitting tables**.

---

### **Quick Memory Tip (2NF)**

* Think: **“All non-key data must depend on the whole key, not part of it.”**
* If a column depends on **only part of a composite key → split it**.



---

# **3NF (Third Normal Form) – Easy Version**

---

## **Definition in Simple Words:**

A table is in **3NF** if:

1. It is already in **2NF**.
2. **No non-key column depends on another non-key column**.

✅ In other words:

* Every piece of data should depend **directly on the primary key**, not on another column.
* This avoids **repetition and mistakes**.

---

## **Example**

Imagine a table of **books and publishers**:

| BookID | BookTitle     | Price | Publisher | PublisherAddress |
| ------ | ------------- | ----- | --------- | ---------------- |
| B01    | Python Basics | 500   | ABC Pub   | Delhi            |
| B02    | Java Basics   | 600   | XYZ Pub   | Mumbai           |

**Primary Key:** `BookID`

**Problem (Transitive Dependency):**

* `PublisherAddress` depends on `Publisher`, not on `BookID` → ❌ This is a transitive dependency.

---

## **Solution (3NF)**

**Step 1: Split publisher info into its own table**

### **Books Table**

| BookID | BookTitle     | Price | PublisherID |
| ------ | ------------- | ----- | ----------- |
| B01    | Python Basics | 500   | P01         |
| B02    | Java Basics   | 600   | P02         |

**Step 2: Create Publisher Table**

| PublisherID | Publisher | PublisherAddress |
| ----------- | --------- | ---------------- |
| P01         | ABC Pub   | Delhi            |
| P02         | XYZ Pub   | Mumbai           |

✅ Now:

* Every column depends **directly on the primary key**.
* No column depends on another non-key column → 3NF achieved.

---

## **Key Points (Easy Words)**

* 3NF = **no “column depends on another column”**.
* Helps **reduce repetition**, **easier updates**, and **prevents mistakes**.
* Usually done by **splitting tables** logically.

---

### **Super Simple Memory Trick:**

* 1NF → atomic values
* 2NF → all columns depend on **whole key**
* 3NF → all columns depend on **only the key, not another column**

---









