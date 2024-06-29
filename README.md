# Amazon_project

# Business Problem
![](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTk_F1E606kfPJhz_ZgJiwdXRizYTvU6stp8g&s)

Welcome to the Amazon Sales Analysis project! In this project, we delve into analyzing sales
data from Amazon to extract insights and trends that can help optimize sales strategies,
understand customer behavior, and improve business operations.

## Introduction

This project focuses on analyzing a dataset containing Amazon sales records, including
information such as sales dates, customer details, product categories, and revenue figures. By
leveraging SQL queries and data analysis techniques, we aim to answer various questions and
uncover valuable insights from the dataset.

## Dataset Overview

The dataset used in this project consists of [insert number] rows of data, representing Amazon
sales transactions. Along with the sales data, the dataset includes information about customers,
products, orders, and returns. Before analysis, the dataset underwent preprocessing to handle
missing values and ensure data quality.

## Analysis Questions Resolved

During the analysis, the following key questions were addressed using SQL queries and data
analysis techniques:

## Analysis Questions Resolved
During the analysis, the following key questions were addressed using SQL queries and data
analysis techniques:

1. what are the total sales made by each customer?

   
        select 
        customer_id,
       sum(sale) as Total_sales
       from orders
        group by 1;

 ![Screenshot 2024-06-29 092012](https://github.com/tarunbiisht/Amazon_project/assets/107929505/96c1067d-5142-423b-ba58-f147b122632c)


2. How many orders were placed in each state?

       select state,
       count(order_id) as Total_orders
       from orders
       group by state;
  ![Screenshot 2024-06-29 092232](https://github.com/tarunbiisht/Amazon_project/assets/107929505/4d1a9fb8-4e5b-4d5c-ba40-d8f5e93e3e78)


3. How many unique products were sold?

       select count(distinct product_id) as Total_unique_product
       from orders
   
   ![Screenshot 2024-06-29 092410](https://github.com/tarunbiisht/Amazon_project/assets/107929505/dfb4e148-6bcd-46ab-a176-144a3ce0e14a)


4. How many returns were made for each product category.

       select  o.category,
       count(r.return_id)as no_of_returns
       from orders o
       join
       returns r on 
       o.order_id = r.order_id
       group by 1;


5. How many orders were placed in each month(2022)


        select Extract(month from order_date) as month,
       count(order_id)as total_orders
       from orders
       where 
       extract(year from order_date) =2022
       group by 1;

![Screenshot 2024-06-29 110010](https://github.com/tarunbiisht/Amazon_project/assets/107929505/d77e9949-9faf-48ad-b612-71411d402db8)


6.  Determine the top 3 products whose revenue has decreased to the previous year?
    
        with Last_year_rev as (
	
        select product_id,
        sum(sale)as Total_sale
        from orders

        where

         extract(year from order_date) =2022
           group by
          product_id
        ),

	
         Current_year_rev as (
         select product_id,
         sum(sale)as Total_sale
         from orders
        where
         extract(year from order_date) =2023
        group by
        product_id
        )
         select l.product_id,
         l.total_sale as Last_year_rev,
         c.total_sale as Current_year_rev,
         ((l.total_sale - c.total_sale)/ l.total_sale) * 100 as Rev_descrease
         from last_year_rev l
         join
         Current_year_rev c
        ON
        l.product_id= c.product_id
        order by rev_descrease desc
         limit 3;

![Screenshot 2024-06-29 110138](https://github.com/tarunbiisht/Amazon_project/assets/107929505/05bb517c-0e6c-445d-b47b-7e33e3a92fc3)

7. List all orders where the quantity sold is greater than the average quantity sold across all orders

          select * from orders
           WHERE
          quantity > (select avg(quantity) from orders);

![Screenshot 2024-06-29 110221](https://github.com/tarunbiisht/Amazon_project/assets/107929505/e06324b0-7cf4-4756-b33c-b765c6b03a6f)

8. Find out the top 5 customers who made the highest profit?


       select  o.customer_id,
       sum((o.price_per_unit - p.cogs) * o.quantity) as Total_sale
       from orders o
       join
       products p
       on o.product_id = p.product_id
       group by o.customer_id
	    order by Total_sale desc
       limit 5;

10. Find details of the top 5 products with the highest total sale, where the total sale for EACH
   products is greater than the avg sale across all products?

   
	    select p.product_id,
        p.product_name,
        sum(o.sale) as Total_sale from
        orders o
         join
        products p
         on
        o.product_id= p.product_id
        group by p.product_id,
		  p.product_name
        HAVING
        sum(o.sale) > (select sum(sale)/ count(distinct product_id) from order
        order by total_sale DESC
        limit 5;

    

## Entity-Relationship Diagram (ERD)
![ERD_Amazon](https://github.com/tarunbiisht/Amazon_project/assets/107929505/2f510a22-a677-42cd-91d0-a17c1dcb4fd1)


An Entity-Relationship Diagram (ERD) has been created to visualize the relationships between
the tables in the dataset. This diagram provides a clear understanding of the data structure and
helps in identifying key entities and their attributes.

## Getting Started
To replicate the analysis or explore the dataset further, follow these steps:

1. Clone the repository to your local machine.
2. Ensure you have a SQL environment set up to execute queries.
3. Load the provided dataset into your SQL database.

4. Execute the SQL queries provided in the repository to analyze the data and derive insights.
5. Customize the analysis or queries as needed for your specific objectives.

## Conclusion

Through this project, we aim to provide valuable insights into Amazon sales trends, customer
preferences, and other factors influencing e-commerce operations. By analyzing the dataset
and addressing the key questions, we hope to assist stakeholders in making informed decisions
and optimizing their sales strategies.

Feel free to explore the repository and contribute to further analysis or enhancements!

## Notice:
All customer names and data used in this project are computer-generated using AI and random
functions. They do not represent real data associated with Amazon or any other entity. This
project is solely for learning and educational purposes, and any resemblance to actual persons,
businesses, or events is purely coincidental.
