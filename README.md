# Walmart-Sales-Analysis
CREATE DATABASE walmart_sales;
USE walmart_sales;
SELECT * FROM walmart_sales.walmartsalesdata;

-- --------------------------------FEATURE ENGINEERING------------------------------------------
-- --------------------------ADDING NEW COLUMN NAMED TIME OF DAY---------------------------------
USE walmart_sales;

ALTER TABLE walmart_sales.walmartsalesdata
ADD COLUMN time_day VARCHAR(50); 

UPDATE walmart_sales.walmartsalesdata
SET time_day =(    (CASE
           WHEN time BETWEEN "00:00:00" AND "12:00:00" THEN "Morning"
           WHEN time BETWEEN "12:01:00" AND "16:00:00" THEN "Afternoon"
           ELSE "Evening"
       END
       )
);

-- -----------------------------ADDING NEW COLUMN NAMED DAY_NAME---------------------------------

ALTER TABLE walmart_sales.walmartsalesdata
ADD COLUMN day_name VARCHAR(10);
UPDATE walmart_sales.walmartsalesdata
SET day_name=dayname(date);

SELECT * FROM walmart_sales.walmartsalesdata;

-- ------------------------------ADDING NEW COLUMN NAMED MONTH NAME-----------------------------
ALTER TABLE walmart_sales.walmartsalesdata
ADD COLUMN month_name VARCHAR(30);

UPDATE walmart_sales.walmartsalesdata
SET month_name = monthname(date);


USE walmart_sales;
-- ----------------------------------Generic Questions--------------------------------------------

-- How many unique cities does the data have?
SELECT DISTINCT
    city
FROM
    walmart_sales.walmartsalesdata;
    
-- What is the most common payment method?
SELECT DISTINCT
	payment
FROM
	walmart_sales.walmartsalesdata;
    

-- What is the most selling product line?
SELECT 
	DISTINCT `product line`,
    COUNT(`Product line`) AS count_p
FROM
	walmart_sales.walmartsalesdata
GROUP BY `product line`
ORDER BY count_p DESC;

-- What is the total revenue by month?
SELECT
	month_name,
    SUM(total) AS total_revenue
FROM
	walmart_sales.walmartsalesdata
GROUP BY month_name
ORDER BY total_revenue DESC;

-- What month had the largest COGS?
SELECT
	 month_name,
     SUM(cogs) AS total_cogs
FROM
	walmart_sales.walmartsalesdata
GROUP BY month_name
ORDER BY total_cogs DESC;

-- What product line had the largest revenue?
SELECT `product line`,
		SUM(total) AS total_revenue
FROM walmart_sales.walmartsalesdata
GROUP BY `Product line`
ORDER BY total_revenue DESC;

-- What is the city with the largest revenue?
SELECT city,
		SUM(total) AS revenue_total
FROM walmart_sales.walmartsalesdata
GROUP BY city
ORDER  BY revenue_total DESC;

-- What product line had the largest VAT?
SELECT * FROM walmart_sales.walmartsalesdata;
SELECT `product line`,
		AVG(`Tax 5%`) AS VAT_avg
FROM walmart_sales.walmartsalesdata
GROUP BY `product line`
ORDER BY VAT_avg DESC;

-- Which branch sold more products than average product sold?
select* from walmart_sales.walmartsalesdata;
SELECT branch,
		SUM(Quantity) AS qty
FROM walmart_sales.walmartsalesdata
GROUP BY branch
HAVING qty>(SELECT AVG(Quantity) FROM walmart_sales.walmartsalesdata)
;

-- What is the most common product line by gender?
SELECT gender,
	   `product line`,
       COUNT(gender) AS Count_of_Gender
FROM walmart_sales.walmartsalesdata
GROUP BY gender,`product line`
ORDER BY gender,Count_of_Gender DESC;

-- What is the average rating of each product line?
SELECT `product line`,
		AVG(Rating) AS Avg_Rating
FROM walmart_sales.walmartsalesdata
GROUP BY `product line`
ORDER BY Avg_Rating DESC;

-- Number of sales made in each time of the day per weekday
SELECT 
		time_day,
        COUNT(*) AS count
FROM
	walmartsalesdata
GROUP BY time_day
ORDER BY count DESC;

-- Which of the customer types brings the most revenue?
SELECT * from walmart_sales.walmartsalesdata;
SELECT 
	`Customer type`,
    SUM(total) AS Total
FROM walmart_sales.walmartsalesdata
GROUP BY `Customer type`
ORDER BY Total DESC;

-- Which city has the largest tax percent/ VAT (Value Added Tax)?
SELECT 
	city,
    AVG(`Tax 5%`) AS Avg
FROM walmartsalesdata
GROUP BY city
ORDER BY Avg DESC;

-- Which customer type pays the most in VAT?
SELECT
	`customer type`,
	AVG(`Tax 5%`) AS Avg
FROM walmartsalesdata
GROUP BY `Customer type`
ORDER BY Avg DESC;

-- How many unique customer types does the data have?
SELECT 
	DISTINCT `customer type`
FROM walmartsalesdata;

-- How many unique payment methods does the data have?
SELECT 
	DISTINCT Payment
FROM walmartsalesdata;

-- What is the most common customer type?
SELECT 
	`customer type`,
    COUNT(*) AS Total 
FROM walmartsalesdata
GROUP BY `Customer type`;

-- What is the gender of most of the customers?
SELECT gender,
	   COUNT(*) AS Total_count
FROM walmartsalesdata 
GROUP BY gender
ORDER BY gender;

-- What is the gender distribution per branch?
SELECT 
    gender, COUNT(*) AS gender_count, branch
FROM
    walmartsalesdata
WHERE
    Branch = 'B'
GROUP BY gender;

-- Which time of the day do customers give most ratings?
SELECT time_day,
	   AVG(Rating) AS avg_rating
FROM walmartsalesdata
GROUP BY time_day
ORDER BY  avg_rating DESC;

-- Which time of the day do customers give most ratings per branch?
SELECT time_day,
		branch,
        AVG(rating) as rat
FROM walmartsalesdata
WHERE Branch="C"
GROUP BY time_day,branch
ORDER BY rat,branch,time_day DESC;

-- Which day of the week has the best avg ratings?
SELECT day_name,
		AVG(Rating) AS a_rating
FROM walmartsalesdata
GROUP BY day_name
ORDER BY day_name 
;

-- Which day of the week has the best average ratings per branch?
SELECT day_name,
	AVG(Rating) AS rating
FROM walmartsalesdata
WHERE Branch="A"
GROUP BY day_name
ORDER BY rating;



