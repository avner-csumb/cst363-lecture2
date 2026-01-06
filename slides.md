---
# try also 'default' to start simple
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
# some information about your slides (markdown enabled)
title: Relational Algebra, DDL, DML
info: |
  ## CST 363
# apply UnoCSS classes to the current slide
class: text-center
drawings:
  persist: false
mdc: true
---

# Relational Algebra, DDL, DML

CST 363

---

## Database schema

<div class="p-4">

<v-clicks>

- A **schema** is the design or structure of a specific database.
- An **instance** is a schema "instantiated" with data

</v-clicks>



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


</div>

---


## Database queries

<div class="p-4">

<v-click>

- A query is a statement requesting the retrieval of information from a database.

</v-click>

<v-click>

Example SQL queries:

</v-click>

<v-click>

```sql
SELECT *
FROM customers
WHERE country = 'Guam';
```

</v-click>

<br>

<v-click>

```sql
SELECT *
FROM customers
WHERE city = 'Berlin' OR city = 'Munich';
```
</v-click>


</div>

---

## Transactions

<div class="p-4">

<v-clicks>

- A transaction is the *unit of change* in a database. 
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

<div class="p-4">


<v-clicks>

- A column is also called an **attribute**
- Each attribute has a **domain**, which is a set of values that are allowed in the column.
- A row is also called a **tuple**.

</v-clicks>

<v-click>

<small>instructor</small>
![](/images/instructor.png){class="w-100"}

</v-click>

</div>



---

## Tuples

<div class="p-2">

Another table from the `courses` DB

<v-clicks depth="2">

- A tuple represents a relationship between the values of the tuple.
- A table represents a mathematical relation.
  - So a table is also called a "relation". This is why we say "relational database".

</v-clicks>


<v-click>

<small>course</small>
![](/images/course.png){class="w-100"}

</v-click>

</div>


---

## Null values

<div class="p-4">

<v-clicks>

- Sometimes we don't know a value, or the value doesn't exist.
- In relational DBs we deal with this by using special `null` values.

</v-clicks>

<v-click>

<small>instructor</small>
![](/images/instructor2.png){class="w-100"}

</v-click>


</div>


---

## Relation schema and instances

<div class="p-4">


<v-clicks depth="2">

- **relation schema:** Gives the names and domains of the attributes (domains not shown here.)
  - `student(student_id, student_name, dept_name, tot_cred)`
- **relation instance:** an instance of a *relation schema* (also called a "table")

</v-clicks>

<v-click>

<small>student</small>
![](/images/student2.png){class="w-100"}

</v-click>


</div>


---

## Database schema and instances

<div class="p-4">

**database schema:** the relation schemas for all the relations in a database

<v-click>

```
student(student_id, student_name, dept_name, tot_cred)
department(dept_name, building, budget)
...
```

</v-click>

</div>

---

## Database schema and instances (cont.)

<div class="p-4">

**database instance:** all the tables of the database

<v-click>

<small>student</small>
![](/images/student3.png){class="w-100"}

</v-click>


<v-click>

<small>department</small>
![](/images/dept.png){class="w-70"}

</v-click>



</div>


---

## All the names

<div class="p-4">

In databases there seem to be two or three ways to say everything.

<v-clicks>

- **schema** = relation schema
- **table** = relation = relation instance = instance
- tuple = **row**
- **attribute** = column

</v-clicks>

</div>

<br>

<v-click>

<div class="text-center">We'll normally use the terms in bold</div>

</v-click>

---

## "Functions" on tables

<div class="p-4">

<v-clicks depth="2">

- In programming we define objects, and methods on those objects.
- For example, for strings we have operations like
  - `substring(string, first, last)`
  - `concat(string1, string2)`
- In relational databases the objects are tables, and we define functions (operations)  on tables.

</v-clicks>

</div>


---


## "Selection" operator: Pick out rows of a table

<div class="p-4">

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


<div class="p-4">

The operation takes a table as input and produces a table as output

<v-click>

![](/images/patient3.png){class="w-90"}

</v-click>

<v-click>

"Patient No.", "First name"

</v-click>

<v-click>


![](/images/patient4.png){class="w-60"}

</v-click>

</div>


---

## "Union" operator: Add the rows of two tables

<div class="p-4">

<v-click>

![](/images/union.png){class="w-160"}

</v-click>

</div>


---

## Question about "projection" operator 

<div class="p-4">


<v-click>

![](/images/patient5.png){class="w-120"}

</v-click>

<v-click>

What happens if we perform a `projection` on this table using attribute 'Last name'?


</v-click>

<v-click>

![](/images/lastname.png){class="w-30"}

</v-click>

<v-click>

**Is this a problem?**

</v-click>

</div>

---

## Removing duplicate rows

<div class="p-4">

<v-clicks>

- As part of applying an operation of relation algebra, duplicate rows should be removed.
- Duplicate rows aren't technically allowed in a valid table.
- SQL query results may include duplicates unless you use `DISTINCT`.


</v-clicks>

</div>

---

## Defining the operations

<div class="p-4">

<v-click>

`selection(table, condition)`  
- return the rows of table that meet the condition
- example: `t = selection(student, tot_cred > 100)`

</v-click>

<v-click>


`projection(table, attributes)`
- returns the specified columns of table
- example: `t = projection(student, ["student_name", "student_id"])`

</v-click>


<v-click>


`union(table1, table2)`
- returns the table containing the rows of table1 and table2
- example: `t = union(student1, student2)`

</v-click>

</div>


---

## Answering questions with the functions

<div class="p-5">

What are the names of students with more than 100 credits?

<v-clicks depth="2">

- To get the answer:
  - select the rows of the `student` table with `tot_cred > 100`
  - get the `name` attribute

</v-clicks>

<v-click>

`projection(selection(student, tot_cred > 100), ["student_name"])`

</v-click>

</div>


---

## Another example

<div class="p-5">


What courses are offered in Spring '09, and what buildings are they in?

<v-click>

![](/images/section.png){class="w-160"}

</v-click>


<v-click>

```
t = selection(section, semester="Spring" and year=2009)
projection(t, ["course_id", "building"])
```

</v-click>

</div>


---

## Exercise


<div class="p-5">

Write the operations to get the IDs of students who took course CS-101 in Fall 2009.


<v-click>


![](/images/takes.png){class="w-120"}

</v-click>


<v-click>



```
projection(selection(takes, course_id="CS-101"
  and semester="Fall" and year=2009), ["ID"])
```

</v-click>


</div>


---

## A 'union' example

<div class="p-5">

Get the ID of courses taught in Fall 2009 or Spring 2010

<v-click>

<small>section</small>
![](/images/section2.png){class="w-160"}

</v-click>

<v-click>

```
t1 = selection(section, semester="Fall" and year=2009)
t2 = selection(section, semester="Spring" and year=2010)
projection(union(t1,t2), ["course_id"])
```

</v-click>


<v-click>

Last line could alternatively be written:

</v-click>

<v-click>

```
union(projection(t1,["course_id"]), projection(t2,["course_id"]))
```

</v-click>

</div>


---

## Cartesian Product

<div class="p-4">

<v-click>

![](/images/cartesian.png){class="w-100"}

</v-click>

<v-click>

Question:<br>If you take the product of a table with 5 rows and a table with 8 rows, how many rows do you get? 

</v-click>

</div>




---

## Using cross product

<div class="p-4">


What are the names of CS instructors and what classes do they teach?


<div class="grid grid-cols-2 gap-4">

<div>

<v-click>

<small>instructor</small>
![](/images/instructor.png){class="w-100"}

</v-click>

</div>

<div>

<br>

<v-click>

<small>teaches</small>
![](/images/teaches.png){class="w-80"}

</v-click>

</div>

</div>

<v-click>

```
t1 = cross(instructor, teaches)
t2 = selection(t1, instructor.instructor_id = teaches.ID and instructor.dept_name = "Comp. Sci.")
projection(t2, ["instructor_name", "course_id"])
```

</v-click>

</div>

---

## Saying it in plain Greek

<div class="p-2">

To make it look more like math, you can write the operations using Greek letters and symbols


<v-click>

- `selection(section, year=2015)`
  - $\sigma_\text{year=2015} (\text{section})$  	
  - sigma ($\sigma$) --- "selection" starts with "s"

</v-click>

<br>

<v-click>


- `projection(section, ["course_id"])`
  - $\pi_\text{course\_id} (\text{section})$
  - pi ($\pi$) --- "projection" starts with "p"

</v-click>

<br>

<v-click>


- $\pi_\text{course\_id}(\sigma_\text{year=2015(section)})$
  - First, filters the `section` table to only the rows where year = 2015. 
  - Then, from those remaining rows, keep only the `course_id` column.

</v-click>


</div>

---
layout: section
---


## Part 2: SQL (DDL/DML)



---

## What is SQL?

<div class="p-4">

<v-clicks>

- SQL = "Structured Query Language"
- Originally developed at IBM in the early 70’s
- Pronounced “sequel” or S-Q-L 
- A special-purpose language that lets you express questions about your relational data in a precise way

</v-clicks>


<v-click>

SQL serves as the interface between the database and users/applications

</v-click>

</div>

---


## Parts of SQL

<div class="p-4">

SQL is not just for writing queries:

<v-clicks depth="2">

- Data-definition language (DDL)
  - create/delete/modify schemas
  - define integrity constraints
  - define views
  - drop tables
- Data-manipulation language (DML)
  - define queries
  - modify tables (insert/delete/modify tuples)
- TCL (Transaction Control Language): `BEGIN`, `COMMIT`, `ROLLBACK`
- DCL (Data Control Language): `GRANT`, `REVOKE`

</v-clicks>

</div>


---

## Defining a relation schema

<div class="p-4">


*Question*: What information do you provide when you define a relation schema?


<v-clicks depth="2">

- name of the table
- names and domains of each attribute
- primary key
- optionally:
  - whether an attribute is allowed to have a null value
  - foreign keys

</v-clicks>

</div>


---

## SQL for defining a relation schema

<div class="p-4">

<v-click>

Relation schema: `department(dept_name, building, budget)`

</v-click>

<br>

<v-click>


DDL table definition:
```sql
CREATE TABLE department (
   dept_name VARCHAR(20) PRIMARY KEY,
   building VARCHAR(15),
   budget NUMERIC(12, 2)
);
```

</v-click>

<br>

<v-clicks>

- For each attribute we give the attribute name and type.
- We also give the primary key.

</v-clicks>

</div>

---

## Another 'create table' example

<div class="p-4">

<v-click>

```sql
CREATE TABLE student (
   student_id VARCHAR(20) PRIMARY KEY,
   student_name VARCHAR(20) NOT NULL,
   dept_name VARCHAR(20), 
   tot_cred NUMERIC(3, 0),
   FOREIGN KEY (dept_name) REFERENCES department(dept_name)
);
```

</v-click>

<br>

<v-clicks depth="2">

- The foreign key constraint says:
  - every `dept_name` value in the student table must equal the key of some row in the department table.
- The `NOT NULL` constraint says:
  - no name value in the student table can be `NULL`
- PostgreSQL requires referenced tables exist before creating FKs --- `department` needs to be created first

</v-clicks>

</div>

---

## SQL Constraints Review

<div class="p-4">

<v-clicks>

- `PRIMARY KEY` = `UNIQUE` + `NOT NULL`
- `FOREIGN KEY` enforces referential integrity
- `NOT NULL` disallows missing values

</v-clicks>

</div>


---

## Attribute types

<div class="p-4">

Recall that attributes can only be simple values, like integers, and strings.*

<v-click>

Here are some of the main types allowed by SQL:

</v-click>

<v-clicks>

- `VARCHAR(n)`: a variable-length string of length at most `n`
- `INT`: an integer --- `integer` is also okay
- `NUMERIC(p, d)`: a fixed-point number of `p` total digits, with `d` digits to right of the decimal point.  
	  - Example: 123.45  is `numeric(5,2)`
- `FLOAT(p)`: a floating-point number with either 4 byte or 8 byte precision
  - alternatively can use `REAL` (4 bytes) and `DOUBLE PRECISION` (8 bytes)

</v-clicks>

<br>

<v-click>

<small>*In the relational model, attributes are atomic (simple) values.<br>PostgreSQL also supports richer types (e.g., arrays/JSON), but we’ll focus on relational design first.</small>

</v-click>

</div>

---


## Yet another example

<div class="p-4">


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


</div>

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

<v-clicks depth="4">

- `ID` in the **teaches** table is a foreign key that "points to" the `ID` field of the **instructor** table.
- What to do when the first row is deleted from the instructor table?

  - Default: delete is rejected if referenced.
    - `CASCADE`: referencing rows are deleted automatically.
      - `CASCADE` is powerful; use intentionally.

</v-clicks>


---

## Inserting rows into a table

<div class="p-4">


![](/images/patient6.png){class="w-140"}

<v-click>

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

</v-click>

</div>

---

## Inserting rows into a table (cont.)

<div class="p-2">


SQL also has statements to delete and modify rows, as well as batch operations.


<v-click>


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

</v-click>


<br>

<v-click>

```sql
INSERT INTO patient VALUES (454, 'Smith', 'John', 'M', '1978-08-14', 6);
INSERT INTO patient VALUES (223, 'Jones', 'Peter', 'M', '1985-12-07', 8);
INSERT INTO patient VALUES (597, 'Brown', 'Brenda', 'F', '1961-06-17', 3);
INSERT INTO patient VALUES (234, 'Jenkins', 'Alan', 'M', '1972-01-29', 7);
INSERT INTO patient VALUES (244, 'Wells', 'Chris', 'F', '1995-02-25', 6);
```

</v-click>


</div>

---

## More efficient insert

<div class="p-4">

- PostgreSQL supports multi-row insert

<br>

<v-click>

```sql
INSERT INTO patient (patient_no, last_name, first_name, sex, date_of_birth, ward)
VALUES
  (454, 'Smith', 'John', 'M', '1978-08-14', 6),
  (223, 'Jones', 'Peter', 'M', '1985-12-07', 8);
```

</v-click>

</div>

---

## Deleting or Modifying a table

<div class="p-4">


<v-click>

```sql
DROP TABLE IF EXISTS instructor;
```

- `DROP TABLE` deletes the table definition (and its data).

</v-click>



<br>

<v-click>


```sql
ALTER TABLE instructor ADD office VARCHAR(5);
```

- adds attribute 'office' to the instructor schema
- value of office is initialized to 'null'

</v-click>


</div>

---

## Part 2 Summary

<div class="p-4">

<v-clicks>

- `CREATE TABLE` — define a relation schema
- `INSERT` — insert rows into a relation instance
- `DROP TABLE` — delete relation schemas
- `ALTER TABLE` — modify relation schemas

</v-clicks>

</div>



