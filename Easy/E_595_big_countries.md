# 595. Big Countries

## 📌 題目重點
我們要找出 **「大國家」**，符合以下任一條件：
- 面積 ≥ 3,000,000  
- 或 人口 ≥ 25,000,000

## 🧠 解題思路

這題是非常典型的：
**單表查詢 + OR 條件過濾**
- **因為Pandas的多欄位輸出不熟悉，所以記錄這一題**

MySQL語法:
```sql
SELECT name, population, area
FROM world
WHERE area >= 3000000 OR population >= 25000000;
```


Pandas語法:
```Pandas
import pandas as pd

def big_countries(world: pd.DataFrame) -> pd.DataFrame:
    big_countries = world.loc[(world["area"] >= 3000000) | (world["population"] >= 25000000)]
    # #輸出多欄位，直接用逗號隔開
    return big_countries.loc[:, ["name", "population", "area"]]
```
    
