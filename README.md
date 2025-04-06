# CELOP-CANDY-Analysis

This project was done with the aim to solve critical problems affecting Celop Candy.
Through proper analysis of the data obtained i was able to uncover patterns and discover useful 
insights that can provide solution to the problem faced by Celop Candy.

![CELOP CANDY](https://github.com/user-attachments/assets/793b2450-82b8-4b88-9393-e9cb93d819ee)

# Tools
- Microsoft Excel
- MySQL

# Data Cleaning / Preparation
- Data integrity
- Removing duplicates
- Data standardization
- Table alteration
- Table joining

# Exploratory Data Analysis
- What region do we have the highest profit made?
- Which division produces most sales?
- The most used method of shipping ?
- Factory with highest production ?
- Top profitable products ?

# Data Analysis
## Here are codes written by me to prepare and clean the data
- duplicating candy_sales table
```sql
create table  candy_sale_dup
like candy_sales;
insert  candy_sale_dup
select * from candy_sales;
select * from  candy_sale_dup;
```
- duplicating candy_product table
```sql
create table  candy_products_dup
like candy_products;
insert    candy_products_dup
select * from candy_products;
select * from  candy_products_dup;
```
- removing duplicates
```sql
with cte as (
select *,
row_number() over(
partition by `Order ID` order by `Order ID`) as row_num
from candy_sale_dup
) 
delete from cte where row_num >1;
```
- create table
```sql
CREATE TABLE `candy_sale_dup2` (
  `Row ID` int DEFAULT NULL,
  `Order ID` text,
  `Order Date` text,
  `Ship Date` text,
  `Ship Mode` text,
  `Customer ID` int DEFAULT NULL,
  `Country/Region` text,
  `City` text,
  `State/Province` text,
  `Postal Code` int DEFAULT NULL,
  `Division` text,
  `Region` text,
  `Product ID` text,
  `Product Name` text,
  `Sales` double DEFAULT NULL,
  `Units` int DEFAULT NULL,
  `Gross Profit` double DEFAULT NULL,
  `Cost` double DEFAULT NULL,
  `row_num` int
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;
```
-

```sql
select * from candy_sale_dup2;

insert into candy_sale_dup2
select *,
row_number() over(
partition by `Order ID` order by `Order ID`) as row_num
from candy_sale_dup;
```
```sql
select * from candy_sale_dup2
 where row_num > 1;
 
delete from candy_sale_dup2
 where row_num > 1;
 select * from candy_sale_dup2;
```
- standardize
```sql
select * from candy_sale_dup2;
     select `Ship Date` from candy_sale_dup2;

  select `Ship Date`,
 str_to_date(`Ship Date`, '%Y-%m-%d') 
 FROM candy_sale_dup2;

update candy_sale_dup2
set `Ship Date`= str_to_date(`Ship Date`, '%Y-%m-%d') ;

 select `Order Date`,
 str_to_date(`Order Date`, '%Y-%m-%d') 
 FROM candy_sale_dup2;

update candy_sale_dup2
set `Order Date`= str_to_date(`Order Date`, '%Y-%m-%d') ;

  select * from candy_sale_dup2;
```
- ALTERATION
```sql
ALTER  table candy_sale_dup2
modify column `Order Date` date;

ALTER  table candy_sale_dup2
modify column `Ship Date` date;

 alter table candy_sale_dup2
 drop column row_num;
```
- REMOVING NULLS
```sql
 select*
 FROM candy_sale_dup2
 where Sales is null 
 and  `Row ID` is null;
```
- joining tables
```sql
 select* from candy_products_dup;
select* FROM candy_sale_dup2;

select candy_sale_dup2.*, candy_products_dup.Factory
from candy_sale_dup2 
join candy_products_dup
on candy_sale_dup2 .`Product ID`=candy_products_dup.`Product ID`;
```
- create new table from joined
```sql
create table main_candy_sales as
select candy_sale_dup2.*, candy_products_dup.Factory
from candy_sale_dup2 
join candy_products_dup
on candy_sale_dup2 .`Product ID`=candy_products_dup.`Product ID`;
 
 
select * from main_candy_sales;
```

### After thorough data cleaning and preparation on Mysql, i performed the analysis on the cleaned data using microsoft excel .
I also made use of MS Excel to visualize the data to an understandable form to make it easy for decision makers and stakeholders.
Each questions asked by the firm decisionmakers was rightly answered in this analysis, by walking them through every bit of my analysis i was able to provide solutions to problems faced by Celop Candy.

















