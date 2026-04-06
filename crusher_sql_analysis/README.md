SELECT * FROM sales_data_staging LIMIT 10;
--top village by profit
ALTER TABLE sales_data_staging
ALTER COLUMN quantity_m3 TYPE numeric USING quantity_m3::numeric;

ALTER TABLE sales_data_staging
ALTER COLUMN rate_per_m3 TYPE numeric USING rate_per_m3::numeric;

ALTER TABLE sales_data_staging
ALTER COLUMN total_amount TYPE numeric USING total_amount::numeric;

ALTER TABLE sales_data_staging
ALTER COLUMN profit TYPE numeric USING profit::numeric;

ALTER TABLE sales_data_staging
ALTER COLUMN distance_km TYPE numeric USING distance_km::numeric;

ALTER TABLE sales_data_staging
ALTER COLUMN cost_per_m3 TYPE numeric USING cost_per_m3::numeric;

ALTER TABLE sales_data_staging
ALTER COLUMN total_transport_cost TYPE numeric USING total_transport_cost::numeric;

ALTER TABLE sales_data_staging
ALTER COLUMN total_cost TYPE numeric USING total_cost::numeric;

ALTER TABLE sales_data_staging
ALTER COLUMN total_profit TYPE numeric USING total_profit::numeric;


--TOTAL SALES
SELECT SUM(total_amount) AS  total_sales FROM sales_data_staging;

--TOTAL PROFIT
SELECT SUM( total_profit) AS Total_profit FROM sales_data_staging;

--TOP 5 CUSTOMERS BY SALES
SELECT customer_name, SUM(total_amount) as total_sales
FROM sales_data_staging
GROUP BY customer_name
ORDER BY total_sales DESC
LIMIT 5; 

--top village by top customer

WITH ranked AS (
    SELECT customer_name, village,
           SUM(total_amount) AS total_sales,
           RANK() OVER (PARTITION BY customer_name ORDER BY SUM(total_amount) DESC) AS rnk
    FROM sales_data_staging
    GROUP BY customer_name, village
)
SELECT customer_name, village, total_sales
FROM ranked
WHERE rnk = 1;


--TOP VILLAGES BY profit
SELECT village, SUM(total_profit) as Total_profit
FROM sales_data_staging
GROUP BY village
order by Total_profit DESC
LIMIT 5;

--payment mode analysis
SELECT payment_mode, COUNT(*) AS total_orders, SUM(total_amount) AS total_sales
FROM sales_data_staging
GROUP BY payment_mode;

--PRODUCT  WISE PROFIT

SELECT product_type, SUM(profit) AS Total_profit
FROM sales_data_staging
GROUP BY product_type
ORDER BY Total_profit DESC;

--average distance and rate per village
SELECT village, AVG(distance_km) AS avg_distance, AVG(rate_per_m3) AS avg_rate
FROM sales_data_staging
GROUP BY village 
limit 5;

-- MONTHLY SALES TREND
SELECT month_year, SUM(total_amount) AS total_sales
FROM sales_data_staging
GROUP BY month_year
ORDER BY month_year asc;


--top month  profit wise
SELECT month_year, SUM(total_profit) as Total_profit
FROM  sales_data_staging
group by month_year
order by  Total_profit desc;
















