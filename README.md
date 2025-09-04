# sales-performance-analysis
This project focuses on analyzing sales data to evaluate performance, identify customer trends, and uncover opportunities for revenue growth. The analysis was performed using SQL for data cleaning and querying, and Excel for KPI tracking and interactive dashboard creation.
select database ();
use blinkit;
create table Sales (
OrderID varchar(15),
OrderDate date,
Customer_ID varchar(10),
Region varchar(10),
Product varchar(25),
Quantity int,
UnitPrice int
)

select * from sales;

set sql_safe_updates=0;

UPDATE sales
SET region = 'Unknown'
WHERE region IS NULL OR TRIM(region) = ''; 

update sales
set Product=
case 
when product ='Lap' then 'Laptop'
when product in ('Tab','Tabet') then 'Tablet'
when product in ('sp','Smartph') then  'Smartphone'
when product ='Headset' then 'Headphones'
else product
end;

alter table sales
add column Total_sales int;

update sales 
set Total_sales = Quantity * UnitPrice;

select SUM(Total_sales) AS total_revenue FROM sales; 

select product,
 sum(Total_sales) as Revenue_by_product
 from sales
 group by product
 order by Revenue_by_product
 limit 3;

select region,
round(sum(total_sales) , 2) Regional_Revenue,
round((sum(total_sales)*100) /(select sum(total_sales) from sales),2) as Regional_Revenue_Percentage
from sales
group by region
order by Regional_Revenue Desc;

select product,
round(sum(total_sales),2) as Product_Revenue,
round(sum(total_sales)*100/(select sum(total_sales) from sales),2) as Product_Revenue_Percentage
from sales
group by product
order by Product_Revenue_Percentage desc;
