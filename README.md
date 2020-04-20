# Retail-Business-Management-System-project
This project is about Writing SQL Queries Using Oracle's SQL*Plus


SQL> start query1
SQL> select CNAME,TELEPHONE# from customers where VISITS_MADE >= 3 AND TELEPHONE# like '666%'
  2  /

CNAME           TELEPHONE#                                                      
--------------- ------------                                                    
Kathy           666-555-4567                                                    
Chris           666-555-6745                                                    

SQL> start query2
SQL> select customers.cname,customers.telephone#
  2  from Customers,Purchases
  3  where customers.cid = purchases.cid
  4  and total_price >= 100
  5  and current_date - ptime between 0 and 25
  6  /

CNAME           TELEPHONE#                                                      
--------------- ------------                                                    
Kathy           666-555-4567                                                    
Chris           666-555-6745                                                    

SQL> start query3
SQL> select p.PID, p.PNAME from products p, purchases p1, employees e
  2   where (ORIGINAL_PRICE*(1 - DISCNT_RATE)) < 10 AND p.PID =p1.PID AND p1.EID=e.EID AND e.ENAME = 'Peter'
  3  /

PID  PNAME                                                                      
---- ---------------                                                            
p005 chair                                                                      

SQL> start query4
SQL> select pur#,purchases.eid,pid,purchases.cid,qty,ptime,total_price from purchases
  2  	join customers
  3  	on purchases.cid = customers.cid
  4  	join employees
  5  	     on purchases.eid = employees.eid
  6  	     where purchases.pid != (select pid from products where pname = 'TV')
  7  	     and substr(employees.telephone#,1,3) = substr(customers.telephone#,1,3)
  8  /

      PUR# EID PID  CID         QTY PTIME     TOTAL_PRICE                       
---------- --- ---- ---- ---------- --------- -----------                       
    100002 e01 p003 c001          1 20-FEB-20       118.4                       
    100004 e01 p005 c003          2 23-FEB-20       18.17                       
    100009 e03 p001 c007          1 12-MAR-20        8.99                       
    100011 e02 p004 c006         10 16-MAR-20         9.9                       

SQL> start query5
SQL> select PUR#,to_char(PTIME,'MONTH DD,YYYY DAY HH24:mi:ss') as PTIME from purchases ORDER BY PTIME ASC
  2  /

      PUR#                                                                      
----------                                                                      
PTIME                                                                           
--------------------------------------------------------------------------------
    100007                                                                      
FEBRUARY  10,2020 MONDAY    17:12:20                                            
                                                                                
    100012                                                                      
FEBRUARY  18,2020 TUESDAY   15:56:38                                            
                                                                                
    100002                                                                      
FEBRUARY  20,2020 THURSDAY  11:23:36                                            
                                                                                

      PUR#                                                                      
----------                                                                      
PTIME                                                                           
--------------------------------------------------------------------------------
    100004                                                                      
FEBRUARY  23,2020 SUNDAY    16:23:35                                            
                                                                                
    100001                                                                      
JANUARY   12,2020 SUNDAY    10:34:30                                            
                                                                                
    100008                                                                      
JANUARY   16,2020 THURSDAY  12:22:15                                            
                                                                                

      PUR#                                                                      
----------                                                                      
PTIME                                                                           
--------------------------------------------------------------------------------
    100010                                                                      
JANUARY   20,2020 MONDAY    17:32:37                                            
                                                                                
    100013                                                                      
JANUARY   30,2020 THURSDAY  10:38:25                                            
                                                                                
    100003                                                                      
MARCH     08,2020 SUNDAY    09:30:50                                            
                                                                                

      PUR#                                                                      
----------                                                                      
PTIME                                                                           
--------------------------------------------------------------------------------
    100009                                                                      
MARCH     12,2020 THURSDAY  14:44:23                                            
                                                                                
    100006                                                                      
MARCH     12,2020 THURSDAY  15:22:10                                            
                                                                                
    100011                                                                      
MARCH     16,2020 MONDAY    16:54:40                                            
                                                                                

      PUR#                                                                      
----------                                                                      
PTIME                                                                           
--------------------------------------------------------------------------------
    100014                                                                      
MARCH     18,2020 WEDNESDAY 10:54:06                                            
                                                                                
    100005                                                                      
MARCH     20,2020 FRIDAY    13:38:55                                            
                                                                                

14 rows selected.

SQL> start query6
SQL> select eid from employees where eid in (select employees.eid from customers,employees
  2  where substr(employees.telephone#,1,3) = substr(customers.telephone#,1,3))
  3  /

EID                                                                             
---                                                                             
e01                                                                             
e02                                                                             
e03                                                                             
e04                                                                             

SQL> start query7
SQL> select CNAME from customers where CID IN ( select CID from purchases
  2  where PID not IN ( select PID from products where PNAME = 'tablet'))
  3  /

CNAME                                                                           
---------------                                                                 
Kathy                                                                           
John                                                                            
Chris                                                                           
Mike                                                                            
Connie                                                                          
Katie                                                                           
Joe                                                                             

7 rows selected.

SQL> start query8
SQL> select ename from employees
  2  where not exists
  3  (select * from purchases,products where purchases.pid=products.pid
  4  and employees.eid=purchases.eid and products.original_price>200)
  5  /

ENAME                                                                           
---------------                                                                 
Mike                                                                            

SQL> start query9
SQL> select distinct cid from purchases where PID IN ( select PID from products where original_price > 200)
  2  /

CID                                                                             
----                                                                            
c006                                                                            
c001                                                                            
c003                                                                            

SQL> start query10
SQL> select employees.eid, employees.ename from employees
  2  join purchases on purchases.eid = employees.eid
  3  join customers on purchases.cid = customers.cid
  4  where customers.visits_made > =3
  5  group by employees.eid, employees.ename
  6   having count(employees.eid) > 1
  7  /

EID ENAME                                                                       
--- ---------------                                                             
e01 Peter                                                                       
e03 Susan                                                                       

SQL> start query11
SQL> select PID,PNAME,QOH,QOH_THRESHOLD,ORIGINAL_PRICE,DISCNT_RATE
  2  from products
  3  where PID IN
  4  ( select PID from purchases where CID = 'c001' MINUS select PID from purchases where CID ='c006')
  5  /

PID  PNAME                  QOH QOH_THRESHOLD ORIGINAL_PRICE DISCNT_RATE        
---- --------------- ---------- ------------- -------------- -----------        
p003 camera                  20             5            148          .2        
p008 computer                 5             3            499          .3        

SQL> start query12
SQL> select * from purchases
  2  	where pid = ANY (select pid from purchases where cid='c006')
  3  /

      PUR# EID PID  CID         QTY PTIME     TOTAL_PRICE                       
---------- --- ---- ---- ---------- --------- -----------                       
    100010 e04 p002 c006          1 20-JAN-20      211.65                       
    100001 e01 p002 c001          1 12-JAN-20      211.65                       
    100011 e02 p004 c006         10 16-MAR-20         9.9                       
    100003 e02 p004 c002          5 08-MAR-20        4.95                       

SQL> start query13
SQL> select CNAME from customers where CID IN (select CID from purchases p
  2  join products p1 ON p.PID= p1.PID
  3  where ((p1.original_price)*p.qty - p.total_price ) > 100 )
  4  /

CNAME                                                                           
---------------                                                                 
Kathy                                                                           
Chris                                                                           

SQL> start query14
SQL> select cname from customers
  2  	 join purchases
  3  	 on customers.cid = purchases.cid
  4  	 where purchases.qty >= 1
  5  	and purchases.total_price =(select max(total_price) from purchases)
  6  /

CNAME                                                                           
---------------                                                                 
Chris                                                                           

SQL> start query15
SQL> select * from products where PID IN ( select PID from purchases group by PID having count(pid)>=2)
  2  /

PID  PNAME                  QOH QOH_THRESHOLD ORIGINAL_PRICE DISCNT_RATE        
---- --------------- ---------- ------------- -------------- -----------        
p004 pencil                 100            10            .99           0        
p002 TV                       6             5            249         .15        
p006 lamp                    10             6          19.95          .1        
p008 computer                 5             3            499          .3        

SQL> start query16
SQL> select pur# from purchases where total_price >= (select max(total_price) from purchases where cid='c006')
  2  /

      PUR#                                                                      
----------                                                                      
    100001                                                                      
    100006                                                                      
    100010                                                                      
    100012                                                                      

SQL> start query17
SQL> SELECT CUST.CID, CNAME, COUNT(DISTINCT PURC.PID) FROM CUSTOMERS CUST
  2  JOIN PURCHASES PURC
  3  ON CUST.CID=PURC.CID GROUP BY CUST.CID, CNAME ORDER BY CUST.CID
  4  /

CID  CNAME           COUNT(DISTINCTPURC.PID)                                    
---- --------------- -----------------------                                    
c001 Kathy                                 3                                    
c002 John                                  1                                    
c003 Chris                                 3                                    
c004 Mike                                  1                                    
c005 Mike                                  1                                    
c006 Connie                                2                                    
c007 Katie                                 1                                    
c008 Joe                                   1                                    

8 rows selected.

SQL> start query18
SQL> select customers.cid,customers.cname,sum(purchases.total_price)
  2  from purchases
  3  join customers
  4  on purchases.cid=customers.cid
  5  where customers.visits_made
  6  in ( select max(visits_made) from customers)
  7  group by customers.cid,customers.cname
  8  /

CID  CNAME           SUM(PURCHASES.TOTAL_PRICE)                                 
---- --------------- --------------------------                                 
c003 Chris                               752.68                                 
c001 Kathy                               679.35                                 

SQL> start query19
SQL> SELECT PROD.PNAME, SUM(QTY) FROM PRODUCTS PROD
  2  JOIN PURCHASES PURC ON PROD.PID=PURC.PID
  3   WHERE PURC.PID
  4  IN (SELECT PID FROM PURCHASES GROUP BY PID HAVING SUM(QTY) = (SELECT MAX(SUM(QTY))
  5  FROM PURCHASES GROUP BY PID)) GROUP BY PROD.PNAME
  6  /

PNAME             SUM(QTY)                                                      
--------------- ----------                                                      
pencil                  15                                                      

SQL> start query20
SQL> select customers.cname, sum(total_price)
  2  from customers join purchases
  3  on purchases.cid = customers.cid
  4  group by purchases.cid , customers.cname order by sum(total_price)
  5  desc fetch first 2 rows only
  6  /

CNAME           SUM(TOTAL_PRICE)                                                
--------------- ----------------                                                
Chris                     752.68                                                
Kathy                     679.35                                                

SQL> spool off
