
---
![RFM Analysis](https://hivemarketingcloud.com/media/zphnp5zi/rfm-analysis-blog-graphic-01.png?center=0.55126050420168071,0.58738261801222658&mode=crop&width=730&height=467&rnd=133039200171670000)

# 📊 Project Title: Retail Customer Segmentation With RFM Analysis (Python)
Author: Nguyễn Văn Trí  



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
  Output
  |       | InvoiceNo | StockCode | Description                           | Quantity | InvoiceDate           | UnitPrice | CustomerID | Country         | Error | Day         | Month   |
  |-------|-----------|----------|---------------------------------------|----------|------------------------|-----------|------------|----------------|-------|------------|---------|
  | 125   | 536381    | 71270    | PHOTO CLIP LINE                      | 3        | 2010-12-01 09:41:00   | 1.25      | 15311.0    | United Kingdom | False | 2010-12-01 | 2010-12 |
  | 498   | 536409    | 90199C   | 5 STRAND GLASS NECKLACE CRYSTAL      | 1        | 2010-12-01 11:45:00   | 6.35      | 17908.0    | United Kingdom | False | 2010-12-01 | 2010-12 |
  | 502   | 536409    | 85116    | BLACK CANDELABRA T-LIGHT HOLDER      | 5        | 2010-12-01 11:45:00   | 2.10      | 17908.0    | United Kingdom | False | 2010-12-01 | 2010-12 |
  | 517   | 536409    | 21866    | UNION JACK FLAG LUGGAGE TAG          | 1        | 2010-12-01 11:45:00   | 1.25      | 17908.0    | United Kingdom | False | 2010-12-01 | 2010-12 |
  | 525   | 536409    | 90199C   | 5 STRAND GLASS NECKLACE CRYSTAL      | 2        | 2010-12-01 11:45:00   | 6.35      | 17908.0    | United Kingdom | False | 2010-12-01 | 2010-12 |


  ``` python
  # Check specific InvoiceNos that are duplicated
  ecommerce_retail_update[(ecommerce_retail_update['InvoiceNo'] == '536381') & (ecommerce_retail_update['StockCode'] == '71270')]
  ```
  Output
  |       | InvoiceNo | StockCode | Description       | Quantity | InvoiceDate           | UnitPrice | CustomerID | Country         | Error | Day         | Month   |
  |-------|-----------|----------|-------------------|----------|------------------------|-----------|------------|----------------|-------|------------|---------|
  | 113   | 536381    | 71270    | PHOTO CLIP LINE  | 1        | 2010-12-01 09:41:00   | 1.25      | 15311.0    | United Kingdom | False | 2010-12-01 | 2010-12 |
  | 125   | 536381    | 71270    | PHOTO CLIP LINE  | 3        | 2010-12-01 09:41:00   | 1.25      | 15311.0    | United Kingdom | False | 2010-12-01 | 2010-12 |

  

  ``` python
  ecommerce_retail_update[(ecommerce_retail_update['InvoiceNo'] == '581538') & (ecommerce_retail_update['StockCode'] == '22992')]
  ```
  Output
  |        | InvoiceNo | StockCode | Description            | Quantity | InvoiceDate           | UnitPrice | CustomerID | Country         | Error | Day         | Month   |
  |--------|-----------|----------|------------------------|----------|------------------------|-----------|------------|----------------|-------|------------|---------|
  | 541640 | 581538    | 22992    | REVOLVER WOODEN RULER  | 1        | 2011-12-09 11:34:00   | 1.95      | 14446.0    | United Kingdom | False | 2011-12-09 | 2011-12 |
  | 541692 | 581538    | 22992    | REVOLVER WOODEN RULER  | 1        | 2011-12-09 11:34:00   | 1.95      | 14446.0    | United Kingdom | False | 2011-12-09 | 2011-12 |


  ``` python
  # Retain the last instance of a row if duplicates exist with varying quantities
  ecommerce_retail_update = ecommerce_retail_update.drop_duplicates( subset=['InvoiceNo', 'StockCode', 'InvoiceDate', 'CustomerID'], keep='last')
  ```
  
  ``` python
  # Delete duplicate rows and retain only the first instance
  ecommerce_retail_update = ecommerce_retail_update.drop_duplicates(keep='first')
  ```


  ``` python
  ecommerce_retail_update
  ```
  Output
  |        | InvoiceNo | StockCode | Description                               | Quantity | InvoiceDate           | UnitPrice | CustomerID | Country         | Error | Day         | Month   |
  |--------|-----------|----------|-------------------------------------------|----------|------------------------|-----------|------------|----------------|-------|------------|---------|
  | 0      | 536365    | 85123A   | WHITE HANGING HEART T-LIGHT HOLDER       | 6        | 2010-12-01 08:26:00   | 2.55      | 17850.0    | United Kingdom | False | 2010-12-01 | 2010-12 |
  | 1      | 536365    | 71053    | WHITE METAL LANTERN                      | 6        | 2010-12-01 08:26:00   | 3.39      | 17850.0    | United Kingdom | False | 2010-12-01 | 2010-12 |
  | 2      | 536365    | 84406B   | CREAM CUPID HEARTS COAT HANGER           | 8        | 2010-12-01 08:26:00   | 2.75      | 17850.0    | United Kingdom | False | 2010-12-01 | 2010-12 |
  | 3      | 536365    | 84029G   | KNITTED UNION FLAG HOT WATER BOTTLE      | 6        | 2010-12-01 08:26:00   | 3.39      | 17850.0    | United Kingdom | False | 2010-12-01 | 2010-12 |
  | 4      | 536365    | 84029E   | RED WOOLLY HOTTIE WHITE HEART.           | 6        | 2010-12-01 08:26:00   | 3.39      | 17850.0    | United Kingdom | False | 2010-12-01 | 2010-12 |
  | ...    | ...       | ...      | ...                                       | ...      | ...                    | ...       | ...        | ...            | ...   | ...        | ...     |
  | 541904 | 581587    | 22613    | PACK OF 20 SPACEBOY NAPKINS              | 12       | 2011-12-09 12:50:00   | 0.85      | 12680.0    | France         | False | 2011-12-09 | 2011-12 |
  | 541905 | 581587    | 22899    | CHILDREN'S APRON DOLLY GIRL              | 6        | 2011-12-09 12:50:00   | 2.10      | 12680.0    | France         | False | 2011-12-09 | 2011-12 |
  | 541906 | 581587    | 23254    | CHILDRENS CUTLERY DOLLY GIRL             | 4        | 2011-12-09 12:50:00   | 4.15      | 12680.0    | France         | False | 2011-12-09 | 2011-12 |
  | 541907 | 581587    | 23255    | CHILDRENS CUTLERY CIRCUS PARADE          | 4        | 2011-12-09 12:50:00   | 4.15      | 12680.0    | France         | False | 2011-12-09 | 2011-12 |
  | 541908 | 581587    | 22138    | BAKING SET 9 PIECE RETROSPOT             | 3        | 2011-12-09 12:50:00   | 4.95      | 12680.0    | France         | False | 2011-12-09 | 2011-12 |

  **Nhận xét:**
  - Có 10038 hàng bị duplicated
  - 2 hàng chỉ khác nhau về Quantity nguyên nhân đến từ việc ngay sau khi đặt hàng KH đã bấm thay đổi số lượng nhưng do hệ thống bị lỗi hay xảy ra vấn đề nên đã lưu thành 2 đơn hàng
  - 2 hàng giống nhau hoàn toàn nguyên nhân do hệ thống bị lỗi nên dữ liệu đã bị duplicated

  </details>  
</details>




<details>
  <summary> 2. Data Processing</summary>  
  
  ``` python
  # Create Recency, Frequency, and Monetary variables
  # Take the last date as the baseline
  Lastday = ecommerce_retail_update['day'].max()
  # Create Cost column
  ecommerce_retail_update['Spend'] = ecommerce_retail_update['Quantity'] * ecommerce_retail_update['UnitPrice']
  
  # Calculate the Recency, Frequency, and Monetary values
  rfm = ecommerce_retail_update.groupby('CustomerID').agg(
                                                        Recency = ('day', lambda x: (Lastday - x.max()).days),
                                                        Frequency = ('InvoiceNo', 'nunique') ,
                                                        Monetary = ('Spend', 'sum')
                                                        ).reset_index()
  
  # Adjust data types
  rfm['Frequency'] = rfm['Frequency'].astype('int')
  rfm.head()
  ```
  Output
  | CustomerID | Recency | Frequency | Monetary |
  |------------|--------|-----------|----------|
  | 12346.0    | 325   | 1         | 77183.60 |
  | 12347.0    | 2     | 7       | 4310.00  |
  | 12348.0    | 75    | 4        | 1744.44  |
  | 12349.0    | 18    | 1        | 1757.55  |
  | 12350.0    | 310   | 1        | 334.40   |



  ``` python
  import matplotlib.pyplot as plt
  import seaborn as sns
  
  # Create a figure and a set of subplots (1 row, 3 columns)
  fig, axes = plt.subplots(1, 3, figsize=(15, 5))
  
  # Plot the distribution of Recency
  sns.boxplot(rfm['Recency'], ax=axes[0])
  axes[0].set_title('Distribution of Recency')
  
  # Plot the distribution of Frequency
  sns.boxplot(rfm['Frequency'], ax=axes[1])
  axes[1].set_title('Distribution of Frequency')
  
  # Plot the distribution of Monetary
  sns.boxplot(rfm['Monetary'], ax=axes[2])
  axes[2].set_title('Distribution of Monetary')
  
  # Adjust the layout and display the plot
  plt.tight_layout()
  plt.show()  
  ```
  Output
  <img width="1666" height="558" alt="image" src="https://github.com/user-attachments/assets/fff529f5-27ac-4fa2-b1a5-a5c95dc93b4a" />



  ``` python
  # Use qcut to create R, F, M
  rfm['R'] = pd.qcut(rfm['Recency'], 5, labels=range(5,0,-1)).astype(str)
  
  rfm['F'] = pd.cut( rfm['Frequency'],
      bins=[0, 1, 2, 4, 7, float('inf')],
      labels=[1, 2, 3, 4, 5]
  ).astype(str)
  
  rfm['M'] = pd.qcut(rfm['Monetary'], 5, labels=range(1,6)).astype(str)
  rfm['RFM'] = rfm['R'] + rfm['F'] + rfm['M']
  rfm.head()
  ```
  Output

  |       | CustomerID | Recency | Frequency | Monetary | RFM Score |
  |-------|------------|---------|-----------|----------|-----------|
  | 0     | 12346      | 325     | 1         | 77183.60 | 115       |
  | 1     | 12347      | 2       | 7         | 4310.00  | 545       |
  | 2     | 12348      | 75      | 4         | 1744.44  | 234       |
  | 3     | 12349      | 18      | 1         | 1757.55  | 414       |
  | 4     | 12350      | 310     | 1         | 334.40   | 112       |
   



  
  
  
  ``` python
  # Load data segmentation
  segmentation = pd.read_csv(path + '/segmentation.csv')
  segmentation.head()
  ```
  Output
  |       | Segment              | RFM Score                                          |
  |-------|----------------------|----------------------------------------------------|
  | 0     | Champions            | 555, 554, 544, 545, 454, 455, 445                  |
  | 1     | Loyal                | 543, 444, 435, 355, 354, 345, 344, 335             |
  | 2     | Potential Loyalist   | 553, 551, 552, 541, 542, 533, 532, 531, 452, ...   |
  | 3     | New Customers        | 512, 511, 422, 421, 412, 411, 311                  |
  | 4     | Promising            | 525, 524, 523, 522, 521, 515, 514, 513, 425, ...   |


  

  ``` python
  segmentation['RFM Score']= segmentation['RFM Score'].astype('string').str.split(',')
  segmentation = segmentation.explode('RFM Score')
  segmentation['RFM Score'] = segmentation['RFM Score'].apply(lambda x: x.replace(' ',''))
  segmentation.head()
  ```
  Output
  | Segment   | RFM Score |
  |---------- |-----------|
  | Champions | 555       |
  | Champions | 554       |
  | Champions | 544       |
  | Champions | 545       |
  | Champions | 454       |


  ``` python
  # Merge segmentation
  final_rfm = rfm_drop_outliers.merge(segmentation, left_on='RFM', right_on='RFM Score', how='left')
  final_rfm.head()
  ```
  Output
  | Index | CustomerID | Recency | Frequency | Monetary | R | F | M | RFM | Segment               | RFM Score |
  |-------|------------|--------|-----------|----------|---|---|---|-----|-----------------------|-----------|
  | 0     | 12348.0    | -75    | 27        | 1744.44  | 2 | 3 | 5 | 235 | At Risk               | 235       |
  | 1     | 12349.0    | -18    | 73        | 1757.55  | 4 | 4 | 5 | 445 | Champions             | 445       |
  | 2     | 12350.0    | -310   | 17        | 334.40   | 1 | 2 | 2 | 122 | Hibernating customers | 122       |
  | 3     | 12352.0    | -36    | 83        | 1849.49  | 4 | 4 | 5 | 445 | Champions             | 445       |
  | 4     | 12353.0    | -204   | 4         | 89.00    | 1 | 1 | 1 | 111 | Lost customers        | 111       |

  **Mô tả quá trình:**
  - Tính Recency, Frequency và Monetary của từng khách hàng.
  - Chia Recency, Frequency và Monetary thành 5 phân vị từ đó tính được điểm RFM của từng khách hàng.
  - Load file segmentation.
  - Kết hợp bảng rfm và bảng segmentation bằng điểm RFM để phân loại khách hàng theo từng CustormerID.

</details>


<details>
  <summary> 3. Visualization</summary>  
  
  <details>
    <summary> 3.1 Overall the distribution of the RFM Modelling</summary> 
    
  ``` python
  import matplotlib.pyplot as plt
  !pip install squarify
  import squarify
  ```

  ``` python
  # Create a function for wrapping text
  import textwrap
  def wrap_text(text, width=12):  # Adjust width
    return "\n".join(textwrap.wrap(text, width=width))
  ```

  ``` python
    Custom_colors = ['#1f77b4', '#aec7e8', '#ff7f0e', '#ffbb78', '#2ca02c', '#98df8a', '#d62728', '#ff9896', '#9467bd', '#c5b0d5', '#8c564b']
  ```

  ``` python
  # Users by segment
  segment_usercnt = final_rfm[['Segment','CustomerID']].groupby(['Segment']).count().reset_index().rename(columns={'CustomerID':'user_cnt'})
  # Percentage of users by segment
  segment_usercnt['volumn_percent'] = ((segment_usercnt['user_cnt']/segment_usercnt['user_cnt'].sum())*100).round(0)
  segment_usercnt['Segment'] = segment_usercnt['Segment'] + ' ' + segment_usercnt['volumn_percent'].astype(int).astype(str) + '%'
  segment_usercnt
  ```
  Output
  |       | Segment                          | User Count | Volume Percent (%) |
  |-------|-------------------------------   |------------|--------------------|
  | 0     | About To Sleep 4%                | 144        | 4.0                |
  | 1     | At Risk 12%                      | 428        | 12.0               |
  | 2     | Cannot Lose Them 3%              | 107        | 3.0                |
  | 3     | Champions 15%                    | 549        | 15.0               |
  | 4     | Hibernating customers 17%        | 612        | 17.0               |
  | 5     | Lost customers 9%                | 324        | 9.0                |
  | 6     | Loyal 10%                        | 345        | 10.0               |
  | 7     | Need Attention 5%                | 183        | 5.0                |
  | 8     | New Customers 8%                 | 298        | 8.0                |
  | 9     | Potential Loyalist 13%           | 471        | 13.0               |
  | 10    | Promising 4%                     | 151        | 4.0                |
      
  ``` python
  # Calculate the average values
  rfm_score_means = final_rfm.groupby('RFM Score').agg({
                                                        'Recency':'mean',
                                                        'Frequency':'mean',
                                                        'Monetary':'mean'
                                                        }).reset_index()

  rfm_score_means.head()
  ```
  Output

  |       | RFM Score | Recency   | Frequency | Monetary     |
  |-------|-----------|-----------|-----------|--------------|
  | 0     | 111       | 281.926829 | 1.0       | 144.334817   |
  | 1     | 112       | 277.730769 | 1.0       | 345.109567   |
  | 2     | 113       | 273.912281 | 1.0       | 627.202456   |
  | 3     | 114       | 271.863636 | 1.0       | 1341.219545  |
  | 4     | 115       | 294.000000 | 1.0       | 21277.557500 |

  

  ``` python
  # Create subplots for each metric
  fig, axes = plt.subplots(1, 3, figsize=(15, 5))
  
  # Plot histograms for each metric
  sns.histplot(rfm_score_means['Recency'], ax=axes[0], bins=10, kde=True)
  axes[0].set_title('Distribution of Recency')
  axes[0].set_xlabel('Recency')
  axes[0].set_ylabel('Custormers')
  
  sns.histplot(rfm_score_means['Frequency'], ax=axes[1], bins=10, kde=True)
  axes[1].set_title('Distribution of Frequency')
  axes[1].set_xlabel('Frequency')
  axes[1].set_ylabel('Custormers')
  
  sns.histplot(rfm_score_means['Monetary'], ax=axes[2], bins=10, kde=True)
  axes[2].set_title('Distribution of Monetary')
  axes[2].set_xlabel('Monetary')
  axes[2].set_ylabel('Custormers')
  
  # Adjust layout and display the plot
  plt.tight_layout()
  plt.show()
  ```
  Output
  <img width="1667" height="560" alt="image" src="https://github.com/user-attachments/assets/a55604b7-9771-4afd-920f-b5aa661c57a2" />



  
  ``` python
  # Use the text wrapping function to the Segment column
  segment_usercnt['Segment_wrapped'] = segment_usercnt['Segment'].apply(wrap_text)
  
  # Create treemap
  squarify.plot(sizes=segment_usercnt['user_cnt'], label=segment_usercnt['Segment_wrapped'], color=Custom_colors, alpha=0.8,text_kwargs={'fontsize': 7})
  plt.axis('off')
  plt.show()
  ```
  Output
  
  <img width="645" height="489" alt="image" src="https://github.com/user-attachments/assets/bf3e5e3c-872b-4c50-95ce-974ea75352b0" />


  </details>


  <details>
    <summary> 3.2 Distribution of RFM Modelling by time</summary>
    
  ``` python
  # Segment user by time
  rmf_customer_month = pd.merge(final_rfm, ecommerce_retail_update[['CustomerID','month']], on='CustomerID', how='left')
  rmf_customer_month = rmf_customer_month.groupby(['month','Segment']).agg({'CustomerID':'nunique'}).reset_index().rename(columns={'CustomerID':'user_cnt'})
  rmf_customer_month = rmf_customer_month.pivot_table(values='user_cnt', index='month', columns='Segment', fill_value=0)
  
  # Create area chart
  customer_month = rmf_customer_month.plot(kind='area', stacked=True, color=Custom_colors)
  plt.tight_layout()
  customer_month.legend(bbox_to_anchor=(1.05, 1), loc='upper left', fontsize='small', ncol=2)
  plt.xlabel("Month")
  plt.ylabel("Number of Customers")
  plt.title("Monthly Active Customers by RFM Segment")
  plt.show()
  ```
  Output
  <img width="1275" height="619" alt="image" src="https://github.com/user-attachments/assets/29ad72e5-3061-4d6e-afeb-b4e9b37977ff" />

  ``` python
  seg_profile = final_rfm.groupby('Segment').agg(
      Customers=('CustomerID','count'), Avg_Recency=('Recency','mean'),
      Avg_Frequency=('Frequency','mean'), Avg_Monetary=('Monetary','mean'),
      Total_Monetary=('Monetary','sum')).round(0)
  seg_profile['Pct_Customers'] = (seg_profile['Customers']/seg_profile['Customers'].sum()*100).round(1)
  seg_profile['Pct_Revenue'] = (seg_profile['Total_Monetary']/seg_profile['Total_Monetary'].sum()*100).round(1)
  seg_profile.sort_values('Total_Monetary', ascending=False)
  ```

  Output 

  | Segment             | Customers | Avg_Recency | Avg_Frequency | Avg_Monetary | Total_Monetary | Pct_Customers | Pct_Revenue |
  |---------------------|----------:|------------:|--------------:|-------------:|---------------:|--------------:|------------:|
  | Champions           | 703       | 10.0        | 14.0          | 7543.0       | 5302829.0      | 16.2          | 59.9        |
  | Loyal               | 321       | 38.0        | 6.0           | 2879.0       | 924276.0       | 7.4           | 10.4        |
  | At Risk             | 319       | 139.0       | 4.0           | 2108.0       | 672472.0       | 7.4           | 7.6         |
  | Need Attention      | 337       | 29.0        | 4.0           | 1478.0       | 498071.0       | 7.8           | 5.6         |
  | Promising           | 240       | 22.0        | 2.0           | 1548.0       | 371435.0       | 5.5           | 4.2         |
  | 5     | Hibernating customers | 685     | 135.0       | 2.0           | 409.0        | 280481.0       | 15.8          | 3.2         |
  | 6     | Potential Loyalist  | 400       | 29.0        | 3.0           | 649.0        | 259522.0       | 9.2           | 2.9         |
  | 7     | Cannot Lose Them    | 123       | 232.0       | 2.0           | 1909.0       | 234859.0       | 2.8           | 2.7         |
  | 8     | Lost customers      | 567       | 278.0       | 1.0           | 220.0        | 124483.0       | 13.1          | 1.4         |
  | 9     | About To Sleep      | 236       | 80.0        | 1.0           | 383.0        | 90336.0        | 5.4           | 1.0         |
  | 10    | New Customers       | 407       | 29.0        | 1.0           | 220.0        | 89436.0        | 9.4           | 1.0         |

   
  ``` python
  # Segment monetary by time
  rmf_monetary_month = ecommerce_retail_update.merge(final_rfm[['CustomerID','Segment']], on='CustomerID', how='inner')
  rmf_monetary_month = rmf_monetary_month.groupby(['month','Segment']).agg({'Spend':'sum'}).reset_index()
  rmf_monetary_month = rmf_monetary_month.pivot_table(values='Spend', index='month', columns='Segment', fill_value=0)
  
  # Create area chart
  monetary_month = rmf_monetary_month.plot(kind='area', stacked=True, color=Custom_colors)
  plt.ticklabel_format(axis='y', style='plain')
  plt.tight_layout()
  monetary_month.legend(bbox_to_anchor=(1.05, 1), loc='upper left', fontsize='small', ncol=2)
  plt.xlabel("Month")
  plt.ylabel("Total Spend")
  plt.title("Monthly Spend by RFM Segment")
  plt.show()
  ```
  Output 
  
  <img width="1260" height="621" alt="image" src="https://github.com/user-attachments/assets/afd9a66f-9063-4a61-942a-4cf41bc531d4" />


  </details>
</details>    


## 🔎 Final Conclusion & Recommendations  

👉🏻 Based on the insights and findings above, we would recommend the [stakeholder team] to consider the following:  
✔️ The largest customer segments are Hibernating Customers (16%), Champions (16%), and Lost Customers (13%), together accounting for nearly half of all customers.  
✔️ The Customer Segmentation by Month chart shows that the number of active customers increases significantly from September to November, reaching its peak in November. The number of new customers also grows during this period, which coincides with the year-end shopping season.   
✔️ Champions account for 16% of all customers but contribute 59.9% of total revenue. Together, Champions and Loyal customers represent 23.6% of customers and contribute around 70.3% of total revenue, showing that revenue mainly comes from these two segments.   
✔️ Lost Customers account for 13% of all customers but contribute only 1.4% of total revenue. This suggests that customers who stopped purchasing generally contributed a relatively small amount of revenue.   
✔️ The Monetary metric shows that total spending increases significantly from September to November, reaching its highest point in November. Champions contribute the largest share of spending during this period, making them an important customer segment for the business.   
     
📌 Key Takeaways:  
✔️ Prioritize retention campaigns for the "Cannot Lose Them" segment by offering personalized discounts and following up with inactive customers to encourage them to return.   
✔️ Take advantage of the increase in customers and revenue from September to November by offering seasonal promotions and vouchers to new customers, encouraging them to make repeat purchases.   
✔️ Reach out to Hibernating and At-Risk customers to understand why they have stopped purchasing regularly. Offer targeted discounts or personalized product recommendations to encourage them to return.   
