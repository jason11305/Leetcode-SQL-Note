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

##🧠 解題思路

這題有兩件事要處理：

local-part 格式驗證 → 用 REGEXP

domain 必須完全小寫 → 用 LIKE BINARY（大小寫敏感比對）

