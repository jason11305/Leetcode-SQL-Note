# 1527. Patients With a Condition

## 📌 題目重點
- 資料表只有一張：`Patients`
- 其中 conditions 是以空白分隔的病碼，例如：
  - `DIAB1`
  - `DIAB100 MYOP`
  - `ACNE DIAB100`

目標：  
找出 **有 DIAB1 這個病碼的病人** 

> [!NOTE]
> **誤區（錯誤示範）**  
> 下面這個寫法看起來像是找「DIAB1」，但其實會誤抓「DIAB10」、「SDIAB12」：
>
> ```sql
> SELECT *
> FROM Patients
> WHERE conditions LIKE '%DIAB1%';
> ```


## 🧠 解題思路
### ✔ 解法 1：
MySQL語法:
```sql
SELECT *
FROM Patients
WHERE conditions LIKE 'DIAB1%'     -- 開頭就是 DIAB1
   OR conditions LIKE '% DIAB1%';  -- 中間病碼，前面有空白
```

### ✔ 解法 2：用 Regular Expression（最精準的寫法）
MySQL語法:
```sql
SELECT *
FROM Patients
WHERE conditions REGEXP '(^| )DIAB1( |$)';
```


## 🔍 Regex 表格拆解
| Pattern | 代表意思         |
| ------- | ------------ |
| `^`     | 字串開頭         |
| `$`     | 字串結尾         |
| ` `     | 空白（space）    |
| `x\|y`  | x 或 y        |
| `( )`   | 群組（Grouping） |

Regex '(^| )DIAB1( |$)' 的意義：
- 前方只能是開頭或空白
- 後方只能是空白或結尾
- 確保 DIAB1 是獨立的 token，不會抓到 DIAB10、DIAB11
