# 1527. Patients With a Condition

## 📌 題目重點
- 第一個字元一定是 英文字母（a–z 或 A–Z）
- 後面可以是：英文字母（大小寫）、數字 0–9、_（底線）、.（點）、-（減號）
- 再接上@leetcode.com（而且全部小寫）

🎯 目標：  
找出 **email 格式完全符合特定規則的使用者**  


## ⚠ 常見誤區（錯誤示範）
> [!NOTE]
> - 以為 REGEXP 大小寫敏感
```sql
WHERE mail REGEXP '^[A-Za-z][A-Za-z0-9_.-]*@leetcode\\.com$';
```

## 🧠 解題思路
這題有兩件事要處理：


local-part 格式驗證 → 用 REGEXP

domain 必須完全小寫 → 用 LIKE BINARY（大小寫敏感比對）

```sql
SELECT* 
FROM Users
WHERE 
    mail REGEXP '^[a-zA-Z][a-zA-Z0-9_.-]*@leetcode\\.com$' 
    AND mail LIKE BINARY '%@leetcode.com';
```


## 🔍 Regex 表格拆解
| Pattern           | 代表意思                               |
| ----------------- | ---------------------------------- |
| `^`               | 字串開頭                               |
| `$`               | 字串結尾                               |
| `[A-Za-z]`        | 第一個字元為英文字母                         |
| `[A-Za-z0-9_.-]*` | 後續可為英文字母、數字、`_ . -`                |
| `@leetcode\.com`  | 固定字串 `@leetcode.com`（`.` 需寫成 `\.`） |
| `x\|y`            | x 或 y                              |
| `( )`             | 群組（Grouping）                       |

## 延伸：
「一般 email 格式檢查」
```sql
WHERE mail REGEXP '^[A-Za-z][A-Za-z0-9_.-]*@[A-Za-z0-9.-]+\\.[A-Za-z]{2,}$';
```
