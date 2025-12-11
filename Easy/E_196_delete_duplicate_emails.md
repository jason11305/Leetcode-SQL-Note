# 196. Delete Duplicate Emails

## 📌 題目重點

題目給了一張資料表 `Person`：

| Column | Type    |
|--------|---------|
| id     | int     |
| email  | varchar |

要求：

- 刪除 `Person` 表中 **重複的 email 資料列**
- 每一組相同 email 中，**只保留 id 最小的那一筆**
- 最後留下來的表格中，每個 email 只會出現一次

---

## ⚠ 常見誤區（錯誤示範）



> [!NOTE]
> **誤區：DELETE + 子查詢沒處理好條件**
>
> 很直覺可能會寫：
>
> ```sql
> DELETE FROM Person
> WHERE id NOT IN (SELECT MIN(id) FROM Person GROUP BY email);
> ```
>
> 問題在於：
> - 有些 SQL 引擎不允許在同一張表上同時 `DELETE` 又 `SELECT` 群組（會出現錯誤）
> - 或者條件沒寫好會把全部資料刪光  
> → 實務上比較安全的寫法是用 **自連接 (self join)** 搭配比較條件。

---

## 🧠 解題思路

1. 題目要「刪掉重複 email」，只留每組中 `id` 最小那一筆
2. 也就是說：對於同一個 email：
   - 如果有 `(id = 1, email = a@example.com)`  
   - 和 `(id = 3, email = a@example.com)`  
   - 就只保留 `id = 1`，刪掉 `id = 3`
3. 最常見且安全的做法是：
   - 把 `Person` 自己 JOIN 自己
   - 用同一個 email 當連接條件
   - 刪掉「id 比另一筆大的那一邊」

---

## ✔ 解法 1：自連接刪除（MySQL 常見標準解）

MySQL 語法：

```sql
DELETE p1
FROM Person p1
JOIN Person p2
  ON p1.email = p2.email
 AND p1.id > p2.id;
```

## ✔ 解法 2：還是可以用子查詢（但要用不同的表格名稱）
MySQL 語法：

```sql
DELETE FROM person
WHERE id NOT IN (
    SELECT id 
    FROM (
        SELECT MIN(id) AS id
        FROM person
        GROUP BY email
    ) AS x
);
```
