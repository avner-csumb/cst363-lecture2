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

<div class="p-2">

<v-clicks>

- As part of applying an operation of relation algebra, duplicate rows should be removed.
- Duplicate rows aren’t technically allowed in a valid table.
- In practice a DB system may allow duplicate rows.

</v-clicks>

</div>

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

<div class="p-2">



```sql
CREATE TABLE department (
   dept_name VARCHAR(20) PRIMARY KEY,
   building VARCHAR(15),
   budget NUMERIC(12, 2)
);
```

For each attribute we give the attribute name and type.

We also give the primary key.

</div>

---

## Another ‘create table’ example

<div class="p-2">


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

</div>

---

## Attribute types

<div class="p-2">


Recall that attributes can only be simple values, like integers, and strings.


Here are some of the main types allowed by SQL:

`VARCHAR(n)`: a variable-length string of length at most n

`INT`: an integer – ‘integer’ is also okay

`NUMERIC(p, d)`: a fixed-point number of p total digits, with d digits to right of the decimal point.  
	
Example: 123.45  `isnumeric(5,2)`

`FLOAT(p)`: a floating-point number with either 4 byte or 8 byte precision 


</div>

---


## Yet another example

<div class="p-2">


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

ID in the **teaches** table is a foreign key that "points to" the ID field of the **instructor** table.


What to do when the first row is deleted from the instructor table?

---

## Inserting rows into a table

<div class="p-2">


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

</div>

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



