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
