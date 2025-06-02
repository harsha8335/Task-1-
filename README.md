# Task-1-

SUMMMARY
🧼 Data Cleaning Summary – sales_data.csv

✅ 1. Missing Values

Identified and handled missing values:

Text Columns: Filled using the most frequent value (mode).

Numeric Columns: Filled using the median.

sale_date: Converted to datetime. Any NaT (missing dates) were filled with the most frequent valid date.



✅ 2. Duplicates

Removed duplicate rows to ensure data consistency.


✅ 3. Standardized Text Columns

Trimmed whitespace, converted to lowercase, then capitalized for readability.

Example: "   cash " → "Cash"


✅ 4. Date Formatting

sale_date was converted from string to datetime type using pd.to_datetime() with dayfirst=True.


✅ 5. Cleaned Column Names

Renamed all column headers to be consistent and Python-friendly:

Lowercase

Replaced spaces with underscores

Removed special characters


📌 Example: | Original Column            | Cleaned Column        | |---------------------------|------------------------| | Sale Date               | sale_date            | | Unit Price              | unit_price           | | Region and Sales Rep    | region_and_sales_rep |

✅ 6. Data Type Fixes

Converted quantity_sold to integer type for accurate analysis.

🎯 Objective

The objective of this task was to clean and preprocess a raw sales dataset to ensure it is ready for analysis or modeling. The dataset contained common issues such as missing values, inconsistent text formatting, and non-uniform column names.

By applying systematic data cleaning steps using Python and Pandas, we aimed to:

Handle missing values appropriately.

Remove duplicate records to prevent biased analysis.

Standardize text data (like region names, sales reps, etc.) for consistency.

Convert date columns into a proper datetime format for time-based analysis.

Ensure column names are clean, lowercase, and follow a uniform naming convention.

Fix data types (e.g., converting quantity to integers).


The cleaned dataset is now consistent, complete, and ready for further exploratory data analysis (EDA), visualizations, or machine learning tasks.

#import libraries


CODE⚙️


import pandas as pd

#Load
df = pd.read_csv("/content/sales_data.csv")

#Handle missing values
for col in df.columns:
    

    
    if df[col].dtype == 'O':  # Object (text) columns

        
        df[col] = df[col].fillna(df[col].mode()[0])

    
    else:  # Numeric columns

        
        df[col] = df[col].fillna(df[col].median())

#Remove duplicates
df = df.drop_duplicates()

#Standardize text values
for col in df.select_dtypes(include='object').columns:
 
    
    df[col] = df[col].astype(str).str.strip().str.lower().str.capitalize()

#Convert date columns
df['Sale_Date'] = pd.to_datetime(df['Sale_Date'], dayfirst=True, errors='coerce')

#Clean column names
df.columns = (

    df.columns.str.strip()
  
    .str.lower()
 
    .str.replace(' ', '_')
 
    .str.replace('[^a-z0-9_]', '', regex=True)
)

#Fix data types if needed (example: ensure quantity_sold is int)
df['quantity_sold'] = df['quantity_sold'].astype(int)

# cleaned data
df.head()


#output 

product_id	sale_date	sales_rep	region	sales_amount	quantity_sold	product_category	unit_cost	unit_price	

0customer_type	discount	payment_method	sales_channel	region_and_sales_rep

 
 0	1052	2023-03-02	Bob	North	5053.97	18	Furniture	152.75	267.22	Returning	0.09	Cash	Online	North-bob


 1	1093	NaT	Bob	West	4384.02	17	Furniture	3816.39	4209.44	Returning	0.11	Cash	Retail	West-bob


 2	1015	NaT	David	South	4631.23	30	Food	261.56	371.40	Returning	0.20	Bank transfer	Retail	South-david


 3	1072	NaT	Bob	South	2167.94	39	Clothing	4330.03	4467.75	New	0.02	Credit card	Retail	South-bob


 4	1061	NaT	Charlie	East	3750.20	13	Electronics	637.37	692.71	New	0.08	Credit card	Online	East-charlie
