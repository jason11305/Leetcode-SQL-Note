# 184. Department Highest Salary

## 📌 題目重點

題目給了兩張表：

### Employee
| Column | Type |
|--------|--------|
| id | int |
| name | varchar |
| salary | int |
| departmentId | int |

### Department
| Column | Type |
|--------|--------|
| id | int |
| name | varchar |

目標：  
找出 **每個部門薪水最高的員工** 

## 🧠 解題思路

- 這題屬於常見面試題 **「找每組 group 的最大值對應的列」**。
- 我建立第三張表格包含**「每一個組別」和其對應的「最大值」**。
> [!NOTE]
 **誤區**  

```
SELECT *    
FROM emp e
WHERE sal IN (
    SELECT MAX(sal)    
    FROM emp    
    GROUP BY deptno
);
```
這個寫法 **沒有檢查員工的部門 **！！
>也就是說 → 若兩個部門的最高薪一樣，高薪部門會互相「誤抓」員工。

### ✔ 解法 1：子查詢找每個部門的最高薪水（最經典）
MySQL語法:
```sql
WITH high_sal AS(
    SELECT departmentId,MAX(salary) AS dept_max
    FROM employee
    GROUP BY departmentId
)
SELECT d.name AS Department,
       e.name AS Employee,
       e.salary AS Salary
FROM employee e
JOIN high_sal h ON e.departmentId = h.departmentId
JOIN department d ON e.departmentId = d.id
WHERE e.salary = h.dept_max;
```
