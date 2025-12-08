# 1757. Recyclable and Low Fat Products

## 📌 題目重點

- 資料表只有一張：`Products`
- 需要關注的欄位：
  - `low_fats`
  - `recyclable`
- 欄位值說明：
  - `'Y'` 表示是
  - `'N'` 表示否

✅ 本題是一題非常純粹的  
**「單表查詢 + 條件過濾（WHERE）」**

## 🧠 解題思路
條件之間是 **AND 關係**

MySQL語法:
SELECT product_id
FROM products
WHERE low_fats = 'Y'AND recyclable = 'Y';


Pandas語法:
import pandas as pd

def find_products(products: pd.DataFrame) -> pd.DataFrame:
    df = products[(products['low_fats'] == 'Y') & (products['recyclable'] == 'Y')]
    df = df[['product_id']]
    
    return df
