# w3_python_mysql

to produce the same visuals as I did in the Jupyter notebook, please use these steps:

1. download the SQL script for creating the w3 database from https://github.com/AndrejPHP/w3schools-database/blob/master/w3schools.sql, import into your own schema.
2. after that is done, create this table, add constraint, and add the stored procedures below:
/*
---create the city_points table

create table city_points

(
	city  varchar(255),
    latitude float,
    longitude float,

    primary key (city)
    
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

---add the constraint

ALTER TABLE customers
ADD FOREIGN KEY (City) REFERENCES city_points(city);:
  
---make these simple updates so it doesn't need to be done everytime in Jupyter Notebook:
  
update customers
set city = 'Tsawwassen'
where city = 'Tsawassen'


update customers
set country = 'United Kingdom'
where country = 'UK';


update customers
set country = 'United States of America'
where country = 'USA';
 
--- stored procedure for fetching, joining orders values
  
  CREATE DEFINER=`root`@`localhost` PROCEDURE `customer_orders`()
BEGIN
select  
CustomerName,Country,orders.OrderID,OrderDate,products.ProductID, ProductName,Quantity,Price,CategoryName,City,products.CategoryID
from customers
join orders on customers.customerid = orders.customerID
join order_details on orders.OrderID = order_details.OrderID
join products on order_details.ProductID = products.ProductID
join categories on products.CategoryID = categories.CategoryID;

END

--- stored procedure for fetching, joining emoloyees values

CREATE DEFINER=`root`@`localhost` PROCEDURE `employee_join`()
BEGIN

SELECT 
orders.CustomerID,
orders.OrderID,
order_details.ProductID,
orders.EmployeeID,
employees.FirstName

FROM w3.employees

join orders on employees.EmployeeID = orders.EmployeeID
join order_details on orders.OrderID = order_details.OrderID;


END

--- stored procedure for merging points with city names

CREATE DEFINER=`root`@`localhost` PROCEDURE `get_points`()
BEGIN

SELECT city_points.city, count(CustomerID) as 'density',latitude,longitude FROM city_points
join customers on city_points.city = customers.City
where exists 
    (select orders.CustomerID
      from orders where orders.CustomerID = customers.CustomerID)
group by city_points.city;

END
 
--- stored procedure for inserting points to city_points table

CREATE DEFINER=`root`@`localhost` PROCEDURE `w3_insert_points`(
    IN city varchar(255),
    IN latitude float,
    IN longitude float
    
)
BEGIN
insert into w3.city_points (city, latitude,longitude) values (city, latitude,longitude);

END

*/

3. boot up w3_get_points.ipynb, running the script to get the geographic points of cities and insert them back into city_points table.
4. boot up w3_mysql.ipynb and run as usual. it should work correctly as long as the stored procedures are named in the same way.
