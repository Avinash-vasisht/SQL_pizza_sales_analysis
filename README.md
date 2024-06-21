# SQL_pizza_sales_analysis
This project aims to provide a comprehensive analysis of pizza sales data for a fictional pizza restaurant. Using SQL and data analytics techniques, I explored various aspects of sales trends, customer preferences, and revenue generation.

Project overview:-
Analyzed monthly sales patterns.
Explored cumulative revenue growth over time.
Customer Insights
Identified peak sales times.
Determined popular pizza types based on sales.
Revenue Analysis
Identified top-performing products driving the most revenue.

Tools and Techniques:-
SQL for data querying and analysis.
CSV files imported into the database.
ER Diagram created to understand database relationships.

How to use:-
Import the provided CSV files into your database.
Use the SQL scripts provided to perform analysis.
Review the ER Diagram to understand data relationships.

Feel free to explore and contribute!

Here are the project questions and queries:-
Q1. Retrieve the total no. of orders placed.
SELECT	
           COUNT(ORDER_ID) AS "total orders"
FROM ORDERS;

Q.2 Calculate the total revenue generated from pizza sales.
SELECT	 
           SUM(PIZZAS.PRICE * ORDER_DETAILS.QUANTITY) AS "Revenue"
FROM 
          PIZZAS
JOIN ORDER_DETAILS ON PIZZAS.PIZZA_ID = ORDER_DETAILS.PIZZA_ID;

Q3. Identify the highest-priced pizza.
SELECT	
           NAME, 
           MAX(PRICE) AS "Max price" 
FROM         
          PIZZAS  	
          JOIN PIZZA_TYPES ON PIZZAS.PIZZA_TYPE_ID = PIZZA_TYPES.PIZZA_TYPE_ID
GROUP BY 
          NAME
ORDER BY 
          2 DESC
LIMIT
         1; 

Q4. Identify the most common pizza size ordered.
SELECT	
          PIZZAS.SIZE,
          COUNT(ORDER_DETAILS.ORDER_DETAILS_ID) 	
FROM
         PIZZAS	
        JOIN ORDER_DETAILS ON PIZZAS.PIZZA_ID = ORDER_DETAILS.PIZZA_ID
GROUP BY	
        PIZZAS.SIZE
ORDER BY	
       2 DESC;	

Q5. List the top 5 most ordered pizza types along with their quantities.
SELECT	
          SUM(ORDER_DETAILS.QUANTITY) AS "Total quantity",                    
          PIZZA_TYPES.name as "Pizza"
FROM	
         PIZZAS	
         JOIN PIZZA_TYPES ON PIZZA_TYPES.PIZZA_TYPE_ID = PIZZAS.PIZZA_TYPE_ID                          
         JOIN ORDER_DETAILS ON PIZZAS.PIZZA_ID = ORDER_DETAILS.PIZZA_ID 	
GROUP BY	
       PIZZA_TYPES.name 
ORDER BY	
       1 DESC 
limit 5;

Q6. Join the necessary tables to find the total quantity of each pizza category.
SELECT	
          SUM(ORDER_DETAILS.QUANTITY) AS "Total quantity", 
          PIZZA_TYPES.CATEGORY 	
FROM	
       PIZZAS	
       JOIN PIZZA_TYPES ON PIZZA_TYPES.PIZZA_TYPE_ID = PIZZAS.PIZZA_TYPE_ID       
       JOIN ORDER_DETAILS ON PIZZAS.PIZZA_ID = ORDER_DETAILS.PIZZA_ID
GROUP BY	
      PIZZA_TYPES.CATEGORY
ORDER BY	
     1 DESC;

Q7. Determine the distribution of orders by hour of the day.
SELECT	
          COUNT(ORDER_DETAILS.ORDER_ID) AS "Count",	
          EXTRACT(	      
                 HOUR
                 FROM	        
	     ORDER_TIME
          ) AS "Order hour"
FROM	
         ORDER_DETAILS	
         JOIN ORDERS ON ORDER_DETAILS.ORDER_ID = ORDERS.ORDER_ID 
GROUP BY	
       "Order hour"
 ORDER BY	
      "Count" DESC;
     
Q8. Join relevant tables to find the category-wise distribution of pizzas.
SELECT	
        CATEGORY,	
        COUNT(NAME)
FROM	
       PIZZA_TYPES 
GROUP BY	
      CATEGORY;

Q9. Group the orders by date and calculate the average number of pizzas ordered per day.
SELECT	
         TO_CHAR(AVG(QUANTITY), '999,999.00') AS "Avg pizza order"
 FROM	
        (		
         SELECT			
                 SUM(QUANTITY) AS "quantity",			
                 ORDER_DATE		
         FROM			
                ORDER_DETAILS			
                JOIN ORDERS ON ORDER_DETAILS.ORDER_ID = ORDERS.ORDER_ID
        GROUP BY 	
               ORDER_DATE	)
 AS "order quantity";

 Q10. Determine the top 3 most ordered pizza types based on revenue.
SELECT	
         PIZZA_TYPES.NAME,	
         SUM(PIZZAS.PRICE * ORDER_DETAILS.QUANTITY) AS "Revenue" 
FROM	
         PIZZA_TYPES	
         JOIN PIZZAS ON PIZZA_TYPES.PIZZA_TYPE_ID = PIZZAS.PIZZA_TYPE_ID	
         JOIN ORDER_DETAILS ON PIZZAS.PIZZA_ID = ORDER_DETAILS.PIZZA_ID 
GROUP BY	
        PIZZA_TYPES.NAME 
ORDER BY	
       "Revenue" DESC
LIMIT	
3;

Q11. Analyze the cumulative revenue generated over time.
SELECT     
         order_date,     
         SUM(Revenue) OVER (ORDER BY order_date) AS Cum_revenue 
FROM     
       (    
        SELECT         
                orders.order_date,       
                SUM(order_details.quantity * pizzas.price) AS Revenue   
        FROM         
               order_details         
        JOIN       
               pizzas   
        ON         
              order_details.pizza_id = pizzas.pizza_id   
       JOIN         
             orders    
       ON         
            orders.order_id = order_details.order_id   
      GROUP BY         
            orders.order_date) AS Sales 
 ORDER BY    
           order_date;






         


