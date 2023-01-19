# Exercise-IV
Consider the Banking database – CUSTOMER, BRANCH, ACCOUNT and TRANSACTION. An account can be a savings account or a current account. Customer can have both types of accounts. The transactions can be a deposit or a withdrawal.

a) Mention the constrainst neatly.

```sql
CREATE TABLE branch (
  bno number(10) primary key,
  add varchar(30)
);
  
CREATE TABLE account (
  accno number(10) primary key,
  balance number(10,2),
  type varchar(30)
);

CREATE TABLE customer (
  custno number(10),
  name varchar(30),
  phone number(10),
  bno number(10) references branch(bno) on delete cascade,
  accno number(4) references account(accno) on delete cascade,
  primary key(custno,bno,accno)
);

CREATE TABLE transaction (
  accno number(4) references account(accno) on delete cascade,
  amount number(10,2),
  transtype varchar(30),
  transid number(10) primary key
);

INSERT ALL
INTO branch VALUES (01, "Bengaluru")
INTO branch VALUES (02, "Kolkata")
INTO branch VALUES (03, "Chennai")
INTO branch VALUES (04, "Mumbai")
INTO branch VALUES (05, "Delhi")
SELECT * FROM DUAL;

INSERT ALL 
INTO customer VALUES (101, "Raj", 1234567890, 01, 56901)
INTO customer VALUES (101, "Raj", 1234567890, 01, 66901)
INTO customer VALUES (102, "Kumar", 0987654321, 02, 56902)
INTO customer VALUES (103, "Kushal", 1222333444, 02, 56903)
INTO customer VALUES (104, "Manoj", 1123567890, 03, 56904)
INTO customer VALUES (105, "Ram", 1234567891, 04, 56905)
INTO customer VALUES (106, "Kunal", 1234565890, 05, 56906)
INTO customer VALUES (106, "Kunal", 1234565890, 05, 66906)
SELECT * FROM DUAL;

INSERT ALL
INTO account VALUES (56901,10000,"Savings")
INTO account VALUES (66901,15000,"Current")
INTO account VALUES (56902,12000,"Savings")
INTO account VALUES (56903,13000,"Savings")
INTO account VALUES (56904,14000,"Savings")
INTO account VALUES (56905,10600,"Savings")
INTO account VALUES (56906,70700,"Savings")
INTO account VALUES (66906,170000,"Current")
SELECT * FROM DUAL;

INSERT ALL
INTO transaction VALUES (56901,5000,"Deposit",30001)
INTO transaction VALUES (56901,3000,"Withdraw",30002)
INTO transaction VALUES (56901,2000,"Deposit",30003)
INTO transaction VALUES (56902,33000,"Withdraw",30004)
INTO transaction VALUES (56903,5400,"Deposit",30005)
INTO transaction VALUES (56904,54000,"Deposit",30006)
INTO transaction VALUES (56904,5000,"Withdraw",30007)
INTO transaction VALUES (56905,6600,"Withdraw",30008)
SELECT * FROM DUAL;

```



b) Design the ER diagram for the problem statement.

![ERD](https://user-images.githubusercontent.com/67141217/213407715-bf987069-5803-4733-a300-989a89c2ddd9.png)

c) State the schema diagram for the ER diagram. 

![schema](https://user-images.githubusercontent.com/67141217/213409479-f635ea94-5302-4ae2-b1b8-44b3779a900a.png)

d) Create the tables, insert suitable tuples (min 6 each) and perform the following operations in SQL

![Table1](https://user-images.githubusercontent.com/67141217/213409741-33ed9f81-5acd-476a-9a48-7756bc960276.png)

![Table2](https://user-images.githubusercontent.com/67141217/213409803-09129b82-120c-42bc-9488-b1053c23aaad.png)


 1. Obtain the details of customers who have both Savings and Current Account.

```sql 
SELECT custno, name, phone
FROM customer
JOIN account
ON customer.accno = account.accno
WHERE type = "Savings"
INTERSECT 
SELECT custno, name, phone
FROM customer
JOIN account
ON customer.accno = account.accno
WHERE type = "Current"
```

![OUTPUT1](https://user-images.githubusercontent.com/67141217/213413549-8ec3166c-1ebc-4add-9424-220c067f6c83.png)

 2. . Retrieve the details of branches and the number of accounts in each branch. 
 
```sql
SELECT branch.bno, address, COUNT(customer.accno) as "No. of Accounts"
FROM branch, customer
WHERE branch.bno = customer.bno
GROUP BY customer.bno;
```

![output2](https://user-images.githubusercontent.com/67141217/213414090-5e0a5037-ff7a-4b9b-a590-77d739f9e38d.png)

 3. Obtain the details of customers who have performed at least 3 transactions.
 
```sql
SELECT custno,name,phone, COUNT(transactions.transid) as "No. of Transactions"
FROM customer, transactions
WHERE transactions.accno = customer.accno
GROUP BY transactions.accno
HAVING count(transactions.transid) >= 3;
```

![output3](https://user-images.githubusercontent.com/67141217/213414810-60253341-5dc1-426f-9cef-e67df4857d79.png)


 4. List the details of branches where the number of accounts is less than the average number of accounts in all branches.
 
 ```sql
SELECT branch.bno, address, COUNT(customer.accno) as "No. of Accounts"
FROM branch,customer
WHERE branch.bno = customer.bno
GROUP BY branch.bno
HAVING COUNT(customer.accno) <= (SELECT (COUNT(account.accno) / COUNT(branch.bno))
                                FROM account,branch);
 ```
 
![Output4](https://user-images.githubusercontent.com/67141217/213416695-5fb8d842-0047-4c2c-96c1-813ae304acc5.png)

 
e) Create the table, insert suitable tuples and perform the following operations using MongoDB

```javascript
db.createCollection("Bank")
db.Bank.insertMany([
    {"CustId": 101, "CustName": "Raj", "AccNo" : 56901, "Balance" :12000, "BranchNo": 01, "AccountType":"Savings", "Bname" : "ABC"},
    {"CustId": 101, "CustName": "Raj", "AccNo" : 66901, "Balance" :12000, "BranchNo": 01, "AccountType":"Current", "Bname" : "ABC"},
    {"CustId": 102, "CustName": "Kumar", "AccNo" : 56902, "Balance" :112000, "BranchNo": 01, "AccountType":"Savings", "Bname" : "ABC"},
    {"CustId": 103, "CustName": "Kushal", "AccNo" : 56903, "Balance" :1000, "BranchNo": 01, "AccountType":"Savings", "Bname" : "ABC"},
    {"CustId": 103, "CustName": "Kushal", "AccNo" : 66903, "Balance" :22000, "BranchNo": 02, "AccountType":"Current", "Bname" : "XYZ"}
    ])
```


1. Find the branch name for a given Branch_ID.

```javascript
db.Bank.findOne({"BranchNo" : 01}, {"Bname" : 1, "_id" : 0})
```

![mongoOutput](https://user-images.githubusercontent.com/67141217/213419355-b251abbb-723b-4317-91f0-51cf8559be79.png)

2. List the total number of accounts for each customer.

```javascript
db.Bank.aggregate([{
        $group : {_id : { "Customer ID" : "$CustId"},
            Number_of_Accounts : {$sum : 1}}
    }])
```

![Output2](https://user-images.githubusercontent.com/67141217/213420321-a54fcb71-c169-4d31-ac0e-93cf363a0518.png)

f)Using cursors demonstrate the process of copying the contents of one table to a new table.

1. Create ```BranchCopy``` table with same schema as ```branch```
Note : We add an always false condition 1=2 here to avoid copying rows from the supply table. We only copy the schema.

```sql
CREATE TABLE BranchCopy AS 
(SELECT * 
FROM branch 
WHERE 1=2)
```

2. Create the PL/SQL file - ```branch.sql``` using the command ```edit branch.sql```

```sql
DECLARE
	cursor c1 is select * from branch;
    v_rec branch%rowtype;
BEGIN
    open c1;
    loop
    fetch c1 into vrec;
    exit when c1%notfound;
    insert into BranchCopy values (v_rec.bno, v_rec.address);
    end loop;
    close c1;
END;
/
```

3. Commands to execute the PL/SQL file -

```sql
set serveroutput on;
@branch.sql
plsql execution completed successfully
SELECT *
FROM BranchCopy;
```

![Outputlast](https://user-images.githubusercontent.com/67141217/213422020-b48e6347-6045-479b-94b7-aa1d734180db.png)
