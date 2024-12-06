# LMS--SQL
Library Management System Project Using SQL
## Project Overview

**Project Title**: Library Management System  
**Level**: Intermediate  
**Database**: `library_project_p2db`

This project demonstrates the implementation of a Library Management System using SQL. It includes creating and managing tables, performing CRUD operations, and executing advanced SQL queries. The goal is to showcase skills in database design, manipulation, and querying.

## Objectives

1. **Set up the Library Management System Database**: Create and populate the database with tables for branches, employees, members, books, issued status, and return status.
2. **CRUD Operations**: Perform Create, Read, Update, and Delete operations on the data.
3. **CTAS (Create Table As Select)**: Utilize CTAS to create new tables based on query results.
4. **Advanced SQL Queries**: Develop complex queries to analyze and retrieve specific data.

## Project Structure

### 1. Database Setup
![ERD](https://github.com/maedeh-dqn/LMS--SQL/blob/main/library_ERD.png)

- **Database Creation**: Created a database named `library_project_p2db`.
- **Table Creation**: Created tables for branches, employees, members, books, issued status, and return status. Each table includes relevant columns and relationships.

```sql
--1-- Creating branch table
create table branch 
	(
		branch_id varchar(10) primary key,
		manager_id varchar(10),	
		branch_address varchar(55),	
		contact_no varchar(20)
	);

alter table branch
alter column contact_no type varchar(20);

--2-- Creating employees table
create table employees 
	(
		emp_id varchar(10) primary key,
		emp_name varchar(25),
		positions varchar(15),
		salary int,
		branch_id varchar(25) --Fk
	);

--3-- Creating books table
create table books 
	(	
		isbn varchar(20) primary key,
		book_title varchar(75),	
		category varchar(20),
		rental_price float,
		status varchar(15),
		author varchar(35),
		publisher varchar(55)
	);

alter table books
alter column category type varchar(20);

--4-- Creating members table
create table members
	(
		member_id varchar(10) primary key,
		member_name varchar(25),
		member_address varchar(75),
		reg_date date
	);

--5-- Creating issued_status table
create table issued_status 
	(
		issued_id varchar(10) primary key,
		issued_member_id varchar(10), --FK
		issued_book_name varchar(75),
		issued_date date,
		issued_book_isbn varchar(25), --FK
		issued_emp_id varchar(10) --FK
	);

--6-- Creating return_status table
create table return_status 
	(
		return_id varchar(10) primary key,
		issued_id varchar(10), --FK
		return_book_name varchar(75),
		return_date date,
		return_book_isbn varchar(20)
	);

```

### 2. CRUD Operations

- **Create**: Inserted sample records into the `books` table.
- **Read**: Retrieved and displayed data from various tables.
- **Update**: Updated records in the `employees` table.
- **Delete**: Removed records from the `members` table as needed.

**Task 1: Create a New Book Record.**
-- "978-1-60129-456-2', 'To Kill a Mockingbird', 'Classic', 6.00, 'yes', 'Harper Lee', 'J.B. Lippincott & Co.')" --

```sql
insert into books 
	(isbn, book_title, category, rental_price, status, author, publisher)
values 
	('978-1-60129-456-2', 'To Kill a Mockingbird', 'Classic', 6.00, 'yes', 'Harper Lee', 'J.B. Lippincott & Co.');
```
**Task 2: Update an Existing Member's Address.**

```sql
update members
set member_address = '945 California St'
where member_id = 'C103';
```

**Task 3: Delete a Record from the Issued Status Table.**
-- Objective: Delete the record with issued_id = 'IS121' from the issued_status table. --

```sql
delete from issued_status where issued_id = 'IS121';
```

**Task 4: Retrieve All Books Issued by a Specific Employee.**
-- Objective: Select all books issued by the employee with emp_id = 'E101'. --

```sql
select issued_book_name 
from issued_status
where issued_emp_id = 'E101';
```

**Task 5: List Members Who Have Issued More Than One Book.**
-- Objective: Use GROUP BY to find members who have issued more than one book. --

```sql
select issued_member_id, count(issued_id) total_books_issued
from issued_status
group by issued_member_id
having count(issued_id) > 1;
```

### 3. CTAS (Create Table As Select)

**Task 6: Create Summary Tables.**
-- Used CTAS to generate new tables based on query results - each book and total no_issued --

```sql
drop table if exists book_counts
create table book_counts
as
select 
	b.isbn, 
	b.book_title, 
	count(iss_stat.issued_id) as no_issued
from books as b
join issued_status as iss_stat
	on iss_stat.issued_book_isbn = b.isbn
group by b.isbn, b.book_title;

select * from book_counts;
```

### 4. Data Analysis & Findings

The following SQL queries were used to address specific questions:

**Task 7: Retrieve All Books in a Specific Category.**

```sql
select category, count(*) as no_books from books
group by category;

--We can also answer this question in the following manner. Every time we want to retrive a specific category, we just change the where clause. --

select * from books
where category = 'History';
```

**Task 8: Find Total Rental Income by Category.**

```sql
select 
	b.category, 
	sum(b.rental_price), 
	count(*)
from books as b
join issued_status as iss_stat
	on b.isbn = iss_stat.issued_book_isbn
group by b.category;
```

**Task 9: List Members Who Registered in the Last 180 Days.**

```sql
select * from members
where reg_date >= current_date - interval '180 days';
```

**Task 10: List Employees with Their Branch Manager's Name and their branch details.**

```sql
select 
	e1.*,
	b.manager_id, 
	e2.emp_name as manager 
from employees as e1
join branch as b
	on b.branch_id = e1.branch_id
join employees as e2
	on e2.emp_id = b.manager_id
order by manager;
```

**Task 11: Create a Table of Books with Rental Price Above a Certain Threshold 6USD.**

```sql
create table books_price_greater_than_six_usd 
as
select * from books
where rental_price > 6;

select * from books_price_greater_than_six_usd;
```






