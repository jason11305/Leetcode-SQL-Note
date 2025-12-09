# 1527. Patients With a Condition

## 📌 題目重點
- 資料表只有一張：`Patients`
- 其中 conditions 是以空白分隔的病碼，例如：
  - `DIAB1`
  - `DIAB100 MYOP`
  - `ACNE DIAB100`

目標：  
找出**病碼前綴一定是 DIAB1**。
- 只要某個病碼是從 DIAB1 開頭的，就代表這個病人有這個 condition。

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
WHERE conditions  REGEXP '(^| )DIAB1[0-9]*($| )';
```


## 🔍 Regex 表格拆解
| Pattern | 代表意思         |
| ------- | ------------ |
| `^`     | 字串開頭         |
| `$`     | 字串結尾         |
| `x\|y`  | x 或 y        |
| ` `     | 空白（space）    |
| [[:space:]] | 任何空白字元（空格、tab、換行等所有空白類型）    |
| `( )`   | 群組（Grouping） |

Regex <mark>'(^|[[:space:]])DIAB1' </mark>的意義：
- 前面是「字串開頭」或「空白」
- 接著是 DIAB1
- 後面再接 0 個以上數字（病碼以 DIAB1 開頭）「病碼後面要嘛是字串結尾，要嘛是一個空白」

## 延伸：若要比對「獨立病碼 DIAB1」，可用：
```sql
SELECT *
FROM Patients
WHERE conditions REGEXP '(^|[[:space:]])DIAB1([[:space:]]|$)';
```
