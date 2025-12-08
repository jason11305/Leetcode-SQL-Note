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

> [!NOTE]
> **誤區（錯誤示範）**  
> 下面這個寫法看起來像是找「最高薪」，但其實 **沒有檢查員工的部門**：
>
> ```sql
> SELECT *
> FROM emp e
> WHERE sal IN (
>     SELECT MAX(sal)
>     FROM emp
>     GROUP BY deptno
> );
> ```
>
> 若兩個部門的最高薪剛好一樣，這個寫法會讓部門互相「誤抓」對方部門的員工。

## 🧠 解題思路

- 這題屬於常見面試題 **「找每組 group 的最大值對應的列」**。
- 我建立第三張表格，包含 <mark>每一個組別</mark> 和其對應的 <mark>最大值</mark>。



### ✔ 解法 1：在JOIN中用子查詢找每個部門的最高薪水（最經典）
MySQL語法:
```sql
SELECT 
    d.name AS Department,
    e.name AS Employee,
    e.salary AS Salary
FROM Employee e
JOIN Department d
    ON e.departmentId = d.id
WHERE e.salary = (
    SELECT MAX(salary)
    FROM Employee
    WHERE departmentId = e.departmentId
);
```sql

### ✔ 解法 2：另建一張表格再用JOIN
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
