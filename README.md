
---
![RFM Analysis](https://hivemarketingcloud.com/media/zphnp5zi/rfm-analysis-blog-graphic-01.png?center=0.55126050420168071,0.58738261801222658&mode=crop&width=730&height=467&rnd=133039200171670000)

# 📊 Project Title: Retail Customer Segmentation With RFM Analysis (Python)
Author: Nguyễn Văn Trí  
Date: 2025-01-09


---

## 📑 Table of Contents  
1. [📌 Background & Overview](#-background--overview)  
2. [📂 Dataset Description & Data Structure](#-dataset-description--data-structure)  
3. [🔎 Final Conclusion & Recommendations](#-final-conclusion--recommendations)

---

## 📌 Background & Overview  

### Objective:
### 📖 What is this project about? What Business Question will it solve?
This project uses the RFM model to help the Marketing Director segment each customer into appropriate segments to:  
✔️ Conduct marketing campaigns to show appreciation to customers for Christmas and New Year.    
✔️ Leverage potential customers.  


### 👤 Who is this project for?  
✔️ Marketing Director & Marketing Department   
✔️ Business Analysts  
✔️ Stakeholders  




---

## 📂 Dataset Description & Data Structure  

### 📌 Data Source  
- Source:  
- Format: .csv

### 📊 Data Structure & Relationships  

#### 1️. Tables Used:  
There are two tables in the dataset

#### 2️. Table Schema & Data Snapshot  

Table 1: E-commerce retail 

![image](https://github.com/user-attachments/assets/41d71a76-3798-45e7-bb0c-c896da010998)


Table 2: Segmentation

![image](https://github.com/user-attachments/assets/d51c18fd-fd23-4607-b1f3-6197610751d8)



---

## ⚒️ Main Process
<details>
<summary> 1. Exploratory Data Analysis (EDA)</summary>  

``` python
from google.colab import drive
drive.mount('/content/drive')
path = '/content/drive/MyDrive/Colab Notebooks/Project python/RFM Segmentation'

# Load Dataset
import pandas as pd
ecommerce_retail = pd.read_csv("/content/drive/MyDrive/Colab Notebooks/Project python/RFM Segmentation/ecommerce retail.csv", encoding='latin1')
ecommerce_retail.head()
```
Output

|     | InvoiceNo | StockCode | Description                        | Quantity | InvoiceDate       | UnitPrice | CustomerID | Country        |
|-----|-----------|-----------|------------------------------------|----------|-------------------|-----------|------------|----------------|
| 0   | 536365    | 85123A    | WHITE HANGING HEART T-LIGHT HOLDER | 6        | 12/1/2010 8:26    | 2.55      | 17850.0    | United Kingdom |
| 1   | 536365    | 71053     | WHITE METAL LANTERN                | 6        | 12/1/2010 8:26    | 3.39      | 17850.0    | United Kingdom |
| 2   | 536365    | 84406B    | CREAM CUPID HEARTS COAT HANGER     | 8        | 12/1/2010 8:26    | 2.75      | 17850.0    | United Kingdom |
| 3   | 536365    | 84029G    | KNITTED UNION FLAG HOT WATER BOTTLE| 6        | 12/1/2010 8:26    | 3.39      | 17850.0    | United Kingdom |
| 4   | 536365    | 84029E    | RED WOOLLY HOTTIE WHITE HEART.     | 6        | 12/1/2010 8:26    | 3.39      | 17850.0    | United Kingdom |
``` python
# Detect the data type of each column
ecommerce_retail.info()
```

Output  
| Column        | Non-Null Count | Dtype   |
|---------------|----------------|---------|
| InvoiceNo     | 541909         | object  |
| StockCode     | 541909         | object  |
| Description   | 540455         | object  |
| Quantity      | 541909         | int64   |
| InvoiceDate   | 541909         | object  |
| UnitPrice     | 541909         | float64 |
| CustomerID    | 406829         | float64 |
| Country       | 541909         | object  |   


``` python
# Convert data type
ecommerce_retail['InvoiceNo']= ecommerce_retail['InvoiceNo'].astype('string')
ecommerce_retail['StockCode']= ecommerce_retail['StockCode'].astype('string')
ecommerce_retail['Description']= ecommerce_retail['Description'].astype('string')
ecommerce_retail['InvoiceDate']= pd.to_datetime(ecommerce_retail['InvoiceDate'])
ecommerce_retail['CustomerID']= ecommerce_retail['CustomerID'].astype('string')
ecommerce_retail['Country']= ecommerce_retail['Country'].astype('string')
```

``` python
ecommerce_retail.shape
```

``` python
# Detect data value of columns
ecommerce_retail.describe()
```
Output
|           | Quantity      | InvoiceDate             | UnitPrice     |
|-----------|---------------|-------------------------|---------------|
| count     | 541909.000000 | 541909                  | 541909.000000 |
| mean      | 9.552250      | 2011-07-04 13:34:57.156 | 4.611114      |
| min       | -80995.000000 | 2010-12-01 08:26:00     | -11062.060000 |
| 25%       | 1.000000      | 2011-03-28 11:34:00     | 1.250000      |
| 50%       | 3.000000      | 2011-07-19 17:17:00     | 2.080000      |
| 75%       | 10.000000     | 2011-10-19 11:27:00     | 4.130000      |
| max       | 80995.000000  | 2011-12-09 12:50:00     | 38970.000000  |
| std       | 218.081158    | NaN                     | 96.759853     |

``` python
# Check data category data types of column StockCode
stockcode_check = ecommerce_retail['StockCode'].value_counts()
stockcode_check.head()
```
Output

| StockCode | Count |
|-----------|-------|
| **85123A** | 2313  |
| **22423**  | 2203  |
| **85099B** | 2159  |
| **47566**  | 1727  |
| **20725**  | 1639  |

``` python
ecommerce_retail.shape
```

``` python
ecommerce_retail.shape
```

``` python
ecommerce_retail.shape
```
</details>
2️ Exploratory Data Analysis (EDA)  
3️ SQL/ Python Analysis 

- First, explain codes' purpose - what they do

- Then how your query/ code & Insert screenshots of your result

- Finally, explain your observations/ findings from the results  ts findings
  
 _Describe trends, key metrics, and patterns._  

---

## 🔎 Final Conclusion & Recommendations  

👉🏻 Based on the insights and findings above, we would recommend the [stakeholder team] to consider the following:  

📌 Key Takeaways:  
✔️ Recommendation 1  
✔️ Recommendation 2  
✔️ Recommendation 3
