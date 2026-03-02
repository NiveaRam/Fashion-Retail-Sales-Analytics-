FASHION RETAIL PERFORMANCE

Retail stakeholders often struggle to identify which factors—brand, category, or season most impact the bottom line after accounting for total revenue and discount rates. The goal of this project is:
total discount and sum of original price by category,sum of net revenue by month and year,unit sold by return reason,sum of net revenue by category,total revenue by brand,total revenue by size,return reason count by season and category,revenue MTD by color.The fashion industry is highly competitive, with customer preferences shifting rapidly. Retailers must balance pricing, discounts, and product quality while minimizing returns. This dashboard leverages transactional data to: Track total revenue.


DATASET OVERVIEW

Rows: 2176 and Columns: 14 fields including product_id,category,brand,season,size,color,original_price,markdown_percentage,current_price,purchase_date,stock_quantity,Customer_Rating,is_returned,Return Reason

Data Cleaning & Transformation

The raw dataset underwent a rigorous ETL (Extract, Transform, Load) process to ensure data integrity and analytical accuracy. Below are the specific steps taken:

 Handling Missing Values (Imputation Logic)
   
A multi-tiered approach was used to address null values based on business logic and statistical relevance:
•	size Column: * Accessories: Assigned a "N/A" label to maintain categorical consistency, as accessories often do not follow standard apparel sizing.
o	Apparel Categories: Imputed missing values using the Mode (most frequent value) specific to each category:
	Outerwear & Tops: Imputed as "S"
	Shoes: Imputed as "XS"
	Bottoms: Imputed as "XXL"
	Dresses: Imputed as "XL"
•	customer_rating Column: * Missing values were handled by grouping the data by Category and Brand.
o	The Mean (Average) was calculated for each group, rounded to one decimal place, and merged back into the dataset to fill gaps.
•	return_reason Column: * Null values were identified as "non-returned items."
o	These were mapped to the string "Not Returned" to differentiate them from actual return reasons (e.g., "Quality Issue").

Data Type Standardization

To enable time-intelligence and logical filtering in Power BI, the following conversions were made:
•	Date Conversion: The purchase_date was converted from a string to a proper DateTime (YYYY-MM-DD) format.
•	Boolean Transformation: The is_returned column was standardized to a Boolean (True/False) type.
•	Text Formatting: The return_reason was strictly cast as Text to ensure uniform categorization.

Data Integrity & Verification

•	Price Validation: A cross-check was performed using the formula:
current_price = original_price * (1 - markdown_percentage / 100)
The verification confirmed zero calculation discrepancies across the dataset.
•	Consistency Check: Categorical columns (brand, category) were audited for casing issues (e.g., "zara" vs "Zara"). The data was confirmed to be consistently formatted.
•	Duplicate Removal: A final check was performed on product_id to ensure each row represents a unique transaction. Duplicate rows were removed to prevent revenue double-counting.
Standardization: Used Capitalize each wors,Trim and Clean
Created columns:Discount amount,purchase year,quarter,month,day

Data Modelling

•	Fact Table: FactSales
•	Dimension Tables: DimDate,DimProduct,DimReturnReason
Relationships:
o	product_id → DimProduct
o	purchase_date → DimDate
o	Return Reason → DimReturnReason
•	Ensured star schema for efficient reporting.

DAX Measures

Created calculated fields: AvgSellingPrice = DIVIDE(SUM('FactSales'[current_price]), COUNT('FactSales'[product_id]), 0)
Return Reason Count = COUNT('FactSales'[Return Reason])
Revenue MTD = TOTALMTD([Total Revenue], 'DimDate'[Date])
Total Discount = SUM('FactSales'[Discount Amount])
Total Revenue = SUM('FactSales'[current_price])
Unit Sold = SUM(FactSales[stock_quantity])

Strategic Insights

The analysis revealed critical performance disparities across brands, categories, and colors:
Brand Performance
•	Highest Revenue: Zara dominates the top-line sales.
•	Lowest Revenue: Gap represents the smallest market share in this dataset.
Category & Discounting
•	Most Profitable: Outerwear generates the highest Net Revenue, making it the core driver of the business.
•	Discount Depth: Shoes received the highest discounts, while Accessories remained the most price-stable with the lowest discounts.
•	Underperformer: Accessories also contributed the lowest net revenue, suggesting a need for inventory re-evaluation.
Product Attributes
•	Size Trend: Size S has the highest revenue rate, indicating the primary target demographic.
•	Color Psychology: Pink fashion items are the top revenue generators, while Purple items showed the lowest performance.


