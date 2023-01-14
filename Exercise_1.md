# Exercise-I
Consider an Employee with a social security number (SSN) working on multiple projects with definite hours for each. Each Employee belongs to a Department. Each project is associated with
some domain areas such as Database, Cloud and so on. Each Employee will be assigned to some project. Assume the attributes for Employee and Project relations. 

a) Mention the constrainst neatly.

```sql
CREATE TABLE employee (
  ssn number(10) primary key,
  fname varchar(30),
  salary number(7),
  DNo number(4)
);
  
CREATE TABLE project (
  pno number(4) primary key,
  pname varchar(30),
  domain varchar(30)
);

CREATE TABLE workson (
  ssn number(10) references employee(ssn) on delete cascade,
  pno number(4) references project(pno) on delete cascade,
  hours number(3),
  primary key(ssn,pno)
);

INSERT ALL
INTO employee VALUES (123, "John", 30000, 3)
INTO employee VALUES (321, "Franklin", 40000, 3)
INTO employee VALUES (124, "Alicia", 35000, 2)
INTO employee VALUES (312, "Mark", 60000, 2)
INTO employee VALUES (122, "Bill", 70000, 1)
INTO employee VALUES (120, "James", 10000, 1)
SELECT * FROM DUAL;

INSERT ALL 
INTO project VALUES (1111, "Product X", "IoT")
INTO project VALUES (2111, "Product Y", "Database")
INTO project VALUES (2311, "Product Z", "Database")
INTO project VALUES (4411, "Product W", "Cloud")
INTO project VALUES (2511, "Product V", "AR/VR")
SELECT * FROM DUAL;

INSERT ALL
INTO workson VALUES (123,1111,10)
INTO workson VALUES (321,1111,20)
INTO workson VALUES (123,2111,18)
INTO workson VALUES (124,2311,30)
INTO workson VALUES (120,4411,55)
INTO workson VALUES (122,1111,10)
INTO workson VALUES (124,1111,20)
INTO workson VALUES (321,2111,18)
INTO workson VALUES (312,2311,30)
INTO workson VALUES (321,4411,55)
INTO workson VALUES (312,2511,4)
SELECT * FROM DUAL;

```



b) Design the ER diagram for the problem statement.

![ERD](https://user-images.githubusercontent.com/67141217/212472698-b4a114c0-9013-41f4-9686-7c308daeee39.png)

c) State the schema diagram for the ER diagram. 

![Schema](https://user-images.githubusercontent.com/67141217/212472632-70f8ed97-48a0-4716-b7bb-e827df2ec33c.png)

d) Create the tables, insert suitable tuples (min 6 each) and perform the following operations in SQL

![Tables1](https://user-images.githubusercontent.com/67141217/212472796-c3a00fc6-e486-410e-a624-15f04c0178b2.png)

![Table2](https://user-images.githubusercontent.com/67141217/212473157-4fe4f740-8745-4a3e-91ec-247865f48cce.png)

 1. Obtain the details of employees assigned to “Database” project.

```sql 
SELECT *
FROM employee
WHERE ssn IN (SELECT ssn
              FROM workson
              WHERE pno IN (SELECT pno
                            FROM project
                            WHERE domain = "Database" ));
```

![Output1](https://user-images.githubusercontent.com/67141217/212472893-6279c293-84f0-425c-b3ed-4a55f31cfdf3.png)

 2. Find the number of employees working in each department with department details.

```sql
SELECT DNo, count(ssn) as "No. of Employees"
FROM employee
GROUP BY DNo;
```
![Output2](https://user-images.githubusercontent.com/67141217/212472969-35951b10-f402-4161-9a9a-7cafdc0b9f29.png)

 3. Update the Project details of Employee bearing SSN = #SSN to ProjectNo = #Project_No and display the same.

```sql
UPDATE workson
SET pno = 2311
WHERE ssn = 120;

SELECT *
FROM workson 
WHERE ssn = 120;
```

![OUTPUT3](https://user-images.githubusercontent.com/67141217/212473257-3df24579-fbce-4d17-a5fd-8d3586dde446.png)


 4. Retrieve the employee who has not been assigned more than two projects.
 
 ```sql
 SELECT ssn, count(pno) as "Projects Assigned"
FROM workson
GROUP BY ssn
HAVING count(pno)<=2;
 ```
 
 ![Output4](https://user-images.githubusercontent.com/67141217/212473313-81f25f9a-91fa-4a2e-ac23-420a9a845e6a.png)

 
e) Create the table, insert suitable tuples and perform the following operations using MongoDB

```javascript
db.createCollection("employee")
db.employee.insertMany([
    {"SSN": 123, "Name" :'John', "DName" : "Research", "PNo":1111},
    {"SSN": 321, "Name" :'Franklin', "DName" : "Research", "PNo":1115},
    {"SSN": 124, "Name" :'Alicia', "DName" : "Admin", "PNo":5111},
    {"SSN": 312, "Name" :'Mark', "DName" : "Admin", "PNo":4511},
    {"SSN": 122, "Name" :'Bill', "DName" : "Finance", "PNo":3111},
    {"SSN": 120, "Name" :'James', "DName" : "Finance", "PNo":2111}
    ])
```


1. List all the employees of Department named #Dept_name.

```javascript
db.employee.find({"DName":'Finance'}).pretty()
```

![MongoOutput1](https://user-images.githubusercontent.com/67141217/212474928-773a09b4-496a-4500-8930-511a7b59e3b1.png)

2. Name the employees working on Project Number :#Project_No

```javascript
db.employee.find({"PNo":1111}).pretty()
```

![MongoOutput2](https://user-images.githubusercontent.com/67141217/212474966-e457f2e9-0be0-412a-afc8-9ee452ca4f27.png)

f) Write a program that gives all employees in Department #number a 15% pay increase. Display a message displaying how many employees were awarded the increase.

1. Create the PL/SQL file - ```file1.sql``` using the command ```edit file1.sql```

```sql
BEGIN
    update employee
    set salary = salary + (0.15*salary)
    where dno = 2;
    dbms_output.put_line('Number of employess awarded hike are :' || sql%rowcount);
END;
/
```

2. Commands to execute the PL/SQL file -

```sql
set serveroutput on;
@file1.sql
plsql execution completed successfully
Number of employess awarded hike are : 2

SELECT ssn, salary, DNo
FROM employee;
```

![PLSQLoutput](https://user-images.githubusercontent.com/67141217/212475529-a6c0286c-2e8b-4469-aa31-89148d5b92f5.png)

