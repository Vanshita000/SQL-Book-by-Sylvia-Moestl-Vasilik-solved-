# SQL-Book-by-Sylvia-Moestl-Vasilik-solved-
I tried to solve all the questions from the book by Sylvia Moestl Vasilik on SQL SERVER STUDIO
--Hi my name is Vanshita and i have tried to solve the questions in a book called 
--SQL Practice Problems 57 beginning, intermediate, and advanced challenges for you to solve using a "learn
--by-doing" approach
-- with doing only one sort of thing daily in office we sometimes forget to revise the basics so 
-- to regain some of the basic pointers from my early days of learning sql I tried to solve all the questions in this amazing book
-- i have started from question no. 10 as I found the question before it quite simple and straight forward

--10
select firstname, lastname, title, birthdate
from employees 
order by birthdate asc
--11
select firstname, lastname, title, CAST(birthdate AS DATE ) AS DATES
from employees 
order by birthdate asc
--12
SELECT firstname, lastname, concat(firstname, ' ' , lastname) as fullname
from Employees
--13
select  OrderID, ProductID,UnitPrice, Quantity, (Unitprice*quantity) as totalprice
from OrderDetails
order by OrderID,ProductID
--14
select count(customerid) from customers
--15
select MIN(orderdate) from orders
--16 
select country from customers
group by country
--17
select contacttitle, count(contacttitle) as total
from Customers
group by contacttitle
order by contacttitle desc
--18
select * from Suppliers
select * from Products
select p.productid, p.productname, s.companyname
from Products p 
join Suppliers s
on P.SupplierID=s.SupplierID
order by productID
--19
select * from products
select * from customers
select  o.OrderID, CAST(o.OrderDate as date) as orderdate, s.CompanyName as shipper
from Orders o join shippers s 
on o.shipvia=s.shipperid
where o.orderid < 10300
order by orderid
-- 20 Intermediate 
select c.categoryname , count(p.productname) as total
from Categories c join Products p
on c.CategoryID = p.CategoryID
group by categoryname
order by total desc
--21
select country, city, count(customerid) as totalcustomer
from customers
group by country, city
order by totalcustomer desc
--22
select * from Products
select productid, productname ,unitsinstock, reorderlevel 
from products
where unitsinstock < ReorderLevel
order by productid
--23
select productid, productname ,unitsinstock,unitsonorder, reorderlevel , discontinued
from products
where unitsinstock+unitsonorder<= reorderlevel 
AND discontinued = 0
--24----->>>> Learn this one 
SELECT CustomerID, CompanyName, Region
FROM Customers
ORDER BY 
    CASE 
        WHEN Region IS NULL THEN 1 
        ELSE 0 
    END, Region, CustomerID;
--25
Select ShipCountry, avg(freight) as AverageFreight
 From Orders
 Group By ShipCountry
 Order By AverageFreight desc
 --26
select * from Orders
select top 3  shipcountry, avg(freight) as average
from orders
where year(orderdate) = 2015
group by shipcountry
order by average desc
--27
 select * from orders order by OrderDate
 --28
select max(orderdate) as latest from orders
 select top 3  shipcountry, avg(freight) as average
from orders
 WHERE OrderDate >= DATEADD(month,-12, ( Select Max(OrderDate) from Orders))
group by shipcountry
order by average desc
--29
select * from products
select * from customers
select * from orders
select * from OrderDetails

select e.employeeid , e.lastname, o.orderid, p.productname, q.quantity
from Employees e join Orders o ON e.employeeid = o.employeeid
join  OrderDetails q on o.OrderID = q.OrderID
join products p on P.ProductID = q.ProductID
order by o.Orderid, p.productid

--30
select customerid from customers
where not exists (select customerid from orders where Orders.CustomerID = Customers.CustomerID )

--32
select c.customerid, c.companyname, o.orderid, ROUND(sum(q.unitprice*q.quantity-q.discount),0) as totalvalue
from customers c join orders o  On c.customerid=o.customerid
join OrderDetails q on o.OrderID = q.OrderID
where year(orderdate)=2016
group by c.customerid, c.companyname, o.orderid
having ROUND(sum(q.unitprice*q.quantity-q.discount),0)  >= 10000
order by totalvalue desc
--33
select c.customerid,c.companyname , ROUND(sum(q.unitprice*q.quantity-q.discount),0) as totalvalue
from customers c join orders o  On c.customerid=o.customerid
join OrderDetails q on o.OrderID = q.OrderID
where year(orderdate)=2016
group by c.customerid, c.companyname
having ROUND(sum(q.unitprice*q.quantity-q.discount),0)  >= 15000
order by totalvalue desc
--34
select *, unitprice*quantity as totalvalue from OrderDetails
WHERE not discount = 0
order by discount desc
--incomplete hard to understand the language 
--35
select * from orders
select employeeid, orderid, orderdate from orders
where OrderDate = EOMONTH(OrderDate ) Order by EmployeeID ,OrderId
--36
select * from orderdetails
select top 10 
orderid, count(orderid) as c
from OrderDetails
group by orderid 
order by c desc
--37
 Select top 2 percent
    OrderID
 From Orders
 Order By NewID()
 --38
 select * from orderdetails
 select OrderID from orderdetails 
 where Quantity >= 60 
 group by orderid, quantity
  Having Count(*) > 1


--41
select * from  orders
select * from employees
 Select OrderID
 , cast(OrderDate as date) as orderdate
 , cast(RequiredDate as date) as RequiredDate
 , cast(ShippedDate as date) as ShippedDate
 From Orders
 Where
    RequiredDate <= ShippedDate
--42
 Select e.employeeid, e.lastname, count(*) as totallate
 from orders o join employees e 
 on e.EmployeeID = o.employeeid
 where requireddate <= shippeddate
 group by e.employeeid, e.lastname
 order by totallate desc

 --43
 with cte as 
 ( select employeeid,count(orderid) as totalorders from orders group by employeeid )
 Select e.employeeid, e.lastname, count(*) as totallate, cte.totalorders
 from orders o join employees e 
 on e.EmployeeID = o.employeeid
 join cte on e.employeeid = cte.employeeid
 where requireddate <= shippeddate
 group by e.employeeid, e.lastname, cte.totalorders
 order by EmployeeID

 --44 and 45
 with cte as 
 ( select employeeid,count(orderid) as totalorders from orders group by employeeid )
 Select e.employeeid, e.lastname, count(o.orderid) as totallate,
 COALESCE(cte.totalorders, 0) AS totalorders
 from orders o left join employees e 
 on e.EmployeeID = o.employeeid
join cte on e.employeeid = cte.employeeid
 where requireddate <= shippeddate
 group by e.employeeid, e.lastname, cte.totalorders
 order by EmployeeID

 --46

with totalorders as 
                   (select employeeid, count(orderid) as allorders 
				   from orders 
				   group by employeeid) , 
totallateorder as ( Select employeeid, count(*) as totallate
                    from orders
                    where requireddate <= shippeddate
                    group by employeeid )
 select e.employeeid, e.lastname, totalorders.allorders , 
coalesce(totallateorder.totallate,0) as totallate,
 (cast(coalesce(totallateorder.totallate,0)as float)
 /cast(totalorders.allorders as float) ) as percentageof
 from orders o left join employees e on o.employeeid=e.employeeid
 left join totalorders on totalorders.employeeid=e.employeeid
 join totallateorder  on totallateorder.employeeid =e.employeeid
 group by e.employeeid, e.lastname, totalorders.allorders ,
 totallateorder.totallate
 order by e.employeeid

--47
with totalorders as 
                   (select employeeid, count(orderid) as allorders 
				   from orders 
				   group by employeeid) , 
totallateorder as ( Select employeeid, count(*) as totallate
                    from orders
                    where requireddate <= shippeddate
                    group by employeeid )
 select e.employeeid, e.lastname, totalorders.allorders , 
                  coalesce(totallateorder.totallate,0) as totallate,
                  cast((cast(coalesce(totallateorder.totallate,0)as decimal(10,2))
                  /cast(totalorders.allorders  as decimal(10,2)) ) as decimal (10,2))as percentageof
 from orders o left join employees e on o.employeeid=e.employeeid
 left join totalorders on totalorders.employeeid=e.employeeid
 join totallateorder  on totallateorder.employeeid =e.employeeid
 group by e.employeeid, e.lastname, totalorders.allorders ,
 totallateorder.totallate
 order by e.employeeid
