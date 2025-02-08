# sales_data sql script

select * from assessment_dataset_in;
--total transactions
SELECT COUNT(*) AS total_transactions 
FROM assessment_dataset_in;
-- total revenue
SELECT SUM(assessment_dataset_in.TransactionAmount  * quantity) AS total_revenue 
FROM assessment_dataset_in;
--total quantity
SELECT SUM(quantity) AS total_quantity_sold 
FROM assessment_dataset_in;
-- average quantity per transaction
SELECT SUM(quantity) / COUNT(DISTINCT assessment_dataset_in.TransactionID ) AS avg_quantity_per_transaction 
FROM assessment_dataset_in;
--average transaction 
SELECT SUM(assessment_dataset_in.TransactionAmount  * quantity) / COUNT(DISTINCT TransactionID) AS avg_transaction_value 
FROM assessment_dataset_in;
--best selling products
SELECT ProductName ,
       SUM(quantity) AS total_units_sold, 
       SUM(TransactionAmount) AS total_revenue 
FROM assessment_dataset_in 
WHERE ProductName IS NOT NULL 
GROUP BY ProductName
ORDER BY total_units_sold DESC 
Limit 5;
--gender
SELECT CustomerGender , 
       COUNT(DISTINCT CustomerID) AS total_customers, 
       SUM(TransactionAmount) AS total_revenue 
FROM assessment_dataset_in
GROUP BY CustomerGender;
--average customer feedback score
SELECT ROUND(AVG(FeedbackScore), 2) AS avg_feedback_score 
FROM assessment_dataset_in;
--shipping cost analysis
SELECT ROUND(AVG(ShippingCost), 2) AS avg_shipping_cost, 
       SUM(ShippingCost) AS total_shipping_cost 
FROM assessment_dataset_in  ;
--average delivery time per region
SELECT region, 
       ROUND(AVG(DeliveryTimeDays), 2) AS avg_delivery_time 
FROM assessment_dataset_in 
GROUP BY region 
ORDER BY DeliveryTimeDays;
--highest revenue by  city
SELECT city, 
       SUM(TransactionAmount) AS total_revenue 
FROM assessment_dataset_in 
GROUP BY StoreType, city 
ORDER BY total_revenue DESC;
--highes returns
SELECT city, ProductName,
       COUNT(*) AS total_returns 
FROM assessment_dataset_in
GROUP BY city ,assessment_dataset_in.ProductName 
ORDER BY total_returns DESC,ProductName desc
Limit 10;
