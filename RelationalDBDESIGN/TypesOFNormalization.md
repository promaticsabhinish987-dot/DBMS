# Normalization Types

1. 1NF
2. 2NF
3. 3NF
4. BCNF

## What is Normalization and what problem does it solve?

Defination :- Normalization is a process to organize our data in database to reduce **redundency**.

Normalization: A systematic process of organizing database data so that each fact is stored in the correct place with minimal redundancy(Unnecessary duplication of the same data.).

Problem it solves: It prevents data inconsistency(Same data having different values in different places.Name = “Jon” in one row, “John” in another.Unreliable data and wrong results.), redundancy, and update anomalies (insert, update, delete problems).

Anomaly: An unintended problem or inconsistency that occurs in a database due to poor data structure, especially during insert, update, or delete operations.


## 1NF

Unnormalized table. Break this table into multiple tables to avoid duplication of data is called Normalization.

| StudentID | StudentName | Courses       | PhoneNumbers |
| --------- | ----------- | ------------- | ------------ |
| 1         | Alice       | Math, Physics | 98765, 91234 |
| 2         | Bob         | Chemistry     | 99887        |


What is the problem with this table.

### requirements of the 1NF.

1. Each column must have atomic (single) values
 problem with current able :- Non-atomic values

eg:- 
Courses has multiple values ( Math, Physics)
PhoneNumbers has multiple values (98765, 91234)

Why to make it atomic.

1. Search :- If we search "Physics" its hard.
2. Update :- Removing or changing one value requires editing text, risking mistakes.
3. Inconsistent data :- Same value may appear in different formats (Physics, physics, Phy).
4. Constraints cannot be enforced → The database cannot apply rules (foreign keys, uniqueness) to individual values.
5. Indexing fails → Indexes work on single values, not lists, making queries slow.
6. Insert and delete anomalies appear → You may be forced to add or lose unrelated data.

Multiple values in one column confuse the database, making data hard to query, update, and keep correct.

Solution => make it atomic. break the data in different rows. 

eg:- 

| StudentID | Course    | PhoneNumber |
| --------- | --------- | ----------- |
| 1         | Math      | 98765       |
| 1         | Physics   | 91234       |
| 2         | Chemistry | 99887       |








