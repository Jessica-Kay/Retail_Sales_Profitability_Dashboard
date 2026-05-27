📘 DATA DICTIONARY

🛒 1. Orders Table
| Field Name | Data Type | Description | Example |
| --- | --- | --- | --- |
| ``Order_ID`` | Text | Unique identifier for each customer order | CA-2019-100006 |
| ``Order_Date`` | Date | Date the order was placed | 2019-01-03 |
| ``Ship_Date`` | Date | Date the order was shipped | 2019-01-05 |
| ``Ship_Mode`` | Text | Shipping method selected by customer | Second Class |
| ``Customer_ID`` | Text | Unique identifier for each customer | CG-12520 |
| ``Customer_Name`` | Text | Full name of the customer | Claire Gute |
| ``Segment`` | Text | Customer segment (Consumer, Corporate, Home Office) | Consumer |
| ``Country`` | Text | Country of the order | United States |
| ``City`` | Text | City of the customer | Los Angeles |
| ``State`` | Text | State of the customer | California |
| ``Postal_Code`` | Number | ZIP code | 90036 |
| ``Region`` | Text | Geographic region | West |
| ``Product_ID`` | Text | Unique identifier for each product | FUR-BO-10001798 |
| ``Category`` | Text | Product category | Furniture |
| ``Sub_Category`` | Text | Product sub‑category | Bookcases |
| ``Product_Name`` | Text | Name of the product | Bush Somerset Collection Bookcase |
| ``Sales`` | Currency | Revenue generated from the sale | 261.96 |
| ``Quantity`` | Number | Number of units sold | 2 |
| ``Discount`` | Decimal | Discount applied to the sale | 0.00 |
| ``Profit`` | Currency | Profit earned from the sale | 41.91 |

🔄 2. Returns Table 
| Field Name | Data Type | Description | Example |
| --- | --- | --- | --- |
| ``Order_ID`` | Text | Identifier of the returned order | CA-2019-100090 |
| ``Returned`` | Text/Boolean | Indicates whether the order was returned | Yes |

📦 3. Products Table
| Field Name | Data Type | Description | Example |
| --- | --- | --- | --- |
| ``Product_ID`` | Text | Unique product identifier | OFF-LA-10000240 |
| ``Category`` | Text | Product category | Office Supplies |
| ``Sub_Category`` | Text | Product sub‑category | Labels |
| ``Product_Name`` | Text | Full product name | Self-Adhesive Address Labels |
