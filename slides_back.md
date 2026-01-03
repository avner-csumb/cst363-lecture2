---
# try also 'default' to start simple
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
# some information about your slides (markdown enabled)
title: Welcome to Slidev
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# apply UnoCSS classes to the current slide
class: text-center
drawings:
  persist: false
mdc: true
---

# Relational Algebra, Data Definition Language, Data Manipulation Language

CST 363

---

## Database schema

<div class="p-2">

- A schema is the design or structure of a specific database.
- An instance is a schema "instantiated" with data

</div>

<div class="grid grid-cols-2 gap-8">

<v-click>
  <div>

Schema:
```
table roster {
   class: string,
   student: string
}
```

  </div>

  </v-click>

<v-click>


  <div>

Instance:
![](/images/student.png){class="w-50"}


  </div>
  </v-click>


</div>

---


## Database queries

<div class="p-2">


- A query is a statement requesting the retrieval of information from a database.
</div>

Example SQL queries:
```sql
SELECT *
FROM customers
WHERE country = 'Guam';

SELECT *
FROM customers
WHERE city = 'Berlin' OR city = 'Munich';
```

---

## Transactions

<div class="p-2">

<v-clicks>

- A transaction is the unit of change in a database. 
- Transactions contain a set of database operations.
- A transaction must suceed, or fail "completely".
- For example, a transaction might be used for  moving a student from one class to another.

</v-clicks>

</div>

<v-click>

<small>*more on transactions later in semester</small>

</v-click>

---

## A Relational DB is a group of tables

<div class="p-2">


- A column is also called an **attribute**
- Each attribute has a **domain**, which is a set of values that are allowed in the column.
- A row is also called a **tuple**.


<small>instructor</small>
![](/images/instructor.png){class="w-100"}

</div>



---

## Tuples

<div class="p-2">


Another table from the "courses" DB

- A tuple represents a relationship between the values of the tuple.
- A table represents a mathematical relation.
  - So a table is also called a "relation". This is why we say "relational database".

<small>course</small>
![](/images/course.png){class="w-100"}

</div>


---

## Null values

<div class="p-2">

- Sometimes we don’t know a value, or the value doesn’t exist.
- In relational DBs we deal with this by using special null values.

<small>instructor</small>
![](/images/instructor2.png){class="w-100"}


</div>


---

## Relation schema and instances

<div class="p-2">


- relation schema: Gives the names and domains of the attributes (domains not shown here.)
`student(student_id, student_name, dept_name, tot_cred)`

- relation instance: an instance of a relation schema (also called a “table”)


<small>student</small>
![](/images/student2.png){class="w-100"}

</div>


---

## Database schema and instances

<div class="p-2">

**database schema:** the relation schemas for all the relations in a database


```
student(student_id, student_name, dept_name, tot_cred)
department(dept_name, building, budget)
...
```

</div>

---

## Database schema and instances (cont.)

<div class="p-2">

**database instance:** all the tables of the database


<small>student</small>
![](/images/student3.png){class="w-90"}



<small>department</small>
![](/images/dept.png){class="w-70"}




</div>


---

## All the names

<div class="p-2">


In databases there seem to be two or three ways to say everything.

- **schema** = relation schema
- **table** = relation = relation instance = instance
- tuple = **row**
- **attribute** = column

</div>

<br><br>

<div class="text-center">We'll normally use the terms in bold</div>


---

## "Functions" on tables

<div class="p-2">

<v-clicks depth="2">

- In programming we define objects, and methods on those objects.
- For example, for strings we have operations like
  - `substring(string, first, last)`
  - `concat(string1, string2)`
- In relational databases the objects are tables, and we define functions (operations)  on tables.

</v-clicks>

</div>


---


## “Selection” operator: Pick out rows of a table

<div class="p-2">

<v-click>

<small>patient</small>
![](/images/patient.png){class="w-90"}

</v-click>

<v-click>

Sex = F

</v-click>


<v-click>

![](/images/patient2.png){class="w-90"}

</v-click>



</div>


---

## “Projection” operator: Pick out columns of a table

The operation takes a table as input and produces a table as output

![](/images/patient3.png){class="w-90"}

“Patient No.”, “First name”

![](/images/patient4.png){class="w-60"}




---

## “Union” operator: Add the rows of two tables



![](/images/union.png){class="w-160"}


---

## Question about “projection” operator 


![](/images/patient5.png){class="w-120"}


What happens if we perform a projection on this table using attribute ‘Last name’?


![](/images/lastname.png){class="w-30"}

Is this a problem?



---

## Removing duplicate rows

- As part of applying an operation of relation algebra, duplicate rows should be removed.
- Duplicate rows aren’t technically allowed in a valid table.
- In practice a DB system may allow duplicate rows.


---

## Defining the operations

`selection(table, condition)`  
- return the rows of table that meet the condition
- example: t = selection(student_name, tot_cred > 100)

`projection(table, attributes)`
- returns the specified columns of table
- example: t = projection(student, ["student_name", "student_id"])

`union(table1, table2)`
- returns the table containing the rows of table1 and table2
- example: t = union(student1, student2)


---

## Answering questions with the functions

What are the names of students with more than 100 credits?


To get the answer:
select the rows of the ‘student’ table with tot_cred > 100
get the "name" attribute



`projection(selection(student, tot_cred > 100), ["student_name"])`



---

## Another example

What courses are offered in Spring ‘09, and what buildings are they in?

![](/images/section.png){class="w-150"}

```
t = selection(section, semester="Spring" and year=2009)
projection(t, ["course_id", "building"])
```


---

## Exercise

Write the operations to get the IDs of students who took course CS-101 in Fall 2009.

![](/images/takes.png){class="w-120"}


```
projection(selection(takes, course_id="CS-101"
  and semester="Fall" and year=2009), ["ID"])
```

---

## A ‘union’ example

Get the ID of courses taught in Fall 2009 or Fall 2010

<small>section</small>
![](/images/section2.png){class="w-120"}


```
t1 = selection(section, semester="Fall" and year=2009)
t2 = selection(section, semester="Spring" and year=2010)
projection(union(t1,t2), ["course_ID"])
```

Last line could alternatively be written:

```
union(projection(t1,["course_id"]), projection(t2,["course_id"])
```

---

## Cartesian Product


![](/images/cartesian.png){class="w-100"}


Question: 
If you take the product of a table with 5 rows and a table with 8 rows, how many rows do you get? 


---

## Using cross product

<div class="p-2">


What are the names of CS instructors and what classes do they teach?


<div class="grid grid-cols-2 gap-4">

<div>

![](/images/instructor.png){class="w-100"}

</div>

<div>

![](/images/teaches.png){class="w-80"}

</div>

</div>

```
t1 = cross(instructor, teaches)
t2 = selection(t1, instructor.ID = teaches.ID and dept_name = "Comp. Sci.")
projection(t2, ["name", "course_id"])
```

</div>

---

## Saying it in plain Greek

<div class="p-2">


To make it look more like math, you can write the operations using Greek letters and symbols (as in the "Relational Algebra" handout)


selection(section, year=2015)

$\sigma_\text{year=2015} (\text{section})$  	

sigma – select starts with ‘s’


projection(section, [“course_id”])



$\pi_\text{year=2015} (\text{section})$  	


pi – project starts with ‘p’

</div>

---

## What is SQL?

<div class="p-2">


- SQL = “Structured Query Language”
- Originally developed at IBM in the early 70’s
- Pronounced “sequel” or S-Q-L 
- A special-purpose language that lets you express questions about your relational data in a precise way


SQL serves as the interface between the database and users/applications

</div>

---


## Parts of SQL

<div class="p-2">


SQL is not just for writing queries.

It has several parts, including:

<v-clicks depth="2">


- Data-definition language (DDL)
  - create/delete/modify schemas
  - define integrity constraints
  - define views
  - drop tables

- Data-manipulation language (DML)
  - define queries
  - modify tables (insert/delete/modify tuples)

</v-clicks>

</div>


---

## Defining a relation schema

*Question*: What information do you provide when you define a relation schema?

<div class="p-2">


- name of the table
- names and domains of each attribute
- primary key
- optionally:
  - whether an attribute is allowed to have a null value
  - foreign keys

</div>

Question: what is a foreign key?

---

## SQL for defining a relation schema


```sql
CREATE TABLE department (
   dept_name VARCHAR(20) PRIMARY KEY,
   building VARCHAR(15),
   budget NUMERIC(12, 2)
);
```

For each attribute we give the attribute name and type.

We also give the primary key.

---

## Another ‘create table’ example

```sql
CREATE TABLE student (
   ID VARCHAR(20) PRIMARY KEY,
   name VARCHAR(20) NOT NULL,
   dept_name VARCHAR(20), 
   tot_cred NUMERIC(3, 0),
   FOREIGN KEY (dept_name) REFERENCES department(dept_name)
);
```

The foreign key constraint says:
- every `dept_name` value in the student table must equal the key of some row in the department table.

The not-null constraint says:
- no name value in the student table can be null

---

## Attribute types

Recall that attributes can only be simple values, like integers, and strings.


Here are some of the main types allowed by SQL:

`VARCHAR(n)`: a variable-length string of length at most n

`INT`: an integer – ‘integer’ is also okay

`NUMERIC(p, d)`: a fixed-point number of p total digits, with d digits to right of the decimal point.  
	
Example: 123.45  `isnumeric(5,2)`

`FLOAT(p)`: a floating-point number with either 4 byte or 8 byte precision 


---


## Yet another example

```sql
CREATE TABLE takes (
   student_id VARCHAR(5),
   course_id VARCHAR(8),
   section_id VARCHAR(8),
   semester VARCHAR(6),
   section_year NUMERIC(4, 0),
   grade VARCHAR(2),
   PRIMARY KEY (student_id, course_id, section_id, semester, section_year),
   FOREIGN KEY (course_id, section_id, semester, section_year)
       REFERENCES section(course_id, section_id, semester, section_year) ON DELETE CASCADE,
   FOREIGN KEY (student_id)
       REFERENCES student(student_id) ON DELETE CASCADE
);
```

- primary key has multiple attributes
- two foreign keys



---

## Cascaded deletes

<div class="grid grid-cols-2 gap-2">

<div>

![](/images/instructor3.png){class="w-90"}

</div>

<div>

![](/images/teaches.png){class="w-90"}

</div>
</div>

ID in the **teaches** table is a foreign key that "points to" the ID field of the **instructor** table.


What to do when the first row is deleted from the instructor table?

---

## Inserting rows into a table

![](/images/patient6.png){class="w-140"}


```sql
CREATE TABLE patient (
   patient_no INTEGER PRIMARY KEY,
   last_name VARCHAR(64) NOT NULL,
   first_name VARCHAR(64) NOT NULL,
   sex VARCHAR(1),
   date_of_birth DATE,
   ward INTEGER
);
```


---

## Inserting rows into a table (cont.)

<div class="p-2">


SQL also has statements to delete and modify rows, as well as batch operations.

```sql
CREATE TABLE patient (
   patient_no INTEGER PRIMARY KEY,
   last_name VARCHAR(64) NOT NULL,
   first_name VARCHAR(64) NOT NULL,
   sex VARCHAR(1),
   date_of_birth DATE,
   ward INTEGER
);
```

<br>

```sql
INSERT INTO patient VALUES (454, 'Smith', 'John', 'M', '1978-08-14', 6);
INSERT INTO patient VALUES (223, 'Jones', 'Peter', 'M', '1985-12-07', 8);
INSERT INTO patient VALUES (597, 'Brown', 'Brenda', 'F', '1961-06-17', 3);
INSERT INTO patient VALUES (234, 'Jenkins', 'Alan', 'M', '1972-01-29', 7);
INSERT INTO patient VALUES (244, 'Wells', 'Chris', 'F', '1995-02-25', 6);
```

</div>

---

## Deleting or Modifying a table

<div class="p-2">


```sql
DROP TABLE IF EXISTS instructor;`
```

- delete the instructor schema and its tuples


<br>


```sql
ALTER TABLE instructor ADD office VARCHAR(5);
```

- adds attribute ‘office’ to the instructor schema
- value of office is initialized to ‘null’



</div>

---

## Summary

<div class="p-2">

<v-clicks>

- `CREATE TABLE` — define a relation schema
- `INSERT` — insert rows into a relation instance
- `DROP TABLE` — delete relation schemas
- `ALTER TABLE` — modify relation schemas

</v-clicks>

</div>

---

## Agenda
- Relational model fundamentals
- Schema vs. instance
- Queries and transactions (conceptual)
- Relational algebra (RA): core operators
- Composing RA expressions (query building)
- Duplicate semantics (set vs. bag)
- Intro SQL: DDL vs. DML
- `CREATE TABLE`, constraints, and data types
- Basic data changes: `INSERT`, `ALTER`, `DROP`

---

## Learning Outcomes
By the end of this week, students should be able to:
- Use relational terminology precisely (relation/tuple/attribute/domain)
- Distinguish schema vs. instance (relation and database levels)
- Write basic RA expressions (σ, π, ∪, ×) and compose them
- Explain duplicate behavior in RA vs. SQL
- Recognize SQL’s roles (DDL and DML) and write simple DDL/DML statements

---

## Relational Model: Core Vocabulary
- **Relation**: table  
- **Tuple**: row (a single record)  
- **Attribute**: column  
- **Domain**: allowed values for an attribute (type + constraints)

Example schema:
- `student(student_id, student_name, dept_name, tot_cred)`

---

## Schema vs. Instance
- **Relation schema**: table definition (attributes + domains)
- **Relation instance**: table contents at a specific point in time

- **Database schema**: collection of all relation schemas
- **Database instance**: collection of all relation instances (current data)

Key takeaway: **Schema = design; instance = data right now.**

---

## Queries and Transactions (Conceptual)
- **Query**: asks for information from the database  
  - Example: “Which students have more than 100 credits?”
- **Transaction**: a sequence of operations treated as a single unit  
  - All succeed or all fail (atomic “all-or-nothing”)

---

## Relational Algebra (RA): Why We Use It
- RA is a formal, mathematical way to express queries
- **Operations take relations as input and return relations as output**
  - Closure property enables composition: build complex queries from simple parts
- RA supports reasoning about query execution and optimization later

---

## RA Operator: Selection (σ)
**Selection filters rows** using a predicate (condition)

- Notation: `σ condition (Relation)`
- Example (conceptually):
  - Students with > 100 credits:
  - `σ tot_cred > 100 (student)`

SQL intuition: similar to `WHERE`

---

## RA Operator: Projection (π)
**Projection selects columns** (attributes)

- Notation: `π attr1, attr2, ... (Relation)`
- Example:
  - Names of students:
  - `π student_name (student)`

Important: Projection can introduce duplicates (RA uses set semantics—duplicates removed).

---

## RA Operator: Union (∪)
**Union combines rows** from two compatible relations

- Compatibility requirement:
  - Same number of attributes
  - Corresponding domains match
- Example concept:
  - “All students who are either CS majors or Math majors” (if represented as relations)

SQL intuition: similar to `UNION` (distinct by default)

---

## RA Operator: Cartesian Product (×)
**Cartesian product pairs every row of R with every row of S**

- Notation: `R × S`
- Row count multiplies: `|R × S| = |R| * |S|`
- Often used as a building block for joins:
  - Use `×` then apply selection to match keys (join condition)

SQL intuition: similar to `CROSS JOIN`

---

## Composing RA Expressions (Example)
Question: “Find names of students with more than 100 credits.”

1) Select qualifying students:  
- `σ tot_cred > 100 (student)`

2) Project just the names:  
- `π student_name (σ tot_cred > 100 (student))`

Composition pattern: **filter → then project**

---

## Join Pattern in RA (Cross Product + Selection)
To combine related tables:

1) Start with product:  
- `student × department`

2) Match keys (join condition):  
- `σ student.dept_name = department.dept_name (student × department)`

3) Project the columns you want:  
- `π student_name, department.building ( ... )`

This is the conceptual basis for joins.

---

## Duplicates: RA vs. SQL
- **Relational Algebra** uses *set semantics*
  - Relations are sets → duplicates are removed
  - Projection returns a set of tuples
- **SQL** often uses *bag (multiset) semantics*
  - Duplicates may appear unless eliminated
  - Use `DISTINCT` to remove duplicates

---

## SQL Overview (Big Picture)
SQL is the standard language to:
- Define database structure (schemas, constraints)
- Query data
- Modify data
- Manage related objects (e.g., views, permissions in full systems)

Two key sublanguages this week:
- **DDL**: Data Definition Language
- **DML**: Data Manipulation Language

---

## DDL vs. DML
**DDL (Definition)**
- `CREATE TABLE`, `ALTER TABLE`, `DROP TABLE`
- Defines schemas and constraints

**DML (Manipulation)**
- `SELECT`, `INSERT` (and later `UPDATE`, `DELETE`)
- Queries and changes actual data

---

## `CREATE TABLE`: What to Specify
A table definition typically includes:
- Table name
- Attributes + data types (domains)
- Constraints:
  - **PRIMARY KEY** (uniqueness + identification)
  - **FOREIGN KEY** (referential integrity)
  - **NOT NULL** (required attribute)
  - Optional actions: `ON DELETE CASCADE`, etc.

---

## Constraints: Primary Key & Foreign Key
- **Primary Key (PK)**: uniquely identifies each tuple  
  - Single-attribute or composite
- **Foreign Key (FK)**: ensures references are valid  
  - Values must exist in the referenced key (unless NULL allowed)

Why it matters:
- Prevents “dangling references”
- Enforces correctness of relationships

---

## Example DDL (Illustrative)
```sql
CREATE TABLE department (
  dept_name VARCHAR(20) PRIMARY KEY,
  building  VARCHAR(15),
  budget    NUMERIC(12,2)
);

CREATE TABLE student (
  student_id   INT PRIMARY KEY,
  student_name VARCHAR(50) NOT NULL,
  dept_name    VARCHAR(20),
  tot_cred     INT,
  FOREIGN KEY (dept_name) REFERENCES department(dept_name)
);
```

---

## Common SQL Data Types (Intro)
- `INT` — integers
- `VARCHAR(n)` — variable-length strings
- `NUMERIC(p,d)` — fixed precision/scale (good for money)
- `FLOAT(p)` — approximate numeric
- `DATE` — date values

Design principle: choose types that match required range and precision.

---

## Basic DML: `INSERT`
Add new tuples (rows) to a table instance:

```sql
INSERT INTO student (student_id, student_name, dept_name, tot_cred)
VALUES (1001, 'Ana Rivera', 'CS', 45);
```

Constraints apply at insert time:
- PK uniqueness
- FK must reference existing department (unless NULL allowed)
- NOT NULL must be satisfied

---

## Schema Change: `ALTER TABLE` and `DROP TABLE`
- `ALTER TABLE` updates the schema (e.g., add column)
  - New column values for existing rows become `NULL` unless specified
- `DROP TABLE` removes the table definition and its data

```sql
ALTER TABLE student ADD email VARCHAR(80);

DROP TABLE student;
```

---

## Practice (In-Class / Homework)
1) Write RA:
- “IDs of students taking CS-101 in Fall 2009”
  - Hint: selection on `takes`, then projection of IDs

2) Translate RA to SQL:
- `π name, surface (tournaments)`
  - What is the equivalent `SELECT ... FROM ...`?

3) DDL design:
- Define a table with PK + FK + NOT NULL
- Pick appropriate numeric type for currency-like values

---

## Summary
- Relational model: relations, tuples, attributes, domains
- Schema vs. instance (relation and database)
- Relational algebra: σ, π, ∪, ×; composition and join patterns
- Set vs. bag duplicates: RA vs SQL
- SQL foundations:
  - DDL: create/alter/drop + constraints
  - DML: select/insert (more later)

---

## Next Week Preview
- SQL querying in depth:
  - `SELECT-FROM-WHERE`
  - joins, aggregation, grouping, ordering
- More practice translating between RA and SQL


