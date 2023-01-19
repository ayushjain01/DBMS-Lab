# Exercise-II
Consider the relations: PART, SUPPLIER and SUPPLY. The Supplier relation holds information about suppliers. The attributes SID, SNAME, SADDR describes the supplier. The Part relation holds the attributes such as PID, PNAME and PCOLOR. The Shipment relation holds information about shipments that include SID and PID attributes identifying the supplier of the shipment and the part shipped, respectively. The Shipment relation should contain information on the number of parts shipped.

a) Mention the constrainst neatly.

```sql
CREATE TABLE part (
  pid number(10) primary key,
  pname varchar(30),
  pcolor varchar(30)
);
  
CREATE TABLE supplier (
  sid number(4) primary key,
  sname varchar(30),
  saddr varchar(30)
);

CREATE TABLE supply (
  pid number(10) references part(pid) on delete cascade,
  sid number(4) references supplier(sid) on delete cascade,
  quantity number(3),
  primary key(pid,sid)
);

INSERT ALL
INTO part VALUES (101, "Bolts", "Red")
INTO part VALUES (102, "Nuts", "Red")
INTO part VALUES (103, "Plugs", "Red")
INTO part VALUES (104, "Bolts", "Green")
INTO part VALUES (105, "Nuts", "Green")
INTO part VALUES (106, "Plugs", "Green")
INTO part VALUES (107, "Bolts", "Blue")
INTO part VALUES (108, "Nuts", "Blue")
INTO part VALUES (109, "Plugs", "Blue")
SELECT * FROM DUAL;

INSERT ALL 
INTO supplier VALUES (1, "Ram", "Bangalore")
INTO supplier VALUES (2, "Shyam", "Delhi")
INTO supplier VALUES (3, "Harish", "Bombay")
INTO supplier VALUES (4, "Manish", "Chennai")
SELECT * FROM DUAL;

INSERT ALL
INTO supply VALUES (107,4,10)
INTO supply VALUES (109,4,15)
INTO supply VALUES (102,1,3)
INTO supply VALUES (109,3,6)
INTO supply VALUES (104,3,3)
INTO supply VALUES (103,2,5)
INTO supply VALUES (109,1,10)
INTO supply VALUES (106,2,10)
INTO supply VALUES (107,2,5)
INTO supply VALUES (108,3,10)
INTO supply VALUES (106,3,5)
INTO supply VALUES (109,2,3)
INTO supply VALUES (105,1,5)
SELECT * FROM DUAL;

```



b) Design the ER diagram for the problem statement.

![ERD](https://user-images.githubusercontent.com/67141217/212478740-54f292ac-56f8-460a-8b04-bf0f97611160.png)


c) State the schema diagram for the ER diagram. 

![schema](https://user-images.githubusercontent.com/67141217/212478762-96c1a2c6-8ecf-4d52-9ed3-f73ab898f57b.png)

d) Create the tables, insert suitable tuples (min 6 each) and perform the following operations in SQL

![Table1](https://user-images.githubusercontent.com/67141217/212478777-35d372f0-a0be-4974-aa7c-86066dd2a206.png)

![Table2](https://user-images.githubusercontent.com/67141217/212478786-58b7e18c-60ba-4c35-9599-4bc186fa98b1.png)

 1. Obtain the details of parts supplied by supplier #SNAME. 

```sql 
SELECT *
FROM part 
WHERE pid IN (SELECT pid
              FROM supply
              WHERE sid IN ( SELECT sid
                            FROM supplier
                            WHERE sname = "Manish"));
```

![Output1](https://user-images.githubusercontent.com/67141217/212524576-595695a0-8e5f-4442-adba-e55cbad6b641.png)

 2. Obtain the Names of suppliers who supply #PNAME.
 
```sql
SELECT *
FROM supplier 
WHERE sid IN (SELECT sid
              FROM supply
              WHERE pid IN ( SELECT pid
                            FROM part
                            WHERE pname = "Plugs"));
```
![Output2](https://user-images.githubusercontent.com/67141217/212524826-08e7adc2-0d71-45e8-9aae-5d83f51bf5cc.png)

 3. Delete the parts which are in #PCOLOR.

```sql
DELETe FROM part
WHERE pcolor = "Green";

SELECT * 
FROM PART;
```

![Output3](https://user-images.githubusercontent.com/67141217/212525172-dd8b2b49-9d1a-4fa6-b2d8-25b93581b82c.png)

 4. List the suppliers who supplies exactly two parts. 
 
 ```sql
SELECT sname
FROM supplier
WHERE sid IN (SELECT sid
              FROM supply
              GROUP BY sid
              HAVING COUNT(pid) = 2);
              
 ```
 
![OutPut4](https://user-images.githubusercontent.com/67141217/212525276-25d93d01-4b75-402c-89c8-bc8cbaf2b08a.png)

 
e) Create the table, insert suitable tuples and perform the following operations using MongoDB

```javascript
db.createCollection("warehouse")
db.warehouse.insertMany([
    {"PNo": 123, "PName" :'Bolts',"Color" : 'Black', "SName" : "Ram", "SNo":1111 , "Address" : "Mumbai"},
    {"PNo": 321, "PName" :'Chain',"Color" : 'Blue', "SName" : "Shyam", "SNo":1115 , "Address" : "Bangalore"},
    {"PNo": 124, "PName" :'Chain',"Color" : 'Blue', "SName" : "Raju", "SNo":5111 , "Address" : "Chennai"},
    {"PNo": 312, "PName" :'Wheels',"Color" : 'Green', "SName" : "Kaju", "SNo":4511 , "Address" : "Pune"},
    {"PNo": 122, "PName" :'Nuts',"Color" : 'White', "SName" : "Manish", "SNo":3111 , "Address" : "Delhi"}
])
```


1. Update the details of parts for a given part identifier: #PID. 

```javascript
db.warehouse.update({"PNo":122, {$set : {"Color":'Black'}},{multi:true}})
db.warehouse.find().pretty()
```

![MongoOutput1](https://user-images.githubusercontent.com/67141217/212525619-9e03bb2a-4cdc-4dec-bf7f-b4c29fa155eb.png)
![Mongooutput1CTD](https://user-images.githubusercontent.com/67141217/212525644-e141d2b7-0c96-41e0-bb47-0f78e689ee54.png)
![Mongo1outputcontinued](https://user-images.githubusercontent.com/67141217/212525672-a713905e-64d0-458e-a658-9c085d58a28c.png)


2. Display all suppliers who supply the part with part identifier: #PID.

```javascript
db.warehouse.find({"PNo":122}).pretty()
```

![MongoOutput2](https://user-images.githubusercontent.com/67141217/212525720-6e7555ed-5862-45e7-8acc-2f42dddbf79e.png)

f)Write a PL/SQL program to copy the contents of the Supply table to another table for maintaining records for specific part number. 

1. Create a table ```Shipment``` with same schema as ```supply```

Note : We add an always false condition 1=2 here to avoid copying rows from the ```supply``` table. We only copy the schema.

```sql
CREATE TABLE shipment AS
(SELECT *
FROM supply
WHERE 1=2);
```

2. Create the PL/SQL file - ```copy.sql``` using the command ```edit copy.sql```

```sql
DECLARE
    cursor c1 is select * from supply;
    v_rec supply%rowtype;
BEGIN
    open c1;
    loop
    fetch c1 into v_rec;
    exit when c1%notfound;
    insert into shipment values(v_rec.pid,v_rec.sid,v_rec.quantity);
    where v_rec.pid in (102,103,107)
    end loop;
    close c1;
END;
/
```

3. Commands to execute the PL/SQL file -

```sql
set serveroutput on;
@copy.sql
plsql execution completed successfully

SELECT *
FROM shipment;
```

![PLSQLoutput](https://user-images.githubusercontent.com/67141217/212526031-9dd4e911-78b1-4d44-8f54-c56c93fa366b.png)
