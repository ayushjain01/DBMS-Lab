# Exercise-II
Consider the relations BOAT, SAILOR and RESERVES. The relation BOAT identifies the features of a boat such as unique identifier, color and a name. The list of sailors with attributes such as SailorID, name, age etc., are stored in the relation SAILOR. The sailors are allowed to reserve any number of boats on any day of the week and the records are to be updated in the RESERVES table. 

a) Mention the constrainst neatly.

```sql
CREATE TABLE boat (
  bid number(10) primary key,
  bname varchar(30),
  bcolor varchar(30)
);
  
CREATE TABLE sailor (
  sid number(4) primary key,
  sname varchar(30),
  age varchar(30)
);

CREATE TABLE reserve (
  bid number(10) references boat(bid) on delete cascade,
  sid number(4) references sailor(sid) on delete cascade,
  primary key (bid,sid)
);


INSERT ALL
INTO boat VALUES (101, "Vikrant", "Red")
INTO boat VALUES (102, "Maria", "Red")
INTO boat VALUES (103, "Santa", "Red")
INTO boat VALUES (104, "Cholan", "Green")
INTO boat VALUES (105, "Dange", "Green")
INTO boat VALUES (106, "Dhoka", "Green")
INTO boat VALUES (107, "Britannic", "Blue")
INTO boat VALUES (108, "Clement", "Blue")
INTO boat VALUES (109, "Khaja", "Blue")
INTO boat VALUES (110, "Burugina","Black")
SELECT * FROM DUAL;

INSERT ALL 
INTO sailor VALUES (1, "Gupta", 25)
INTO sailor VALUES (2, "Shyam", 45)
INTO sailor VALUES (3, "Harish", 34)
INTO sailor VALUES (4, "Joshi", 43)
INTO sailor VALUES (5, "Burugina", 23)
INTO sailor VALUES (6, "Chetan", 22)
INTO sailor VALUES (7, "Chris", 33)
INTO sailor VALUES (8, "Jacob", 44)
SELECT * FROM DUAL;

INSERT ALL
INTO reserves VALUES (107,4)
INTO reserves VALUES (109,4)
INTO reserves VALUES (102,4)
INTO reserves VALUES (109,3)
INTO reserves VALUES (104,2)
INTO reserves VALUES (103,2)
INTO reserves VALUES (109,1)
INTO reserves VALUES (106,1)
INTO reserves VALUES (107,1)
INTO reserves VALUES (108,8)
INTO reserves VALUES (106,8)
INTO reserves VALUES (105,7)
INTO reserves VALUES (109,2)
INTO reserves VALUES (109,5)
INTO reserves VALUES (109,6)
INTO reserves VALUES (109,7)
INTO reserves VALUES (109,8)
SELECT * FROM DUAL;

```



b) Design the ER diagram for the problem statement.

![ERD](https://user-images.githubusercontent.com/67141217/212526870-ca39b3fa-9b5a-461b-a434-8c5cf1fac3da.png)

c) State the schema diagram for the ER diagram. 

![schema](https://user-images.githubusercontent.com/67141217/212526876-80b28dcd-23e4-42e4-b4e3-e4a83e1220bf.png)

d) Create the tables, insert suitable tuples (min 6 each) and perform the following operations in SQL

![Table1](https://user-images.githubusercontent.com/67141217/212530349-445d3169-c2ec-46fc-bd20-2808ebf66fc9.png)

![Table2](https://user-images.githubusercontent.com/67141217/212526916-5074cef4-d16d-4420-8090-824c5cbaccf7.png)

![Table3](https://user-images.githubusercontent.com/67141217/212527248-e3269e8b-ce90-4b6b-abf5-4ae7ffe8a1e0.png)


 1. Obtain the details of the boats reserved by ‘#Sailor_Name’.

```sql 
SELECT *
FROM boat
WHERE bid IN ( SELECT bid
              FROM reserves
              WHERE sid IN (SELECT sid 
                           FROM sailor
                           WHERE sname = "Joshi"));
```

![Output1](https://user-images.githubusercontent.com/67141217/212527029-c65f871d-1442-4172-893f-4f587c98132d.png)

 2. Retrieve the BID of the boats reserved necessarily by all the sailors.
 
```sql
SELECT boat.bname
FROM boat
WHERE NOT (( SELECT sailor.sid
                    FROM sailor)
                  -
                  ( SELECT reserves.sid
                   FROM reserves
                   WHERE reserves.bid = boat.bid ));
```

**Alternate Solution**

```sql
SELECT *
FROM boat 
WHERE bid in(SELECT bid
             FROM reserves 
             GROUP BY bid
             HAVING COUNT(sid) = (SELECT COUNT(sid) 
                                  FROM sailor));
```

![output2](https://user-images.githubusercontent.com/67141217/212529892-38963475-b0b0-45bd-9685-80596f82f970.png)

 3. Find the number of boats reserved by each sailor. Display the Sailor_Name along with the number of boats reserved.

```sql
SELECT sailor.sname, COUNT(reserves.bid) AS "No. of Boats Reserved"
FROM sailor,reserves
WHERE reserves.sid = sailor.sid
GROUP BY sname;
```

![OUTPUT4](https://user-images.githubusercontent.com/67141217/212530190-e6a597ac-6f36-4f2e-9613-9bff8cb7da4b.png)


 4. Identify which boats have the same name as their sailor.
 
 ```sql
SELECT bid,bname,sid,sname
FROM boat,sailor
WHERE boat.bname = sailor.sname;
              
 ```
 
![Output4](https://user-images.githubusercontent.com/67141217/212530493-642f2e1d-9fa1-4e80-9812-e5318284c6e3.png)

 
e) Create the table, insert suitable tuples and perform the following operations using MongoDB

```javascript
db.createCollection("baots")
db.boats.insertMany([
    {"BID": 123, "BName" :'Vikrant',"Color" : 'Black', "SName" : "Gupta", "SNo":1111 },
    {"BID": 321, "BName" :'Maria',"Color" : 'Blue', "SName" : "Shyam", "SNo":1115 },
    {"BID": 124, "BName" :'Cholan',"Color" : 'Blue', "SName" : "Joshi", "SNo":5111 },
    {"BID": 312, "BName" :'Britannic',"Color" : 'Green', "SName" : "Chetan", "SNo":4511},
    {"BID": 122, "BName" :'Burugina',"Color" : 'White', "SName" : "Jacob", "SNo":3111 },
    {"BID": 124, "BName" :'Cholan',"Color" : 'Blue', "SName" : "Jacob", "SNo":3111 },
    {"BID": 312, "BName" :'Britannic',"Color" : 'Green', "SName" : "Jacob", "SNo":3111},
    {"BID": 122, "BName" :'Burugina',"Color" : 'White', "SName" : "Jacob", "SNo":3111 }
])
```


1. Update the details of parts for a given part identifier: #PID. 

```javascript
db.boats.find({"SName":"Jacob"}).count()
```

![mongooutput](https://user-images.githubusercontent.com/67141217/212531120-91b17612-bdec-4a8a-ab86-074e66daa545.png)

2. Retrieve boats of color :”#color”

```javascript
db.boats.find({"Color":'Blue'}).pretty()
```

![image](https://user-images.githubusercontent.com/67141217/212531174-eccefb78-05f3-491b-854b-4bd069f89b23.png)

f)Write a PL/SQL program to check whether a given number is prime or not. 

1. Create the PL/SQL file - ```prime.sql``` using the command ```edit prime.sql```

```sql
DECLARE
	n number := &n;
    j number := 2;
    count number :=0;
BEGIN
	while(j<=n/2) loop
    	if mod(n,j) =0 then
        	dbms_output.put_line(n || "is not a prime number");
            count := 1;
            exit;
        else
        	j := j+1;
        end if;
    end loop;
    if count = 0 then
    	dmbs_output.put_line(n || "is a prime number");
    end if;
END;
/
/
```

3. Commands to execute the PL/SQL file -

```sql
set serveroutput on;
@copy.sql
Enter value of n : 4
4 is not a prime number
```
