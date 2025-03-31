
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

  <details>
    <summary> 1.1  Understand about the data (data type, data value)</summary> 
    
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
  |               | Non-Null Count | Dtype   |
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
  Output    
  (541909, 8)
  
  
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
  stockcode_check
  ```
  Output
  
  | StockCode | Count |
  |-----------|-------|
  | 85123A | 2313  |
  | 22423  | 2203  |
  | 85099B | 2159  |
  | 47566  | 1727  |
  | 20725  | 1639  |
  | ...       | ...   |
  | 84967A    | 1     |
  | 84967B    | 1     |
  | 84966B    | 1     |
  | 84966A    | 1     |
  | 72802c    | 1     |
  
  
  ``` python
  # Check data category data types of column Description
  description_check = ecommerce_retail['Description'].value_counts()
  description_check
  ```
  Output
  | Description                                   | Count |
  |-----------------------------------------------|-------|
  | WHITE HANGING HEART T-LIGHT HOLDER           | 2369  |
  | REGENCY CAKESTAND 3 TIER                     | 2200  |
  | JUMBO BAG RED RETROSPOT                      | 2159  |
  | PARTY BUNTING                                | 1727  |
  | LUNCH BAG RED RETROSPOT                      | 1638  |
  | ...                                          | ...   |
  | check?                                       | 1     |
  | SET 10 CARDS TRIANGLE ICONS 17220            | 1     |
  | SET 10 CARDS CHRISTMAS BAUBLE 16954          | 1     |
  | wrongly marked                               | 1     |
  | dotcom sales                                 | 1     |
  
  
  
  ``` python
  description_check.to_csv(path + '/description_check.csv')
  description_check_update = pd.read_csv(path + '/description_check.csv')
  
  # Add column 'Error': True if any letter is not uppercase or contains only '?
  description_check_update['Error'] = description_check_update['Description'].str.contains(r'[a-z]|\?', regex=True)
  description_check_update
  ```
  Output
  | Description                                   | Count | Error |
  |-----------------------------------------------|-------|-------|
  | WHITE HANGING HEART T-LIGHT HOLDER           | 2369  | False |
  | REGENCY CAKESTAND 3 TIER                     | 2200  | False |
  | JUMBO BAG RED RETROSPOT                      | 2159  | False |
  | PARTY BUNTING                                | 1727  | False |
  | LUNCH BAG RED RETROSPOT                      | 1638  | False |
  | ...                                          | ...   | ...   |
  | check?                                       | 1     | True  |
  | SET 10 CARDS TRIANGLE ICONS 17220            | 1     | False |
  | SET 10 CARDS CHRISTMAS BAUBLE 16954          | 1     | False |
  | wrongly marked                               | 1     | True  |
  | dotcom sales                                 | 1     | True  |
  
  
  
  ``` python
  ecommerce_retail_update = ecommerce_retail.merge(description_check_update[['Description', 'Error']], on='Description', how='left')
  print(ecommerce_retail_update[ecommerce_retail_update['Error'] == True].shape)
  print(ecommerce_retail_update.shape)
  ```
  Output   
  (3092, 9)   
  (541909, 9)
  
  
  
  ``` python
  # Check the reason for data quantity <0
  ecommerce_retail_update[ecommerce_retail_update['Quantity'] < 0].head()
  ```
  Output
  | InvoiceNo | StockCode | Description                         | Quantity | InvoiceDate           | UnitPrice | CustomerID | Country         | Error |
  |-----------|----------|-------------------------------------|----------|------------------------|-----------|------------|----------------|-------|
  | C536379   | D        | Discount                           | -1       | 2010-12-01 09:41:00   | 27.50     | 14527.0    | United Kingdom | True  |
  | C536383   | 35004C   | SET OF 3 COLOURED FLYING DUCKS    | -1       | 2010-12-01 09:49:00   | 4.65      | 15311.0    | United Kingdom | False |
  | C536391   | 22556    | PLASTERS IN TIN CIRCUS PARADE     | -12      | 2010-12-01 10:24:00   | 1.65      | 17548.0    | United Kingdom | False |
  | C536391   | 21984    | PACK OF 12 PINK PAISLEY TISSUES   | -24      | 2010-12-01 10:24:00   | 0.29      | 17548.0    | United Kingdom | False |
  | C536391   | 21983    | PACK OF 12 BLUE PAISLEY TISSUES   | -24      | 2010-12-01 10:24:00   | 0.29      | 17548.0    | United Kingdom | False |
  
  
  
  ``` python
  # Check if Quantity <0 is due to cancellation
  ecommerce_retail_update[ecommerce_retail_update['InvoiceNo'].str.startswith('C') & (ecommerce_retail_update['Quantity'] < 0)].head()
  ```
  Output
  | InvoiceNo | StockCode | Description                         | Quantity | InvoiceDate           | UnitPrice | CustomerID | Country         | Error |
  |-----------|----------|-------------------------------------|----------|------------------------|-----------|------------|----------------|-------|
  | C536379   | D        | Discount                           | -1       | 2010-12-01 09:41:00   | 27.50     | 14527.0    | United Kingdom | True  |
  | C536383   | 35004C   | SET OF 3 COLOURED FLYING DUCKS    | -1       | 2010-12-01 09:49:00   | 4.65      | 15311.0    | United Kingdom | False |
  | C536391   | 22556    | PLASTERS IN TIN CIRCUS PARADE     | -12      | 2010-12-01 10:24:00   | 1.65      | 17548.0    | United Kingdom | False |
  | C536391   | 21984    | PACK OF 12 PINK PAISLEY TISSUES   | -24      | 2010-12-01 10:24:00   | 0.29      | 17548.0    | United Kingdom | False |
  | C536391   | 21983    | PACK OF 12 BLUE PAISLEY TISSUES   | -24      | 2010-12-01 10:24:00   | 0.29      | 17548.0    | United Kingdom | False |
  
  
  
  
  ``` python
  # Check if Quantity <0 that is not due to cancellation
  ecommerce_retail_update[~ecommerce_retail_update['InvoiceNo'].str.startswith('C') & (ecommerce_retail_update['Quantity'] < 0)].head()
  ```
  Output
  | InvoiceNo | StockCode | Description | Quantity | InvoiceDate           | UnitPrice | CustomerID | Country         | Error |
  |-----------|----------|-------------|----------|------------------------|-----------|------------|----------------|-------|
  | 536589    | 21777    | <NA>        | -10      | 2010-12-01 16:50:00   | 0.0       | <NA>       | United Kingdom | NaN   |
  | 536764    | 84952C   | <NA>        | -38      | 2010-12-02 14:42:00   | 0.0       | <NA>       | United Kingdom | NaN   |
  | 536996    | 22712    | <NA>        | -20      | 2010-12-03 15:30:00   | 0.0       | <NA>       | United Kingdom | NaN   |
  | 536997    | 22028    | <NA>        | -20      | 2010-12-03 15:30:00   | 0.0       | <NA>       | United Kingdom | NaN   |
  | 536998    | 85067    | <NA>        | -6       | 2010-12-03 15:30:00   | 0.0       | <NA>       | United Kingdom | NaN   |
  
  
  
  
  
  ``` python
  # Check the reason for Unit Price <0
  ecommerce_retail_update[ecommerce_retail_update['UnitPrice'] < 0]
  ```
  Output
  | InvoiceNo | StockCode | Description        | Quantity | InvoiceDate           | UnitPrice  | CustomerID | Country         | Error |
  |-----------|----------|--------------------|----------|------------------------|------------|------------|----------------|-------|
  | A563186   | B        | Adjust bad debt    | 1        | 2011-08-12 14:51:00   | -11062.06  | <NA>       | United Kingdom | True  |
  | A563187   | B        | Adjust bad debt    | 1        | 2011-08-12 14:52:00   | -11062.06  | <NA>       | United Kingdom | True  |

**Nhận xét:**
  - Có các cột chưa đúng data type nên convert về đúng dạng
  - Có missing values ở cột Description và cột CustomerID
  - Cột Description có 3092 đơn hàng có nội dung mô tả không chính xác
  - Gần 88% các trường hợp có Quantity <0 là do bị cancle, 12% các trường hợp còn lại đến từ các lí do như: thất lạc, hư hỏng, đang kiểm tra lại hoặc chưa có thông tin và ta có thể thấy UnitPrice = 0
  - 2 trường hợp có UnitPrice < 0 là do điều chỉnh nợ xấu
  </details>

  
  <details>
    <summary> 1.2 Handle incorrect values</summary> 
     
  ``` python
  # Remove orders with Quantity <=0 (including canceled orders)
  ecommerce_retail_update = ecommerce_retail_update[ecommerce_retail_update['Quantity'] > 0]
  ecommerce_retail_update.shape
  ```
  Output  
  (531285, 9)


  ``` python
  # Remove orders with UnitPrice <=0
  ecommerce_retail_update = ecommerce_retail_update[ecommerce_retail_update['UnitPrice'] > 0]
  ecommerce_retail_update.shape
  ```
  Output  
  (530104, 9)
  </details>


  <details>
    <summary>1.3 Handle missing values</summary>  

  ``` python
  # Check missing value
  missing_values = ecommerce_retail_update.isnull().sum()
  missing_percentage = (ecommerce_retail_update.isnull().sum() / len(ecommerce_retail_update)) * 100
  missing_df = pd.DataFrame({'Missing Values': missing_values, 'Missing Percentage': missing_percentage})
  missing_df
  ```
  Output
  |        | Missing Values | Missing Percentage |
  |------------|---------------|--------------------|
  | InvoiceNo  | 0             | 0.000000%         |
  | StockCode  | 0             | 0.000000%         |
  | Description| 0             | 0.000000%         |
  | Quantity   | 0             | 0.000000%         |
  | InvoiceDate| 0             | 0.000000%         |
  | UnitPrice  | 0             | 0.000000%         |
  | CustomerID | 132220        | 24.942275%        |
  | Country    | 0             | 0.000000%         |
  | Error      | 0             | 0.000000%         |


  ``` python
  # Check the rows with missing CustomerID to understand the reason
  ecommerce_retail_update[ecommerce_retail_update['CustomerID'].isna()].head()
  ```
  Output
  |          | InvoiceNo | StockCode | Description                         | Quantity | InvoiceDate           | UnitPrice | CustomerID | Country         | Error |
  |----------|-----------|----------|-------------------------------------|----------|------------------------|-----------|------------|----------------|-------|
  |    1443  | 536544    | 21773    | DECORATIVE ROSE BATHROOM BOTTLE    | 1        | 2010-12-01 14:32:00   | 2.51      | <NA>       | United Kingdom | False |
  |    1444  | 536544    | 21774    | DECORATIVE CATS BATHROOM BOTTLE    | 2        | 2010-12-01 14:32:00   | 2.51      | <NA>       | United Kingdom | False |
  |    1445  | 536544    | 21786    | POLKADOT RAIN HAT                  | 4        | 2010-12-01 14:32:00   | 0.85      | <NA>       | United Kingdom | False |
  |    1446  | 536544    | 21787    | RAIN PONCHO RETROSPOT              | 2        | 2010-12-01 14:32:00   | 1.66      | <NA>       | United Kingdom | False |
  |    1447  | 536544    | 21790    | VINTAGE SNAP CARDS                 | 9        | 2010-12-01 14:32:00   | 1.66      | <NA>       | United Kingdom | False |


  ``` python
  # Create a month column to check for missing values by month
  ecommerce_retail_update['day'] = ecommerce_retail_update['InvoiceDate'].dt.date
  ecommerce_retail_update['month'] = ecommerce_retail_update['InvoiceDate'].dt.strftime('%Y-%m')
  ecommerce_retail_update.head()
  ```
  Output
  |       | InvoiceNo | StockCode | Description                              | Quantity | InvoiceDate           | UnitPrice | CustomerID | Country         | Error | Day         | Month   |
  |-------|-----------|----------|------------------------------------------|----------|------------------------|-----------|------------|----------------|-------|------------|---------|
  | 0     | 536365    | 85123A   | WHITE HANGING HEART T-LIGHT HOLDER      | 6        | 2010-12-01 08:26:00   | 2.55      | 17850.0    | United Kingdom | False | 2010-12-01 | 2010-12 |
  | 1     | 536365    | 71053    | WHITE METAL LANTERN                     | 6        | 2010-12-01 08:26:00   | 3.39      | 17850.0    | United Kingdom | False | 2010-12-01 | 2010-12 |
  | 2     | 536365    | 84406B   | CREAM CUPID HEARTS COAT HANGER          | 8        | 2010-12-01 08:26:00   | 2.75      | 17850.0    | United Kingdom | False | 2010-12-01 | 2010-12 |
  | 3     | 536365    | 84029G   | KNITTED UNION FLAG HOT WATER BOTTLE     | 6        | 2010-12-01 08:26:00   | 3.39      | 17850.0    | United Kingdom | False | 2010-12-01 | 2010-12 |
  | 4     | 536365    | 84029E   | RED WOOLLY HOTTIE WHITE HEART.          | 6        | 2010-12-01 08:26:00   | 3.39      | 17850.0    | United Kingdom | False | 2010-12-01 | 2010-12 |



  ``` python
  ecommerce_retail_update[ecommerce_retail_update['CustomerID'].isna()]['month'].value_counts().sort_index()
  ```
  Output
  
  | Month   | Count  |
  |---------|--------|
  | 2010-12 | 15323  |
  | 2011-01 | 13077  |
  | 2011-02 | 7178   |
  | 2011-03 | 8628   |
  | 2011-04 | 6454   |
  | 2011-05 | 7844   |
  | 2011-06 | 8792   |
  | 2011-07 | 11820  |
  | 2011-08 | 7476   |
  | 2011-09 | 9233   |
  | 2011-10 | 9750   |
  | 2011-11 | 18838  |
  | 2011-12 | 7807   |
  
  
  
  ``` python
  # Remove missing values in the CustomerID column
  ecommerce_retail_update = ecommerce_retail_update[ecommerce_retail_update['CustomerID'].notnull()]
  ecommerce_retail_update.shape
  ```
  Output  
  (397884, 11)
  
  
  **Nhận xét:**

  - Cột CustomerID có 132220 missing value (chiếm gần 25%)
  - Tháng nào cũng đều có missing value do đó cần check lại hệ thống hay quy trình lưu trữ data để khắc phục
  - Customer ID là dữ liệu quan trọng không thể thay thế nên chỉ có thể xóa đi
</details>  


  <details>
    <summary> 1.4 Handle duplicate values</summary> 
     

  ``` python
  # Check duplicate values
  duplicates_df= ecommerce_retail_update.duplicated(subset=['InvoiceNo','StockCode','InvoiceDate','CustomerID'])
  ecommerce_retail_update[duplicates_df].head()
  ```


  ``` python
  # Check duplicate values
  duplicates_df= ecommerce_retail_update.duplicated(subset=['InvoiceNo','StockCode','InvoiceDate','CustomerID'])
  ecommerce_retail_update[duplicates_df].head()
  ```
  

  ``` python
  # Check duplicate values
  duplicates_df= ecommerce_retail_update.duplicated(subset=['InvoiceNo','StockCode','InvoiceDate','CustomerID'])
  ecommerce_retail_update[duplicates_df].head()
  ```

  ``` python
  # Check duplicate values
  duplicates_df= ecommerce_retail_update.duplicated(subset=['InvoiceNo','StockCode','InvoiceDate','CustomerID'])
  ecommerce_retail_update[duplicates_df].head()
  ```

  ``` python
  # Check duplicate values
  duplicates_df= ecommerce_retail_update.duplicated(subset=['InvoiceNo','StockCode','InvoiceDate','CustomerID'])
  ecommerce_retail_update[duplicates_df].head()
  ```
  
  ``` python
  # Check duplicate values
  duplicates_df= ecommerce_retail_update.duplicated(subset=['InvoiceNo','StockCode','InvoiceDate','CustomerID'])
  ecommerce_retail_update[duplicates_df].head()
  ```
  </details>  

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
