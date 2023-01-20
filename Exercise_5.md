# Exercise-V
Consider the Book Lending system from the library- BOOKS, STUDENT, BORROWS. The students are allowed to borrow any number of books on a given date from the library. The details of the book should include ISBN, Title of the Book, author and publisher. All students need not compulsorily borrow books. 

a) Mention the constrainst neatly.

```sql

CREATE TABLE books (
  isbn number(4) primary key,
  title varchar(30),
  author varchar(30)
);
  
CREATE TABLE student (
  sid number(4) primary key,
  name varchar(30),
  gender char(1)
);

CREATE TABLE borrows (
  sid number(4) references student(sid) on delete cascade,
  isbn number(4) references books(isbn) on delete cascade,
  issuedate date,
  primary key(sid,isbn)
);

INSERT ALL
INTO books VALUES (101, "C Programming", "Reema Thareja")
INTO books VALUES (102, "Computer Networks", "V S Bagad")
INTO books VALUES (103, "OOPS with Java", "Siddharth Santosh")
INTO books VALUES (104, "Software Engineering", "T Dayar")
INTO books VALUES (105, "Python Programming", "Sumita Arora")
INTO books VALUES (106, "DBMS", "Sumita Arora")
SELECT * FROM DUAL;

INSERT ALL 
INTO student VALUES (1, "Ram", "M")
INTO student VALUES (2, "Sita", "F")
INTO student VALUES (3, "Shyam", "M")
INTO student VALUES (4, "Manish", "M")
INTO student VALUES (5, "Harish", "M")
INTO student VALUES (6, "Gopal", "M")
SELECT * FROM DUAL;

INSERT ALL
INTO borrows VALUES (1,101,"1-2-22")
INTO borrows VALUES (1,102,"1-2-22")
INTO borrows VALUES (2,101,"1-3-22")
INTO borrows VALUES (2,103,"1-4-22")
INTO borrows VALUES (2,104,"1-3-22")
INTO borrows VALUES (3,102,"1-4-22")
INTO borrows VALUES (4,102,"1-5-22")
INTO borrows VALUES (5,105,"1-6-22")
INTO borrows VALUES (6,105,"1-7-22")
SELECT * FROM DUAL;


```



b) Design the ER diagram for the problem statement.

![ERD](https://user-images.githubusercontent.com/67141217/213426940-11f06ca8-aabe-4e3f-9377-759649af449b.png)

c) State the schema diagram for the ER diagram. 

![Schema](https://user-images.githubusercontent.com/67141217/213427737-f6baf512-48c8-4cbe-b6b3-a1cb93cccea2.png)

d) Create the tables, insert suitable tuples (min 6 each) and perform the following operations in SQL

![Table1](https://user-images.githubusercontent.com/67141217/213427810-4be8f43b-fbad-4e76-9472-989e7c84fbc0.png)

![Table2](https://user-images.githubusercontent.com/67141217/213427903-1bbf30b9-3a64-4060-b841-9ce4ff3cac72.png)

 1. Obtain the names of the student who has borrowed either book bearing ISBN ‘101’ or ISBN ‘105’.  

```sql 
SELECT sid, name 
FROM student 
WHERE sid IN ( SELECT sid 
             FROM borrows 
             WHERE isbn = 101 
             OR isbn = 105)
```

![output1](https://user-images.githubusercontent.com/67141217/213428394-dd2cfc2a-7337-46a0-9084-6367e66d8d3a.png)

 2. Obtain the Names of female students who have borrowed “OOPS with Java” books.
 
```sql
SELECT sid, name 
FROM student 
WHERE sid IN ( SELECT sid 
             FROM borrows 
             WHERE isbn IN ( SELECT isbn 
                           FROM books
                           WHERE title = "OOPS with Java"))
AND gender = "F";
```
![Output2](https://user-images.githubusercontent.com/67141217/213429009-cb5d3d41-9e60-42ef-a870-0c4f449356b8.png)

 3. Find the number of books borrowed by each student. Display the student details along with the number of books.

```sql
SELECT student.sid, name, gender, COUNT(borrows.isbn) as "No. of Books Issued"
FROM student, borrows
WHERE student.sid = borrows.sid
GROUP BY student.sid;
```

![Output3](https://user-images.githubusercontent.com/67141217/213429633-21e12da3-513b-491b-a71c-6e80a8115016.png)

 4. List the books that begin with the letters “DB” and has never been borrowed by any students.
 
 ```sql
SELECT title
FROM books
WHERE title like "DB%"
AND isbn NOT IN (SELECT DISTINCT isbn 
                FROM borrows);
              
 ```
 
![Output4](https://user-images.githubusercontent.com/67141217/213430078-b0d22dcc-2cc9-4eab-bc13-34e098a79e36.png)
 
e) Create the table, insert suitable tuples and perform the following operations using MongoDB

```javascript
db.createCollection("Library")
db.Library.insertMany([
    {"ISBN": 101, "Title" :'C Programming',"Author" : 'Reema Thareja', "SName" : "Ram", "SID":1 , "Date" : "1-2-22"},
    {"ISBN": 101, "Title" :'C Programming',"Author" : 'Reema Thareja', "SName" : "Sita", "SID":2 , "Date" : "1-2-22"},
    {"ISBN": 101, "Title" :'C Programming',"Author" : 'Reema Thareja', "SName" : "Manish", "SID":4 , "Date" : "1-2-22"},
    {"ISBN": 102, "Title" :'DBMS',"Author" : 'Reema Thareja', "SName" : "Ram", "SID":1 , "Date" : "1-2-22"},
    {"ISBN": 103, "Title" :'Python Programming',"Author" : 'Sumita Arora', "SName" : "Sita", "SID":2 , "Date" : "1-2-22"},
])
```


1. Obtain the book details authored by “author_name”. 

```javascript
db.Library.distinct("Title",{ "Author":"Reema Thareja"})
```

![MongoOut1](https://user-images.githubusercontent.com/67141217/213432152-878f074c-e49d-4c61-8a0d-9f2529099a15.png)


2. Obtain the Names of students who have borrowed “C Programming” books.

```javascript
db.Library.find({"Title" :'C Programming'}, { _id : 0, "SName" : 1}).pretty()
```

![image](https://user-images.githubusercontent.com/67141217/213432403-b10180a1-f276-4afd-8ed3-23cfbcb6858f.png)

f)Write a PL/SQL procedure to print the first 8 Fibonacci numbers and a program to call the same.

1. Create the PL/SQL file - ```fib.sql``` using the command ```edit fib.sql```

```sql
DECLARE
    a number := 0;
    b number := 1;
    c number := 0;
    counter number := 2;

PROCEDURE fibonacci is
	BEGIN
    	while (counter < 8)
        	loop
            c := a+b;
            dbms_output.put_line(c);
            a := b;
            b := c;
            counter := counter + 1;
        end loop;
    END;
BEGIN
  dbms_output.put_line("Fibonacci Series : ");
	dbms_output.put_line(a);
	dbms_output.put_line(b);
	fibonacci;
END;
/
```

2. Commands to execute the PL/SQL file -

```sql
set serveroutput on;
@fib.sql
Fibonacci Series : 
0
1
1
2
3
5
8
13
```
