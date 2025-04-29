# -Sales-Trend-Analysis-Using-Aggregations 
-- Create a temporary table to extract month and year from Order Date
WITH monthly_data AS (
  SELECT 
    STR_TO_DATE(`Order Date`, '%m/%d/%y') AS order_date,
    Sales,
    `Order ID`
  FROM sales_data
),

-- Calculate monthly metrics
monthly_metrics AS (
  SELECT 
    DATE_FORMAT(order_date, '%Y-%m') AS month,
    COUNT(DISTINCT `Order ID`) AS order_count,
    ROUND(SUM(Sales), 2) AS total_revenue
  FROM monthly_data
  GROUP BY DATE_FORMAT(order_date, '%Y-%m')
  ORDER BY month
)

-- Final result with formatted output
SELECT 
  month,
  order_count,
  total_revenue,
  ROUND(total_revenue / order_count, 2) AS avg_revenue_per_order
FROM monthly_metrics;
