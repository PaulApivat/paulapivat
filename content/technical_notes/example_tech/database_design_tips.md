---
date: "2021-10-26T00:00:00+01:00"
draft: false
linktitle: Database Design Course
menu:
  example_tech:
    parent: Database
    weight: 2
title: Database Design Course by Caleb Curry
toc: true
type: docs
weight: 2
---

## Introduction to Database Design 

**Situation**: There are notes from an [8 hour course on YouTube](https://www.youtube.com/watch?v=ztHopE5Wnpc). 

## TLDR

1. What is the relationship between entities (tables)? Physically draw out the lines and relationships (cardinality)
	1. one-to-one
	2. one-to-many ('many' side is the Foreign Key)
	3. many-to-many (break into two one-to-many relationships w/ intermediary table)
2. Do we need Lookup Tables?
3. Design Data Tables for Integrity
	1. Entity Integrity - ID for uniqueness 
		1. Ensure Atomic Values (Natural Keys, if cannot, then add surrogate keys)
	2. Referential Integrity - connect tables between Foreign Keys to Primary Keys
	3. Domain Integrity - identify data types of each variable (i.e., numeric, string, date)
	4. No repeating data
3. Identify which foreign key is NOT NULL (surrogate id will Auto-Increment) (modality)
4. Normalize the data
	1. 1 NF (first normal form) - atomicity
	2. 2 NF (second normal form) - partial depenency
	3. 3 NF (third normal form) - transitive dependency (solution: take problematic columns and split into their own tables with foreign key)
5. Foreign Key Constraints; SQL statements:
	1. ON DELETE
		1. RESTRICT
		2. CASCADE
		3. SET NULL
	2. ON UPDATE
		1. RESTRICT
		2. CASCADE
		3. SET NULL
6. Deteremine which JOIN is needed to get the best "view"; which table goes after the FROM statement (which table is on the left?)
	1. INNER JOIN
	2. LEFT JOIN 
	3. SELF JOIN


**What is a Relational Database?**
1. Entity (Rows)
	1. Entity = User (a person, an object)
	2. Row = all attribute values for an entity 
2. Attribute (Columns)
	1. Attributes are *about* an entity (user name, name, password, address etc)
3. Mathematical name for a Row is a Tuple

**RDBMS**
1. View Mechanism changes the how data is presented (i.e., we don't want all rows x columns, just a subset)
	1. Select only certain columns
2. View = Read Only (not everyone has access to Update for security purpose)
3. RDBMS allows "transactions"
4. Example: MySQL, SQL Server, PostgreSQL (open source)
5. Database & Relational Database are not separate things


**SQL**
1. Define structure (DDL, data definition language) 
	1. CREATE, 
	2. structure & connection between tables - JOIN
2. Manipulates data (DML, data manipulation language) - 
	1. UPDATE
	2. data within tables


**What is Database Design?**
1. Separate information into multiple tables, while preventing data integrity issues
2. How do you measure whether a database is "good" or "bad"? --> Data Integrity
3. Good design prevents "data integrity issues"
	1. All data up to date
	2. No repeating data
	3. No incorrect data
	4. No broken relationships
4. Conceptual Schema: How data is related.
5. Logical Schema: Table structures (i.e., X number of columns, data types), number of tables
6. Physical Schema: Implementing into database, table types, what server? how will people access?


**Data Integrity**
1. Entity integrity
	1. ID is used to enforce uniqueness of an entity (user)
2. Referential integrity
	1. Foreign key constrains
3. Domain integrity
	1. Range of what we're storing (correct Data Types; integers, text or dates)
4. Note: Relational Database does not come from the word "relationship", it comes from Relations which is a mathematical connection between Sets
5. When we don't have data integrity, we have errors
6. When we have errors, data integrity allows us to correct those errors


**Database Terms (Review)**
1. Data
2. Database
3. Relational Database (stores things in tables)
4. DBMS (how to control database)
5. RDBMS 
6. Null (when someone doesn't enter a value)
7. Anomolies (errors)
8. Integrity (protect against anomolies)
	1. Entity
	2. Referential - keep connection through Foreign Keys and Primary Keys
	3. Domain - correct data types
9. Entity - what we store
10. Attributes*** - things about an entity
11. Relations* - connection between two sets or Tables
12. Tuple** or Row (all attributes about an entity)
13. Table* - physical representation of
	1. Rows**
	2. Columns***
14. File* (aka Table)
15. Record** (aka Row)
16. Field*** (aka Column)
17. Value (something in a column)
18. Entry (aka a Row)
19. DB Design - process of designing table to have integrity
20. Schema - structure of tables
21. Normalization - steps to get the best data base design
22. Naming Convention - consistency in naming 
23. Keys - something to make things in unique in a table and connect between tables

**More Database Terms**
1. SQL
	1. DDL data define language
	2. DML data manipulation language
2. SQL Keywords - reserved words (e.g., SELECT)
3. Frontend - we program frontends so people can securely access the database (doesn't allow us to type in SQL)
4. Backend - serverside code to communicate with database
5. Client side
6. Server side - serves instances of the database to the client
7. Server side scripting language
8. Views - taking data from database and illustrating it in a different from how it's stored
9. Joins - connect data from multiple tables
	1. ID foreign key connection


**Atomic Values**
1. Everything in a database should be about 1 thing
2. Example: "Paul Apivat Hanvongse" -- to make it atomic, create 3 separate columns (i.e., nickname, first name, last name)
3. Atom - smallest indivisible piece (1 thing) but still makes sense to treat as 1 thing
	1. example: Address - street, city, state, zip code (3 columns)


**Relationships**
1. Relationship - connects two or more entities

Example:
					(Entities)
Database --> Student - Attribute
				 --> Professor - Attribute
				 --> Class - Attribute
				 --> Major - Attribute
				 
There's multiple relationships here; Student have a Major, Students are in a Class, Professors are part of a Major, Professors teach a Class.


One-to-One 
1. One Entity has connection with another Entity (e.g., Husband - Wife)
2. Social security number unique to one person


One-to-Many
1. Comments under a Youtube video; 

User --> Comment 1
		 --> Comment 2
		 --> Comment 3


Many-to-Many
1. Polygamous marriage (i.e., Multiple husbands have multiple wives)
2. College: Class & Students; class can have multiple students & students can take multiple classes


**Designing One-to-One Relationships**
1. Example: Person and Username
2. Generally One-to-one relations are stored in the same table

| ID | name   | user_name   |
|----|--------|-------------|
| 1  | Apivat | Paul        |
| 2  | Caleb  | Caleb_Curry |

3. There are times when one-to-one relations are stored in *different* tables (when you want to store *extra* attributes *about* the attribute)

Cardholder Entity          
| ID               |
|------------|
| first_name |
| last_name  |
| card_id       |


(card_id <--> ID )

Card
| ID                    |
|-------------   |
| card_number |
| issue_date      |
| late_fees         |
| max                 |

Summary - One-to-One Relationship
1. Store attribute of the entity in the table
2. OR use another table and connect with a foreign_key

**Designing One-to-Many Relationships**
1. The "many" side is a foreign key to the "one" side
2. User id stays the same. 


| User    |
|---------|
| user_id |  <-- primary key (in primary table; aka Parent)
|         |


(multiple cards)

| Card    |
|---------|
| card_id |
| user_id |   <-- foreign key (aka child)


| Card2    |
|---------|
| card2_id |
| user_id |  <-- foreign key (aka child)


| Card3    |
|---------|
| card3_id |
| user_id |  <-- foreign key (aka child)


**Parent Tables and Child Tables**
1. Tables are either Parent or Child
2. Keys keep tables Unique
3. Primary key = Parent (User ID)
4. Foreign key = Child (user_id as a reference to User ID)
5. Child points back to Parents
6. In one-to-one, we don't have to worry about parent or child
7. In one-to-many, many Children point to a Parent
8. When we have a Child table, we always know the Parent (but not vice versa)



**Notation**
One to One --> 1 : 1
One to Many --> 1 : N
Many to Many --> M : N


**Designing Many-to-Many Relationships**
1. M : N
2. Classes : Students
3. Parent <--> Parent
4. Solution: Break it up to TWO One-to-Many Relationships with an INTERMEDIARY or JUNCTION table to connect


| ID | Class   |
|----|---------|
| 63 | math    |
| 75 | science |
| 89 | english |


| ID | Student |
|----|---------|
| 8  | John    |
| 17 | Jake    |
| 16 | Sally   |
| 6  | Claire  |


Intermediary Table 
(Child Table for both Parents)


| class_id | student_id |
|----------|------------|
| 75       | 8          |
| 89       | 8          |
| 63       | 17         |
| 75       | 17         |
| 89       | 17         |
| 89       | 16         |
| 63       | 6          |
| 89       | 6          |


**Summary of Relationships**
1. Now we can design every "binary relationship" - any relationship between two Entities


**Introduction to Keys**
1. Keys should be Unique
2. Never Changing
3. Never NULL
4. What should be unique? (e.g., a user e-mail)
	1. Could be a *Natural Key* (already in the table, no need to define a new column)
	2. Could be *user name*.
5. Key should be Never Changing (otherwise database integrity is compromised)


**Primary Key Index**
1. Index - think Index in a Book; 
2. Index points you to the data
3. Keys are a type of Index
4. Indexs are used for 
	1. SELECT * FROM
	2. WHERE first_name = 'Caleb'     (need index for this)
	3. 


**Look Up Table**

Example look-up table of member status:
1. member_status id is the foreign key that can point to a members table
2. all connections stay the same even if member_status changes
3. can set Foreign Key constraints


| id | member_status        |
|----|----------------------|
| 1  | gold                 |
| 2  | silver               |
| 3  | bronze               |
| 4  | first_quest_complete |
| 5  | guess_pass           |
| 6  | level_1              |
| 7  | level_2              |
| 8  | level_3              |


(where member_status & student_id are One-to-Many)

| id | student | member_status_id |
|----|---------|------------------|
| 8  | John    | 3                |
| 17 | Jake    | 3                |
| 16 | Sally   | 6                |
| 6  | Claire  | 7                |

(where member_status & student_id are Many-to-Many)

| student_id | member_status_id |
|------------|------------------|
| 8          | 3                |
| 8          | 4                |
| 17         | 6                |
| 17         | 7                |
| 17         | 8                |
| 16         | 1                |
| 6          | 4                |
| 6          | 5                |

Lookup Tables (w Keys) allow for:
1. Integrity
2. Uniqueness
3. Improves functionality (no repeating data)
4. Less work
5. Allows for added complexity


**Superkey and Candidate Key**
1. Two main kinds of keys:
	1. Primary Key
	2. Foreign Key


Superkey
1. Any number of columns that forces each row to be unique
2. How do we know each row is unique and talks about one entity (user)?
3. Superkey = any number column values  to force that each row is unique
4. Candidate key = the least number of column to force every row to be unique (ie., username)

You'll never program a superkey

Candidate Key
1. Superkey is asking: "Can every row be unique?"
2. Then, once we answer yes, Candidate key asks: "How many columns are needed (to force every row to be unique) ?" -- what's the least number of columns
3. Then, "How many Candidate Keys do we have?"
4. THEN, decide which Candidate Key will be the PRIMARY KEY.



**Primary Key & Alternate Keys**
Primary Key Possibilities
1. username*
2. email
3. full name, last name, middle name, address, birthday

*out of these three, username is the best primary key


What are the criteria for choosing a Primary Key?
1. UNIQUE
2. NEVER CHANGING
3. NEVER NULL

Primary keys can also be an Index (use Select statement and how you connect most of your data).

Keys *not* chosen to be the Primary Key become the Alternate Keys. Alternate Keys can be useful - you could use "email" (an alternate key) as an Index.

SELECT * FROM table
WHERE email = "value@gmail.com"

**Surrogate Keys and Natural Keys**
1. These are "categories" of Primary Keys
2. You won't search for these, but you'll Design the Database with these in mind
3. Natural Keys are NATURALLY in the table; they fit requirements for Primary Keys and already *in* the table (email, username)
4. Surrogate Keys are ADDED to your table (i.e., _id_); they AUTO-INCREMENT
	1. user --> user_id
	2. sale --> sale_id
	3. comment --> comment_id
5. 



**Should I use Surrogate Keys or Natural Keys?**
1. Natural keys = already there, but not always obvious which should e natural key
2. Surrogate keys = easy, but you have to add a new column
3. Choose one or the other	
	1. minor performance differences
4. Caleb personally uses Surrogate Key
	1. Example:    user --> user_id


**Foreign Key**
1. Foreign Key References a Primary Key (either same table or separate table)
2. Every table has ONE Primary Key (could be composed of many columns)
3. Every table can have MULTIPLE Foreign Keys referencing many other tables



**Not NULL Foreign Keys**
1. Foreign Key constraints
2. Every row is required to have a value if the column has "not Null"
3. related to Cardinality 
4. (NOTE: sometimes you don't want to set "Not NULL" because  an id doesn't currently exist; but sometimes you want to force that relationship to be there between Foreign Key and Primary Key)
5. you either want Not NULL or Not Required (depending on the situation)
5. Primary Key values should never change, Foreign Key values *can* change
6. We don't want Primary Key values to change, but we could have Foreign Key references change (?)
7. Primary Key and Foreign Key should be the same data type
8. 


**Foreign Key Constraints**
1. FK = Referential Integrity
2. Make sure if you update Parent, ZChildren will update
3. Prevent creating Children if there's no Parent

SQL Statements that talk about FK constraints and refer to the Parent (Primary Key)
1. ON DELETE = when we delete the Parent, we want to delete the Child
	1. RESTRICT = (no action) throw an error whenever the Parent is deleted
	2. CASCADE = whatever we do to Parent, we do to Child (delete parent, delete child)
	3. SET NULL = if delete Parent, sets Child to NULL
2. ON UPDATE = when we update the Parent, we want to update the Child
	1. RESTRICT = (no action) throw an error when try to Update Parent
	2. CASCADE = If we update Parent, Child updates as well
	3. SET NULL = if update Parents, thro


- Every Foreign Key column value needs to reference a Primary Key value
- 

**Simple Key, Composite Key, Compound Key**
- categories of Keys
- Simple Primary Keys - single column (e.g., username)
- Composite Primary Key has multiple columns, as a group as Primary Key; at least one column doesn't have to be a key
- Compound Primary Key - combination of columns in intermediary tables (in many-to-many relations); Primary Keys are compounded; all columns have to be a key


For Intermediary Tables some people will
- add a surrogate_id to the Compound


**REVIEW**
1. Superkey
2. Candidate Key - least number of columns used to enforce uniqueness
3. Primary Key** - the candidate key you select as the Main key
4. Alternate Keys - the candidate keys you didn't select as Primary Key
5. Foreign Keys** - make connection between tables; references Primary Keys


Primary + Foreign
1. Surrogate & Natural Keys - surrogate (user_id) is random with no real value; Natural is already contained in your database 
2. (don't switch between these two)
3. Rule: You should be able to enforce uniqueness by the columns that are Naturally already there, *add* a surrogate key if you want
4. Rule 2: If you cannot define uniqueness naturally, you'll need to rely on a Surrogate key (try to avoid)


(can switch between Simple + Composites - you won't define these explicitly)
6. Simple Key - one column key
7. Composite Key - multiple column keys
8. Compound Key - multiple column keys


Foreign Key Constraints
1. ON DELETE
	1. RESTRICT
	2. CASCADE
	3. SET NULL
2. ON UPDATE
	1. RESTRICT
	2. CASCADE
	3. SET NULL


**Introduction to Entity Relationship Modeling**
- A standard for Drawing Databases
- EER Model (Enhanced Entity Relationship Model)
- ERD            (Enhanced Relationship Diagram)
- ER Model   (Enhanced Relationship)
- DDL: Define Database Structure

{do actual drawing}


**Cardinality**
- one to one: |-------|
- one to many:    |-----------E
- one row to many rows
- many to one:    E---------|


**Modality**
- Can assign "NOT NULL" to Foreign Key, to say each card *must* have an owner

| card_holder (Primary Key) | card Foreign Key | card_number |
|---------------------------|------------------|-------------|
| 7                         | 7                | 12          |
| 12                        | 7                | 48          |
| 368                       | 7                | 98          |
|                           | 368              | 112         |


4 Modalities
card_owner -------- card

1. -|---------0--|-      one-to-one; one card_owner can have zero or 1 card
2. -|---------|--|-      one-to-one; one card_owner must have 1 and only 1 card
3. -|---------0--E-  one-to-many; one card_owner can have 0 or many cards
4. -|---------|--E-  one-to-many; one card_owner must hv 1 or many cards


**Introduction to Database Normalization**
1. Normalization is a process where we go through our database and correct things that may cause database problems like 
	1. data integrity problems
	2. repeating data


Third Main Forms (three step-by-step data normalization process)
1. 1 NF     (first normal form)
2. 2 NF    (second normal form)
3. 3 NF    (third normal form)

Systematic way to normalize a good structured database
- everything must be atomic
- think about how data *depends* on other data (dependencies)
- must go in sequential order 1 NF --> 2 NF --> 3 NF


**1 NF (First Normal Form of Database Normalization)**
- Atomicity  (data must be atomic or about one thing)

Problem: "Address column" is not atomic (street, apt, city, country, zip)
Solution: Break this column into multiple columns

Problem: User enters two email(s) as a single value (two values in one cell)
Still Problem: Generate same two users for two emails (two of same primary keys)

Solution: Break into *two* tables - User table & Email table - turn this into a 
one-to-many   (one user to many emails)

User
user_id (primary key)
first_name
last_name

Email
email_id
email
user_id (foreign key)


**2 NF (Second Normal Form of Database Normalization)**
- *Partial Dependency*  (when a column depends on part of a Primary Key)
- You need ot have a *compound* or *composite* key (Primary Key has to be multiple columns)
- Found in Many-to-Many relationships w/ Intermediary Tables


{see notebook for illustration}



**3 NF (Third Normal Form of Database Normalization)**
- must do 1st and 2nd Normal Form before getting to 3 NF
- *Transitive Dependency* (when a column depends on a column, which depends on a Primary Key)
- Solution: Take a transitive dependency (problematic column), move them to a new table and reference them with a foreign key


**Summary of Normal Forms**
1. 1 NF = making everything atomic
2. 2 NF = removing partial dependencies
3. 3 NF = removing transitive dependencies 


**Indexes (Clustered, Nonclustered, Composite Index)**

- See a book's index (or Phone Book)
- A list of where certain data points are
- Data is sorted in a way that can easily be found
- Nonclustered Index = tells you how to get to the data (book's index)
	- points to the data
	- a list of references that point to the data (like back of the book)
	- 
- Clustered Index = organizes the actual data in a way that's easy to use
	- organizes the actual data
	- faster and better than non-clustered

Databases
- Rather than having to go through "all" the data (i.e., Table Scan), you create an index (index seek, makes queries faster)
- Downside: When you update the data, you have to update the index as well otherwise index becomes outdated and useless
- 
- 
- You only want to create an index for frequently used data
	- Primary Key that is indexed makes it faster
- Apply WHERE (sql query) column to an Indexed column
- Index increase speed of JOINS
- *Whenever you're JOINing certain columns, the two columns you're joining should be Indexed*


**Data Types**
1. Date
	1. date
	2. time
	3. datetime
	4. timestamp (can be millisecond or time, when something was "done" or when something was "created" or "updated" or when a new row is entered)
2. Numeric
	1. integer (only whole numbers)
	2. decimal (more accurate)
	3. float / double (unsigned)
	4. binary
3. String  
	1. Char(8), 
	2. varchar(8) 0 up to 8 characters
	3. text - longer strings (comments, paragraphs)


## Everything above here is DDL - data definition language

## Below here are DML - data manipulation language**


**Introduction to JOINS**

- Joins bring multiple tables into a presentable format


**Inner Join**
- Table A (customer table)
- Table B (card table)
- When there are rows that connect them, a new Table is presented
- Taking only the rows that intersect between two tables
	- Eliminate any customers that do not have a card
	- Eliminate any cards that do not have a customer
- Exclude rows that are *not* in both tables

Example:

Customer Table

| customer (Primary Key) |
|------------------------|
| customer_id            	   |
| first_name            		 |
| last_name             		 |

Card Table

| card (Foreign Key) |
|--------------------|
| card_id            |
| customer_id        |
| max_amount         |
| monthly_bill       |
| amount_paid        |
| amount_owed        |


Example Inner Join of Customer Table & Card Table

| first_name | last_name | amount_paid | amount_owed |
|------------|-----------|-------------|-------------|
| Paul       | Apivat    | 2200        | 3000        |
| Paul       | Apivat    | 720         | 1000        |
| Jimmy      | John      | 3000        | 5000        |


SELECT
	first_name,
	last_name,
	amount_paid,
	amount_owed
FROM customer cu
INNER JOIN card ca ON cu.customer_id = ca.customer_id

**INNER JOIN on 3 Tables**

See illustration

**INNER JOIN on 3 Tables (with Example)**

See illustration

**Introduction to OUTER JOINS**
1. INNER JOIN                      - least rows
2. LEFT (Outer) JOIN  
3. RIGHT (Outer) JOIN
4. FULL (Outer) JOIN          - most rows

**RIGHT OUTER JOIN**
- same as Left Outer Join, except the Right Table keeps all the rows
- IN PRACTICE, most people don't use Right Joins; instead they'll use a LEFT JOIN and just flip the tables

- How do you know which table is LEFT or RIGHT?

SELECT column1, column2, column3
FROM this_table_is_Left
LEFT JOIN....

**JOIN with NOT NULL Columns**
- Not Null columns can cause some confusion when it comes to Joins
- If you want to return *all* of a column, don't worry about NOT NULL, just put that table in the SELECT statement (i.e., make the table on the left side) and use a LEFT JOIN

{see illustrations}


**Outer JOIN Across 3 Tables**
Can combine
- LEFT (outer) JOIN with
- RIGHT (outer) JOIN

**Aliases**
- use when writing SELECT statements
- use AS


**SELF JOIN**
- take a Table and JOINING with itself
- How to think about it:
1. Duplicate your exact table
2. Joining with itself

Illustration

| v1          |
|-------------|
| user        |
| referred_by |
|             |


| v2      |
|---------|
| user_id |     --------  referred_by
|         |


SELECT
	v1.first_name,
	v1.last_name,
	v1.email,
	v2.email
FROM user AS v1
JOIN user AS v2
ON v1.referred_by = v2.user_id

(Self-Join is taking the same table, "user", and making a duplicate)



For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).