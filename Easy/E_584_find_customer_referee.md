# 584. Find Customer Referee
題目要我們**找出所有「推薦人不是 2 的顧客」**
## 📌 題目重點

- 資料表只有一張：`Customer`
重要欄位：
- `referee_id`：推薦人 ID  
  - 可能是某個顧客的 `id`
  - 也可能是 `NULL`（代表沒有推薦人）

## 🧠 解題思路
要額外用 `IS NULL` 把「沒推薦人」補進來

MySQL語法:
```sql
SELECT name
FROM customer
WHERE referee_id !=2 OR referee_id IS NULL;
```

Pandas語法:
```Pandas
import pandas as pd

def find_customer_referee(customer: pd.DataFrame) -> pd.DataFrame:
    name = customer.loc[(customer['referee_id'] != 2) | (customer['referee_id'].isnull()), ["name"]]
      
    return name
```
